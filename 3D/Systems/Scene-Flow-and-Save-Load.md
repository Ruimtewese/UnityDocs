# 3D Scene Flow, Pause, and JSON Save

Add every scene to Build Profiles → Scene List.

## SceneFlow3D.cs

~~~csharp
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneFlow3D : MonoBehaviour
{
    public void LoadScene(string sceneName)
    {
        if (string.IsNullOrWhiteSpace(sceneName)) return;
        Time.timeScale = 1;
        StartCoroutine(LoadRoutine(sceneName));
    }
    private IEnumerator LoadRoutine(string sceneName)
    {
        AsyncOperation op = SceneManager.LoadSceneAsync(sceneName);
        if (op == null) yield break;
        while (!op.isDone) yield return null;
    }
    public void QuitGame()
    {
        Time.timeScale = 1;
        Application.Quit();
    }
}
~~~

## PauseController3D.cs

~~~csharp
using UnityEngine;
public class PauseController3D : MonoBehaviour
{
    [SerializeField] private GameObject panel;
    public bool IsPaused { get; private set; }
    public void TogglePause() => SetPaused(!IsPaused);
    public void SetPaused(bool value)
    {
        IsPaused = value;
        Time.timeScale = value ? 0 : 1;
        if (panel) panel.SetActive(value);
        Cursor.lockState = value ? CursorLockMode.None : CursorLockMode.Locked;
        Cursor.visible = value;
    }
    private void OnDisable()
    {
        if (IsPaused) SetPaused(false);
    }
}
~~~

## SaveSystem3D.cs

~~~csharp
using System;
using System.IO;
using UnityEngine;

[Serializable]
public class SaveData3D
{
    public int version = 1;
    public float x, y, z;
    public int score;
}

public static class SaveSystem3D
{
    private static string FilePath => Path.Combine(Application.persistentDataPath, "save.json");
    public static void Save(Transform player, int score)
    {
        if (!player) return;
        Vector3 p = player.position;
        var data = new SaveData3D { x = p.x, y = p.y, z = p.z, score = score };
        File.WriteAllText(FilePath, JsonUtility.ToJson(data, true));
    }
    public static bool TryLoad(out SaveData3D data)
    {
        data = null;
        if (!File.Exists(FilePath)) return false;
        try
        {
            data = JsonUtility.FromJson<SaveData3D>(File.ReadAllText(FilePath));
            return data != null && data.version == 1;
        }
        catch (Exception e) { Debug.LogError(e); return false; }
    }
    public static void Apply(SaveData3D data, Transform player)
    {
        if (data == null || !player) return;
        player.position = new Vector3(data.x, data.y, data.z);
    }
}
~~~

Wire SceneFlow3D public methods to UI buttons. Call SaveSystem3D.Save(player, score), then TryLoad and Apply during scene startup. Reset time scale and cursor lock on every transition. Quit works only in a built player. Use persistentDataPath, stable IDs, schema migrations, and backups for production saves.
