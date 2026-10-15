# Saving and Loading Small Amounts of Data

This example saves local progress as JSON under Application.persistentDataPath. It is not secure against tampering and should not store secrets or be trusted for competitive authority.

## Save manager
Save as LocalSaveSystem.cs and create one manager object in the first scene.

    using System;
    using System.IO;
    using UnityEngine;
    [Serializable]
    public sealed class SaveData
    {
        public int coins;
        public int highestLevel = 1;
        public float musicVolume = 1f;
    }
    public sealed class LocalSaveSystem : MonoBehaviour
    {
        public static LocalSaveSystem Instance { get; private set; }
        public SaveData Data { get; private set; } = new SaveData();
        private string SavePath => Path.Combine(Application.persistentDataPath, "save.json");
        private void Awake()
        {
            if (Instance != null && Instance != this)
            {
                Destroy(gameObject);
                return;
            }
            Instance = this;
            DontDestroyOnLoad(gameObject);
            Load();
        }
        public void Save()
        {
            try
            {
                File.WriteAllText(SavePath, JsonUtility.ToJson(Data, true));
            }
            catch (Exception exception)
            {
                Debug.LogError($"Save failed: {exception.Message}", this);
            }
        }
        public void Load()
        {
            if (!File.Exists(SavePath))
            {
                Data = new SaveData();
                return;
            }
            try
            {
                Data = JsonUtility.FromJson<SaveData>(File.ReadAllText(SavePath)) ?? new SaveData();
            }
            catch (Exception exception)
            {
                Debug.LogError($"Load failed: {exception.Message}", this);
                Data = new SaveData();
            }
        }
        private void OnApplicationPause(bool paused)
        {
            if (paused) Save();
        }
        private void OnApplicationQuit() => Save();
    }

## Limitations
JsonUtility serializes fields and supported types, not arbitrary properties or dictionaries. Add a save-version field and migration logic when the schema changes. Save at meaningful checkpoints because mobile apps may not receive a reliable quit callback. persistentDataPath differs by platform. Handle I/O errors and invalid save files.
