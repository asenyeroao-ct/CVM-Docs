# Config Tab Parameters and Workflow

Back to index: [CVM.md](./CVM.md)

## Profile Actions

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Profile Selector | `N/A` | UI state | profile names from `configs/*.json` | first available profile | Selects active profile target for load/save operations. | List is refreshed from `configs` directory. |
| SAVE | `N/A` | action | button click | idle | Saves current runtime settings to currently selected profile file. | Overwrites selected profile JSON file. |
| LOAD | `N/A` | action | button click | idle | Loads selected profile values into runtime config/UI. | Applies values immediately to UI and runtime state. |
| DEL | `N/A` | action | button click + confirmation dialog | idle | Deletes the currently selected profile file. | Requires user confirmation and is blocked when only one profile remains. |
| NEW | `N/A` | action | button click + name input flow | idle | Creates a new profile file from current settings. | Uses provided profile name as filename in `configs/`. |
| EXPORT | `N/A` | action | button click | idle | Exports selected profile JSON content to clipboard. | Shows an English confirmation message after copy. |
| IMPORT | `N/A` | action | button click + file picker | idle | Imports a selected JSON file into profile storage. | Saves imported file as a profile and applies it immediately. |
| Clipboard Import Prompt | `N/A` | automatic dialog (Config tab only) | detects CVM config in clipboard + asks Yes/No + asks profile name on Yes | idle | While the Config tab is active, valid clipboard config content triggers an import confirmation. | If user selects No, it will not ask again until clipboard content changes or current config values change. |

## Storage and Naming

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Runtime Config File | `config.json` | file path | repo root JSON file | `config.json` | Main persisted runtime configuration file. | Loaded/saved by `Config` class. |
| Profile Folder | `configs/*.json` | file path pattern | JSON profile files | `configs/` | User profile storage location. | Non-default user profiles are typically git-ignored. |
| Default Profile Template | `configs/default.json` | file path | JSON file | `configs/default.json` | Baseline profile template shipped with repo. | Keep this file stable for onboarding defaults. |
| Profile Naming Rule | `N/A` | convention | filesystem-safe profile names | suffix `_cvm.json` | UI profile names map to `*_cvm.json` filenames when saved/imported from Config tab. | UI hides the trailing `_cvm` in the selector; legacy `.json` profiles remain load-compatible. |


## Safe Profile Workflow

1. Load the nearest baseline profile before making changes.
2. Change one tab at a time, then SAVE to a new profile name (do not overwrite baseline immediately).
3. Validate in runtime, then SAVE again only if behavior is confirmed.
4. Keep a clearly named fallback profile (for example: `stable_backup`).
5. Only overwrite frequently used profiles after at least one rollback option exists.

## Naming and Versioning Conventions

| Pattern | Example | Why |
|---|---|---|
| Date + mode | `2026-02-18_normal_balanced` | Easy chronological tracking |
| Scenario tag | `scrim_trigger_safe` | Quickly identify intended use case |
| Iteration suffix | `ncaf_v3` | Clear progression when tuning repeatedly |

## Rollback and Diff Tips

- Keep `configs/default.json` as an onboarding baseline; avoid frequent edits.
- When testing risky changes, duplicate profile first (`*_test`) and compare behavior before replacing stable versions.
- If a profile suddenly behaves wrong after UI updates, reload known-good profile and re-save with the current schema.
- For team sharing, align naming conventions to reduce accidental overwrite or wrong-profile loading.
