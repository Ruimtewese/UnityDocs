# Version Control for Unity Projects

Version control protects source assets and makes changes reviewable. A small solo project benefits from Git even before multiple people collaborate.

## Project settings
In Unity, open Edit → Project Settings → Editor:
- Version Control: Visible Meta Files.
- Asset Serialization: Force Text, where supported by the project workflow.

Keep .meta files with their assets. Unity uses meta GUIDs to maintain references; deleting or losing a meta file can break references.

## What to commit
Commit Assets, Packages/manifest.json, Packages/packages-lock.json, ProjectSettings, scripts, scenes, prefabs, and important project documentation. Use the standard Unity .gitignore template so generated Library, Temp, Obj, Build, Logs, and UserSettings folders do not bloat the repository. Team-specific exceptions may apply.

## Git workflow
1. Commit a working baseline before a large change.
2. Make one coherent change at a time.
3. Inspect status/diff before committing.
4. Commit project assets and their .meta files together.
5. Pull/rebase before pushing shared changes and resolve conflicts intentionally.

## Merge conflict cautions
Scene, prefab, and ProjectSettings text conflicts require understanding both changes; don't blindly accept one side. Use UnityYAMLMerge / Smart Merge if configured for the project. Large binary assets cannot be merged like text.

## Common mistakes
- .meta files are ignored or omitted.
- Library is committed and balloons repository size.
- A scene/prefab conflict is resolved as plain text without testing in Unity.
- Package manifests are not committed, so another machine resolves different dependencies.
- Credentials, API keys, or local personal files are included in Git.
