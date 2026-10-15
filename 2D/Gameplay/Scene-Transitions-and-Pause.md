# 2D Gameplay: Scene Changes, Pause, and Restart

Add every required scene to the active Build Profile. Scene names are case-sensitive; build indices depend on the configured scene list.

## Pause controller
Requires the Input System package. Attach to a game manager and assign a pause menu GameObject. Wire Resume and RestartCurrentScene to menu buttons.

    using UnityEngine;
    using UnityEngine.InputSystem;
    using UnityEngine.SceneManagement;
    public sealed class PauseController : MonoBehaviour
    {
        [SerializeField] private GameObject pauseMenu;
        private bool paused;
        private void Start() => SetPaused(false);
        private void Update()
        {
            if (Keyboard.current != null && Keyboard.current.escapeKey.wasPressedThisFrame)
                SetPaused(!paused);
        }
        public void Resume() => SetPaused(false);
        public void RestartCurrentScene()
        {
            Time.timeScale = 1f;
            SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
        }
        private void SetPaused(bool value)
        {
            paused = value;
            Time.timeScale = paused ? 0f : 1f;
            if (pauseMenu != null) pauseMenu.SetActive(paused);
        }
        private void OnDestroy() => Time.timeScale = 1f;
    }

Time.timeScale = 0 stops scaled physics and scaled waits, but Update still runs. WaitForSeconds uses scaled time; WaitForSecondsRealtime does not. Restore timeScale before scene changes. For gamepad/touch support, use a Pause action instead of hardcoding Escape. Avoid multiple systems independently controlling global timeScale.
