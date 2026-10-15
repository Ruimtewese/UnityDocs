# 2D Game State: Win, Lose, and Restart Flow

Keep the current game state in one place. For a small game, an enum can distinguish Playing, Paused, Won, and Lost; this is easier to reason about than several booleans that can conflict.

## Basic state manager
Save as GameState2D.cs and attach to a game manager. Connect UI buttons to RestartCurrentScene and ReturnToMenu. Add the menu scene to the active Build Profile.

    using UnityEngine;
    using UnityEngine.SceneManagement;

    public sealed class GameState2D : MonoBehaviour
    {
        public enum State { Playing, Paused, Won, Lost }
        public State CurrentState { get; private set; } = State.Playing;

        [SerializeField] private GameObject winPanel;
        [SerializeField] private GameObject losePanel;
        [SerializeField] private string menuScene = "MainMenu";

        private void Start() => RefreshUI();

        public void Win()
        {
            if (CurrentState != State.Playing) return;
            CurrentState = State.Won;
            RefreshUI();
        }

        public void Lose()
        {
            if (CurrentState != State.Playing) return;
            CurrentState = State.Lost;
            RefreshUI();
        }

        public void RestartCurrentScene()
        {
            Time.timeScale = 1f;
            SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
        }

        public void ReturnToMenu()
        {
            Time.timeScale = 1f;
            SceneManager.LoadScene(menuScene);
        }

        private void RefreshUI()
        {
            if (winPanel != null) winPanel.SetActive(CurrentState == State.Won);
            if (losePanel != null) losePanel.SetActive(CurrentState == State.Lost);
        }
    }

This manager displays panels but does not automatically pause gameplay or stop movement. Connect a centralized pause/state-aware input gate if the character must stop after winning/losing. Avoid setting timeScale in several unrelated scripts.

## Testing checklist
Trigger Win and Lose twice, verify only the first valid result changes state, restart after pause, and test missing UI references. Decide whether pause and terminal states may coexist; if they can, model them deliberately rather than overwriting one enum value.
