# Main Aimbot Tab Parameters

Back to index: [CVM.md](./CVM.md)

## Core

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Aimbot | `enableaim` | bool | `true` / `false` | `true` | Master switch for Main Aimbot. | Disabling stops main aiming logic. |
| Enable Anti-Smoke | `anti_smoke_enabled` | bool | `true` / `false` | `false` | Enables smoke-aware filtering for Main Aimbot targeting. | Runtime filtering is applied independently from Secondary Aimbot anti-smoke. |
| Mode | `mode` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier`, `PID` | `Normal` | Selects Main Aimbot movement mode. | UI label for PID is `PID Controller ( RISK )`. |

## Shared Parameters

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX` | float | `-100` to `100` | `0` | Horizontal offset applied to target point. | Useful for weapon/model alignment. |
| Y-Offset | `aim_offsetY` | float | `-100` to `100` | `0` | Vertical offset applied to target point. | Useful for head/body compensation tuning. |
| Target | `aim_type` | enum (string) | `head`, `body`, `nearest` | `head` | Chooses target selection point. | Affects final aim point before movement. |
| Keybind | `selected_mouse_button` | int or string | Mouse button (`0..4`) or keyboard token | `1` | Activation bind for Main Aimbot. | Set with click-to-bind. Input is captured through current Input API state (not a fixed Win32 listener). |
| Type | `aimbot_activation_type` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | Defines how keybind controls aimbot activation. | UI labels: Hold to Enable/Disable, Toggle, Press to Enable. |

## FOV and ADS

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Base Main Aimbot FOV radius. | Always active when Main Aimbot is enabled. |
| Enable ADS FOV | `ads_fov_enabled` | bool | `true` / `false` | `false` | Enables ADS-specific FOV override. | ADS controls are placed in the FOV section. |
| ADS FOV Size | `ads_fovsize` | float | `1` to `1000` | `100` | FOV radius used while ADS key is pressed. | Slider appears when ADS FOV is enabled. |
| ADS Keybind | `ads_key` | string | Mouse button name or keyboard token | `Right Mouse Button` | Bind used to switch from base FOV to ADS FOV. | Set with click-to-bind using current Input API state. |

## Mode Docs

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)
- [PID](./aimbot/pid.md)

## Main and Secondary Role Split (Recommended)

| Goal | Main Aimbot (`Main Aimbot`) | Secondary (`Sec Aimbot`) |
|---|---|---|
| Primary fighting behavior | More direct and faster mode | More conservative fallback mode |
| Typical target strategy | `head` or `nearest` based on game style | `body` or slightly larger FOV for stability |
| Keybind design | Main engagement key (often RMB) | Separate key to avoid overlap |
| Smoothing/speed style | Responsiveness first | Stability first |

## Activation Type Behavior

| Activation Type | Behavior | Practical Use |
|---|---|---|
| `hold_enable` | Active only while key is held | Most predictable for manual control |
| `hold_disable` | Active by default, disabled while key is held | Useful for temporary disengage windows |
| `toggle` | Press once to enable, press again to disable | Good for long tracking phases |
| `use_enable` | Uses enable-style press behavior from UI implementation | Keep keybind conflicts minimal when using this mode |

## Conflict Avoidance Checklist

1. Do not assign the same keybind to main and secondary unless intentionally testing override behavior.
2. Keep one mode as "aggressive" and the other as "stable" to avoid duplicated movement intent.
3. When both are enabled, verify `fovsize` and `fovsize_sec` are not unintentionally identical in crowded scenes.
4. After changing `in_game_sens`, retune offsets and speeds before changing advanced mode parameters.
5. Re-test with anti-smoke on/off separately because filtering can change target continuity.
