# 2D Save and Load with JSON

Use Application.persistentDataPath for small local progress. This example intentionally stores only simple fields. It is not secure storage; players can edit local save files.

## Data model and system
Save as LocalProgressSave.cs.

    using System;
    using System.IO;
    using UnityEngine;

    [Serializable]
    public sealed class LocalProgressData
    {
        public int coins;
        public int bestScore;
        public int unlockedLevel = 1;
    }

    public sealed class LocalProgressSave : MonoBehaviour
    {
        [SerializeField] private string fileName = "progress.json";
        public LocalProgressData Data { get; private set; } = new LocalProgressData();
        private string PathToSave => Path.Combine(Application.persistentDataPath, fileName);

        public void Save()
        {
            try
            {
                string json = JsonUtility.ToJson(Data, true);
                File.WriteAllText(PathToSave, json);
            }
            catch (Exception e)
            {
                Debug.LogError($"Could not save progress: {e.Message}", this);
            }
        }

        public void Load()
        {
            if (!File.Exists(PathToSave))
            {
                Data = new LocalProgressData();
                return;
            }

            try
            {
                Data = JsonUtility.FromJson<LocalProgressData>(File.ReadAllText(PathToSave))
                       ?? new LocalProgressData();
            }
            catch (Exception e)
            {
                Debug.LogError($"Could not load progress: {e.Message}", this);
                Data = new LocalProgressData();
            }
        }

        private void OnApplicationPause(bool paused)
        {
            if (paused) Save();
        }
    }

Place one instance in a scene and call Load at startup and Save after meaningful progress changes. Mobile platforms may not call quit reliably, so don't depend only on OnApplicationQuit. Avoid using arbitrary player-controlled file names without validation.

## Serialization limits
JsonUtility handles serializable fields and supported types, but not arbitrary dictionaries or properties. Add a schema version and migration logic if the data format changes. Use stable item IDs rather than serializing direct references to scene objects.

## Testing
Test a first run with no file, normal save/load, invalid/corrupt JSON, and values at boundary cases. Local files are not appropriate for secrets or authoritative multiplayer currency.
