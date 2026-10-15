# 3D Scene Loading: Synchronous vs Asynchronous

SceneManager.LoadScene loads a scene by name or build index. LoadSceneAsync is useful when a scene transition could cause a visible stall. All destination scenes must be included in the active Build Profile.

## Asynchronous loader
Save as AsyncSceneLoader.cs and attach it to a manager or transition UI.

    using System.Collections;
    using UnityEngine;
    using UnityEngine.SceneManagement;

    public sealed class AsyncSceneLoader : MonoBehaviour
    {
        private bool loading;

        public void LoadScene(string sceneName)
        {
            if (loading || string.IsNullOrWhiteSpace(sceneName)) return;
            StartCoroutine(LoadRoutine(sceneName));
        }

        private IEnumerator LoadRoutine(string sceneName)
        {
            loading = true;
            AsyncOperation operation = SceneManager.LoadSceneAsync(sceneName);
            if (operation == null)
            {
                Debug.LogError($"Could not start loading scene: {sceneName}", this);
                loading = false;
                yield break;
            }

            while (!operation.isDone)
                yield return null;

            loading = false;
        }
    }

Use a loading screen when it helps users understand a longer transition. If setting allowSceneActivation to false, be aware that progress behavior and activation rules differ; explicitly restore/allow activation when ready so loading cannot stall forever.

## Transition design
Decide which scene owns persistent managers, audio, loading UI, and player data. Prevent duplicated DontDestroyOnLoad objects. Restore Time.timeScale before loading a gameplay scene after pause.

## Common mistakes
- Scene not included in the Build Profile.
- Loading called repeatedly by multiple UI events.
- Persistent managers duplicate across scene transitions.
- Async operation is held before activation and never released.
- A loading UI references an object destroyed during the scene change.
Official API: https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.LoadSceneAsync.html
