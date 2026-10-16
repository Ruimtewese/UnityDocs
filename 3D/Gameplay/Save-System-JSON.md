# Save simple game data with JSON

Use JSON for small structured settings or progress data. Do not store passwords, secrets, or authoritative multiplayer state in a local save file.

## SaveData.cs

```csharp
using System;
using System.IO;
using UnityEngine;

[Serializable]
public sealed class SaveData
{
    public int level = 1;
    public int coins = 0;
    public float musicVolume = 0.8f;
}

public static class SaveSystem
{
    private static string SavePath =>
        Path.Combine(Application.persistentDataPath, "save.json");

    public static void Save(SaveData data)
    {
        if (data == null) throw new ArgumentNullException(nameof(data));

        string json = JsonUtility.ToJson(data, true);
        File.WriteAllText(SavePath, json);
    }

    public static SaveData Load()
    {
        if (!File.Exists(SavePath))
            return new SaveData();

        try
        {
            string json = File.ReadAllText(SavePath);
            SaveData data = JsonUtility.FromJson<SaveData>(json);
            return data ?? new SaveData();
        }
        catch (Exception exception)
        {
            Debug.LogWarning($"Could not load save: {exception.Message}");
            return new SaveData();
        }
    }
}
```

## Usage

Call `SaveSystem.Save(data)` when progress changes or at safe save points. Call `SaveSystem.Load()` at startup. For important saves, consider versioning, backup files, validation, and atomic writes.

## Limitations

Unity's JsonUtility has serialization restrictions, including limited support for dictionaries and polymorphic structures. Use a suitable serializer if your data model requires those features.
