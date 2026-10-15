# 3D Game State: Win, Lose, and Restart Flow

A single state owner makes game flow predictable. An enum can represent Playing, Paused, Won, and Lost; separate booleans can otherwise create conflicting combinations.

## State manager
Save as GameState3D.cs. Assign win/lose panels and the main menu scene in Inspector; include the scene in the active Build Profile.

    using UnityEngine;
    using UnityEngine.SceneManagement;

    public sealed class GameState3D : MonoBehaviour
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

This manages state and panel visibility only. Gate player input/AI and decide whether time should pause on win/loss in one central place. Avoid letting multiple components independently set Time.timeScale.

## Testing checklist
Call Win/Lose more than once, restart after a pause, test missing UI references, and verify scene inclusion. Decide explicitly whether pause may coexist with a terminal state.
