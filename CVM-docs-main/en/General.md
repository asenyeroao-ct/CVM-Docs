# General Tab Parameters

Back to index: [CVM.md](./CVM.md)

## Hardware API

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Input API | `mouse_api` | enum (string) | `Serial`, `Arduino`, `SendInput`, `Net`, `KmboxA`, `MakV2`, `MakV2Binary`, `DHZ` | `Serial` | Selects mouse control backend. | MAKCU and Ferrum both use this option. |
| Auto Connect Mouse API On Startup | `auto_connect_mouse_api` | bool | `true` / `false` | `false` | Auto-connects selected mouse backend during startup. | Useful for fixed hardware setups. |
| Auto Switch Serial to 4M On Startup | `serial_auto_switch_4m` | bool | `true` / `false` | `false` | Sends Serial 4M baud handshake during startup after auto-connect. | Applies when `mouse_api=Serial` and device is connected first. |
| COM Mode | `serial_port_mode` | enum (string) | `Auto`, `Manual` | `Auto` | Serial API COM port selection mode. | Applies when `mouse_api=Serial`. |
| COM Port | `serial_port` | string | e.g. `COM3` | `""` | Manual COM port value for Serial API. | Used only in Serial + Manual mode. |
| COM Port (optional) | `arduino_port` | string | e.g. `COM4` | `""` | Arduino serial port. | Empty value allows auto-detect by backend logic. |
| Baud | `arduino_baud` | int | positive integer | `115200` | Arduino serial baud rate. | Invalid input is normalized to `115200`. |
| IP Address | `net_ip` | string | IPv4 / hostname | `192.168.2.188` | Network target for Net API. | Use with `net_port` and `net_uuid`. |
| Port | `net_port` | string | numeric string | `6234` | Network target port for Net API. | Stored as string in config. |
| UUID | `net_uuid` | string | device UUID/MAC-like value | `""` | Device identifier for Net API. | `net_mac` is a deprecated alias synchronized with `net_uuid`. |
| VID/PID | `kmboxa_vid_pid` | string | e.g. `66882021`, `6688/2021`, `V984D741`, `0x1A20/0x07E5` | `"0/0"` | Combined KmboxA device ID input used for API init. | UI uses one field; backend parses and syncs `kmboxa_vid` + `kmboxa_pid` for compatibility. |
| Port (optional) | `makv2_port` | string | e.g. `COM5` | `""` | Serial port for MakV2 API. | Empty value allows backend default detection path. |
| Baud | `makv2_baud` | int | positive integer | `4000000` | Serial baud for MakV2 API. | Invalid input is normalized to `4000000`. |
| IP Address | `dhz_ip` | string | IPv4 / hostname | `192.168.2.188` | Network target for DHZ API. | Used with UDP-style command transport. |
| Port | `dhz_port` | string | numeric string | `5000` | Network target port for DHZ API. | Stored as string in config. |
| Random Shift | `dhz_random` | int | integer `>=0` | `0` | Caesar-shift randomization parameter for DHZ commands. | Invalid input is normalized to `0`. |

## Capture Controls

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Method | runtime (`capture.mode`) | enum (string) | `NDI`, `UDP`, `CaptureCard`, `MSS` | runtime value | Selects capture backend. | Runtime state from `CaptureService`; not persisted as a config key in this table. |
| Target FPS | `target_fps` | float | `1` to `1000` | `80` | Limits tracking/processing loop frequency. | Invalid UI value is reset to `80`. |
| NDI Source | `last_ndi_source` | string or null | discovered NDI source name | `null` | Remembers preferred NDI source for reconnect. | Set when connecting selected NDI source. |
| Enable Center Crop (NDI) | `ndi_fov_enabled` | bool | `true` / `false` | `false` | Enables center square crop for NDI frames. | Crop size controlled by `ndi_fov`. |
| FOV (NDI half-size) | `ndi_fov` | int | slider `16` to `1920` | `320` | Half-size of square crop area for NDI. | Effective crop area is `2 * ndi_fov` per side. |
| UDP IP Address | `udp_ip` | string | IPv4 / hostname | `127.0.0.1` | Source address for UDP capture stream. | Fill in the second PC IP (the PC running this software). |
| UDP Port | `udp_port` | string | numeric string | `1234` | Source port for UDP capture stream. | Stored as string in config. |
| Enable Center Crop (UDP) | `udp_fov_enabled` | bool | `true` / `false` | `false` | Enables center square crop for UDP frames. | Crop size controlled by `udp_fov`. |
| FOV (UDP half-size) | `udp_fov` | int | slider `16` to `1920` | `320` | Half-size of square crop area for UDP. | Effective crop area is `2 * udp_fov` per side. |
| Device Index | `capture_device_index` | int | integer `>=0` | `0` | Capture card device index. | Used in CaptureCard mode. |
| Resolution Width | `capture_width` | int | positive integer | `1920` | Capture card width setting. | Paired with `capture_height`. |
| Resolution Height | `capture_height` | int | positive integer | `1080` | Capture card height setting. | Paired with `capture_width`. |
| FPS | `capture_fps` | float | positive number | `240` | Capture card source FPS target. | Backend/hardware may clamp actual value. |
| CaptureCard Force BGR | `capture_card_force_bgr` | bool | `true` / `false` | `true` | Forces CaptureCard frames through BGR normalization before HSV detection. | Recommended to keep enabled for consistent color detection across devices/backends. |
| CaptureCard Set Convert RGB | `capture_card_set_convert_rgb` | bool | `true` / `false` | `true` | Requests backend color conversion via `CAP_PROP_CONVERT_RGB`. | Backend support varies; unsupported backends silently ignore this request. |
| CaptureCard Probe Frames | `capture_card_probe_frames` | int | integer `>=1` | `3` | Number of probe frames used to validate each FourCC during initialization. | Failed probe triggers automatic fallback to the next preferred FourCC. |
| CaptureCard Debug Color Log | `capture_card_debug_color_log` | bool | `true` / `false` | `false` | Enables verbose frame color-format logs for CaptureCard path. | Intended for troubleshooting only due high log volume. |
| Range X | `capture_range_x` | int | minimum `128` | `128` | Capture region width baseline for CaptureCard workflow. | UI enforces minimum `128` on focus-out. |
| Range Y | `capture_range_y` | int | minimum `128` | `128` | Capture region height baseline for CaptureCard workflow. | UI enforces minimum `128` on focus-out. |
| Monitor Index | `mss_monitor_index` | int | integer `>=1` | `1` | Target monitor for MSS capture. | Value depends on local monitor list order. |
| FOV X (MSS half-width) | `mss_fov_x` | int | slider `16` to `1920` | `320` | MSS capture half-width around center. | Effective width is `2 * mss_fov_x`. |
| FOV Y (MSS half-height) | `mss_fov_y` | int | slider `16` to `1080` | `320` | MSS capture half-height around center. | Effective height is `2 * mss_fov_y`. |

## Settings

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| In-Game Sensitivity | `in_game_sens` | float | `0.1` to `20` | `0.235` | Reference in-game sensitivity value used by movement calculations. | Tune to match your game sensitivity scale. |
| Target Color | `color` | enum (string) | `yellow`, `purple`, `red`, `custom` | `purple` | Selects target color profile for HSV detection. | `custom` enables Custom HSV section. |

## Custom HSV

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| H Min | `custom_hsv_min_h` | int | `0` to `179` | `0` | Minimum hue bound for custom detection mask. | Only used when `color=custom`. |
| S Min | `custom_hsv_min_s` | int | `0` to `255` | `0` | Minimum saturation bound for custom detection mask. | Only used when `color=custom`. |
| V Min | `custom_hsv_min_v` | int | `0` to `255` | `0` | Minimum value bound for custom detection mask. | Only used when `color=custom`. |
| H Max | `custom_hsv_max_h` | int | `0` to `179` | `179` | Maximum hue bound for custom detection mask. | Only used when `color=custom`. |
| S Max | `custom_hsv_max_s` | int | `0` to `255` | `255` | Maximum saturation bound for custom detection mask. | Only used when `color=custom`. |
| V Max | `custom_hsv_max_v` | int | `0` to `255` | `255` | Maximum value bound for custom detection mask. | Only used when `color=custom`. |

## Detection Parameters

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Merge Distance | `detection_merge_distance` | int | UI slider `50` to `500` | `12` | Distance threshold for merging nearby detection rectangles. | Tooltip recommendation: `200` to `300` (tooltip default text mentions `250`). |
| Min Contour Points | `detection_min_contour_points` | int | UI slider `3` to `100` | `5` | Filters contours with too few points (noise suppression). | Tooltip recommendation: `3` to `10` (default `5`). |

## Mouse Lock

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Lock Main Aimbot X-Axis | `mouse_lock_main_x` | bool | `true` / `false` | `false` | Blocks physical X-axis movement while Main Aimbot is actively driving mouse movement. | Lock auto-releases when aimbot no longer drives movement. |
| Lock Main Aimbot Y-Axis | `mouse_lock_main_y` | bool | `true` / `false` | `false` | Blocks physical Y-axis movement while Main Aimbot is active. | Lock auto-releases when aimbot no longer drives movement. |
| Lock Sec Aimbot X-Axis | `mouse_lock_sec_x` | bool | `true` / `false` | `false` | Blocks physical X-axis movement while Sec Aimbot is active. | Lock auto-releases when aimbot no longer drives movement. |
| Lock Sec Aimbot Y-Axis | `mouse_lock_sec_y` | bool | `true` / `false` | `false` | Blocks physical Y-axis movement while Sec Aimbot is active. | Lock auto-releases when aimbot no longer drives movement. |

## Button Mask

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Button Mask | `button_mask_enabled` | bool | `true` / `false` | `false` | Enables per-button masking/locking policy. | Master switch for all button mask items below. |
| L-Click | `mask_left_button` | bool | `true` / `false` | `false` | Mask policy for left mouse button. | Internal index `0`. |
| R-Click | `mask_right_button` | bool | `true` / `false` | `false` | Mask policy for right mouse button. | Internal index `1`. |
| M-Click | `mask_middle_button` | bool | `true` / `false` | `false` | Mask policy for middle mouse button. | Internal index `2`. |
| Side 4 | `mask_side4_button` | bool | `true` / `false` | `false` | Mask policy for side mouse button 4. | Internal index `3`. |
| Side 5 | `mask_side5_button` | bool | `true` / `false` | `false` | Mask policy for side mouse button 5. | Internal index `4`. |

## Recommended Setup Order

1. Set `mouse_api` first and confirm the device can connect/reconnect reliably.
2. Pick capture method (`NDI`/`UDP`/`CaptureCard`/`MSS`) and lock a stable source resolution/FPS.
3. Set `target_fps` based on real capture throughput, not only monitor refresh rate.
4. Confirm target color (`color`) before tuning any aim/trigger logic.
5. If using `custom` color, narrow HSV range first, then fine-tune detection merge/noise thresholds.
6. Enable mouse lock or button mask only after basic tracking behavior is stable.

## Key Parameter Interactions

- `target_fps` + capture FPS: if `target_fps` is much higher than real capture FPS, control feels unstable due to repeated frames.
- `ndi_fov` / `udp_fov` / `mss_fov_x` + mode FOV (`fovsize`/`fovsize_sec`): capture crop too small can hide valid targets even when mode FOV is large.
- `color=custom` + `detection_min_contour_points`: very wide HSV plus low contour points usually increases false positives.
- `detection_merge_distance` + `trigger_min_pixels`: over-merging can inflate pixel counts and trigger too early.
- Mouse lock (`mouse_lock_*`) + activation mode (`hold_disable`/`toggle`): test release conditions carefully to avoid unexpected input blocking.

## Quick Troubleshooting

| Symptom | Check First | Then Adjust |
|---|---|---|
| Device connects on first launch but fails after restart | `auto_connect_mouse_api`, COM mode/port consistency | `serial_auto_switch_4m` and backend baud settings |
| Target color is visible but not detected | `color` profile and capture color conversion options | Custom HSV bounds, `detection_min_contour_points` |
| Too many false detections in cluttered scenes | Capture crop size and color profile selection | Increase `trigger_min_ratio`, raise contour/min-pixel related thresholds |
| Aimbot/Trigger behavior changes between modes unexpectedly | Active capture backend and current mode-dependent keys | Re-check shared keys (`fovsize`, offsets, activation keybinds) |
| Input feels delayed or inconsistent | Real capture FPS and `target_fps` mismatch | Delay/smoothing parameters in mode-specific docs |
