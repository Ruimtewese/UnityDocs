# 2D Scene Flow, Pause, and JSON Save

Add all scene assets to Build Profiles → Scene List before loading them by name.

## SceneFlow2D.cs

~~~csharp
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneFlow2D : MonoBehaviour
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

## PauseController2D.cs

~~~csharp
using UnityEngine;
public class PauseController2D : MonoBehaviour
{
    [SerializeField] private GameObject panel;
    public bool IsPaused { get; private set; }
    public void TogglePause() => SetPaused(!IsPaused);
    public void SetPaused(bool value)
    {
        IsPaused = value;
        Time.timeScale = value ? 0 : 1;
        if (panel) panel.SetActive(value);
    }
    private void OnDisable()
    {
        if (IsPaused) SetPaused(false);
    }
}
~~~

## SaveSystem2D.cs

~~~csharp
using System;
using System.IO;
using UnityEngine;

[Serializable]
public class SaveData2D
{
    public int version = 1;
    public float x, y;
    public int score;
}

public static class SaveSystem2D
{
    private static string FilePath => Path.Combine(Application.persistentDataPath, "save.json");

    public static void Save(Transform player, int score)
    {
        if (!player) return;
        SaveData2D data = new SaveData2D
        {
            x = player.position.x, y = player.position.y, score = score
        };
        File.WriteAllText(FilePath, JsonUtility.ToJson(data, true));
    }

    public static bool TryLoad(out SaveData2D data)
    {
        data = null;
        if (!File.Exists(FilePath)) return false;
        try
        {
            data = JsonUtility.FromJson<SaveData2D>(File.ReadAllText(FilePath));
            return data != null && data.version == 1;
        }
        catch (Exception e) { Debug.LogError(e); return false; }
    }

    public static void Apply(SaveData2D data, Transform player)
    {
        if (data == null || !player) return;
        player.position = new Vector3(data.x, data.y, player.position.z);
    }
}
~~~

Connect SceneFlow2D methods to Button OnClick. Call SaveSystem2D.Save(player, score) from a save button; on startup call TryLoad and Apply. Application.Quit only works in a built game. For reliable production saves, add backup/temp-file handling, validation, and migrations for future schema versions. Save primitive values and stable IDs, not GameObject references.
