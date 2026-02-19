# Sec Aimbot Tab Parameters

Back to index: [CVM.md](./CVM.md)

## Core

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Sec Aimbot | `enableaim_sec` | bool | `true` / `false` | `false` | Master switch for Secondary Aimbot. | Secondary aimbot runs when its activation logic is satisfied. |
| Enable Anti-Smoke | `anti_smoke_enabled_sec` | bool | `true` / `false` | `false` | Enables smoke-aware filtering for Secondary Aimbot targeting. | Runtime filtering is applied independently from Main Aimbot anti-smoke. |
| Mode | `mode_sec` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier` | `Normal` | Selects Secondary Aimbot movement mode. | Mode-specific parameters are documented in mode files below. |

## Shared Parameters

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX_sec` | float | `-100` to `100` | `0` | Horizontal offset for Secondary Aimbot target point. | Applied after target selection. |
| Y-Offset | `aim_offsetY_sec` | float | `-100` to `100` | `0` | Vertical offset for Secondary Aimbot target point. | Applied after target selection. |
| Target | `aim_type_sec` | enum (string) | `head`, `body`, `nearest` | `head` | Secondary target selection strategy. | Independent from main `aim_type`. |
| Keybind | `selected_mouse_button_sec` | int or string | Mouse button (`0..4`) or keyboard token | `2` | Activation bind for Secondary Aimbot. | Set with click-to-bind. Input is captured through current Input API state (not a fixed Win32 listener). |
| Type | `aimbot_activation_type_sec` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | Defines how keybind controls secondary activation. | UI labels: Hold to Enable/Disable, Toggle, Press to Enable. |

## FOV and ADS

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Base Secondary Aimbot FOV radius. | Always active when Secondary Aimbot is enabled. |
| Enable ADS FOV | `ads_fov_enabled_sec` | bool | `true` / `false` | `false` | Enables ADS-specific FOV override for secondary mode. | ADS controls are placed in the FOV section. |
| ADS FOV Size | `ads_fovsize_sec` | float | `1` to `1000` | `150` | FOV radius used while secondary ADS key is pressed. | Slider appears when ADS FOV is enabled. |
| ADS Keybind | `ads_key_sec` | string | Mouse button name or keyboard token | `Right Mouse Button` | Bind used to switch from base secondary FOV to ADS FOV. | Set with click-to-bind using current Input API state. |

## Important Silent-Mode Note

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Sec Silent Parameter Set | `normal_x_speed_sec`, `normal_y_speed_sec`, `fovsize_sec` | mixed | see mode docs | `2`, `2`, `150` | In current UI implementation, Sec Silent uses secondary speed + FOV keys instead of dedicated silent delay keys. | This is expected behavior in current build. |

## Mode Docs

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)

## Coordination With Main Aimbot

1. Keep `selected_mouse_button_sec` different from main keybind unless you intentionally want shared activation.
2. Start with secondary speed/smoothing lower than main to reduce sudden direction conflicts.
3. Use secondary as a contingency profile: wider `fovsize_sec`, safer target type, slower motion.
4. If switching feels inconsistent, test each activation type independently before combining.
5. Re-validate secondary behavior after changing any shared capture/color settings in `General`.

## Secondary Baseline Presets

| Intent | Suggested Starting Point |
|---|---|
| Stable fallback tracking | `aim_type_sec=body`, slightly lower speed, higher smoothing |
| Fast close-range assist | `aim_type_sec=nearest`, medium speed, moderate smoothing |
| Low-noise mixed scenes | Keep `fovsize_sec` moderate and avoid very large offsets |

## Silent-Mode Clarification

Secondary Silent currently relies on `normal_x_speed_sec`, `normal_y_speed_sec`, and `fovsize_sec` from the existing UI path. If behavior looks different from Main Silent, verify these three keys first before adjusting other silent timing values.
