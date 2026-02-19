# Aimbot Mode: Bezier

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Segments | `bezier_segments` | int | `1` to `30` | `8` | Number of segments used to sample the Bezier path. | More segments can produce smoother interpolation. |
| Ctrl X | `bezier_ctrl_x` | float | `0.0` to `100.0` | `16.0` | Horizontal control point influence for curve shape. | Works with `Ctrl Y` to bend trajectory. |
| Ctrl Y | `bezier_ctrl_y` | float | `0.0` to `100.0` | `16.0` | Vertical control point influence for curve shape. | Works with `Ctrl X` to bend trajectory. |
| Speed | `bezier_speed` | float | `0.1` to `20.0` | `1.0` | Movement speed scale along Bezier path. | Higher value moves faster along generated curve. |
| Delay (ms) | `bezier_delay` | float (seconds in config) | UI `0.1` to `50.0` ms | `0.002` s | Delay between Bezier movement steps. | UI shows milliseconds; config stores seconds. |
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Main FOV used with Bezier mode. | Shared key with other main modes. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Segments | `bezier_segments_sec` | int | `1` to `30` | `8` | Secondary Bezier path segment count. | Independent from main value. |
| Ctrl X | `bezier_ctrl_x_sec` | float | `0.0` to `100.0` | `16.0` | Secondary horizontal control point influence. | Independent from main value. |
| Ctrl Y | `bezier_ctrl_y_sec` | float | `0.0` to `100.0` | `16.0` | Secondary vertical control point influence. | Independent from main value. |
| Speed | `bezier_speed_sec` | float | `0.1` to `20.0` | `1.0` | Secondary movement speed scale. | Independent from main value. |
| Delay (ms) | `bezier_delay_sec` | float (seconds in config) | UI `0.1` to `50.0` ms | `0.002` s | Secondary delay between Bezier movement steps. | UI shows milliseconds; config stores seconds. |
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Secondary FOV used with Bezier mode. | Independent from main FOV key. |

## Practical tuning tips

1. Tune curve shape before speed: adjust `bezier_ctrl_x` and `bezier_ctrl_y` first to get the trajectory style you want.
2. Use `bezier_segments` to smooth interpolation; increase gradually because very high segments can add latency feel.
3. After shape is stable, tune `bezier_speed` for responsiveness.
4. If movement feels delayed, reduce `bezier_delay` before aggressively increasing speed.
5. Keep `fovsize` moderate while tuning path parameters so target switching noise does not mask curve behavior.
6. For secondary mode, copy main settings first, then reduce `bezier_speed_sec` slightly for safer fallback tracking.
7. Re-test after changing polling/FPS-related settings because Bezier step timing is sensitive to update cadence.
