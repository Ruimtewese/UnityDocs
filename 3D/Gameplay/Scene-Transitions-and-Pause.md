# 3D Scene Transitions and Pause

Add every scene to the active Build Profile. Prefer explicit scene names or build indices that are configured intentionally. The example uses Escape and requires the Input System package.

## Pause and restart manager
Save as GameFlow3D.cs. Assign the pause menu object and wire menu buttons to Resume, LoadMenu, or Restart.

    using UnityEngine;
    using UnityEngine.InputSystem;
    using UnityEngine.SceneManagement;

    public sealed class GameFlow3D : MonoBehaviour
    {
        [SerializeField] private GameObject pauseMenu;
        [SerializeField] private string menuSceneName = "MainMenu";
        private bool paused;

        private void Start() => SetPaused(false);

        private void Update()
        {
            if (Keyboard.current != null &&
                Keyboard.current.escapeKey.wasPressedThisFrame)
                SetPaused(!paused);
        }

        public void Resume() => SetPaused(false);

        public void Restart()
        {
            Time.timeScale = 1f;
            SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
        }

        public void LoadMenu()
        {
            Time.timeScale = 1f;
            SceneManager.LoadScene(menuSceneName);
        }

        private void SetPaused(bool value)
        {
            paused = value;
            Time.timeScale = paused ? 0f : 1f;
            if (pauseMenu != null) pauseMenu.SetActive(paused);
        }

        private void OnDestroy() => Time.timeScale = 1f;
    }

## Design rules
- Centralize pause ownership so multiple menus do not overwrite timeScale.
- Scaled physics/coroutines pause at timeScale zero; Update and unscaled systems can continue.
- Restore timeScale before loading another scene.
- Use Input Actions for gamepad/mobile compatibility.
- Ensure the pause UI still receives input and is not disabled with gameplay.

Common issues: scene omitted from Build Profile, incorrect menu name, pause state left at zero between scenes, or a coroutine waits on scaled time while paused.
