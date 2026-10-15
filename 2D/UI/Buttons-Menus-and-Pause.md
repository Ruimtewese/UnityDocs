# 2D UI: Buttons, Main Menus, and Pause Menus

This guide uses uGUI and TextMeshPro. Create a Canvas, Button objects, EventSystem, and TMP text for labels. For responsive menus, use layout groups and a Canvas Scaler configured for Scale With Screen Size.

## Scene loader
Save as SceneMenuActions.cs, attach to a UI manager, and connect button On Click events in the Inspector.

    using UnityEngine;
    using UnityEngine.SceneManagement;

    public sealed class SceneMenuActions : MonoBehaviour
    {
        public void LoadSceneByName(string sceneName)
        {
            if (string.IsNullOrWhiteSpace(sceneName))
            {
                Debug.LogError("Scene name is empty.", this);
                return;
            }
            SceneManager.LoadScene(sceneName);
        }

        public void QuitGame()
        {
            Application.Quit();
#if UNITY_EDITOR
            UnityEditor.EditorApplication.isPlaying = false;
#endif
        }
    }

The UnityEditor code is excluded from player builds by the preprocessor directive. In the Inspector, choose the method in Button → On Click and enter the exact scene name. Ensure that scene is included in the active Build Profile.

## Pause menu
A pause menu usually sets Time.timeScale to 0 and restores it to 1 on resume. Keep the pause state owned by one game-flow component; independently toggling timeScale from many scripts creates conflicts. UI can still receive input while timeScale is zero.

## Accessibility and layout
- Provide visible keyboard/controller focus where applicable.
- Do not encode every state using color alone.
- Use button labels that describe the action.
- Test multiple aspect ratios and UI scale factors.
- Ensure EventSystem input module matches the configured input system.

Troubleshooting: if a button does not fire, check EventSystem, input module, disabled/intercepting CanvasGroup, raycast overlap, and On Click listeners. If it fails only in a build, inspect scene inclusion and platform-specific code.
