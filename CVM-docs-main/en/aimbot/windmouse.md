# Aimbot Mode: WindMouse

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Gravity | `wm_gravity` | float | `0.1` to `30.0` | `9.0` | Main attraction force toward target. | Higher values pull trajectory more directly. |
| Wind | `wm_wind` | float | `0.1` to `20.0` | `3.0` | Main random wind component strength. | Higher values make trajectory less linear. |
| Max Step | `wm_max_step` | float | `1` to `100` | `15.0` | Main maximum step size per update. | Caps fast movement bursts. |
| Min Step | `wm_min_step` | float | `0.1` to `20` | `2.0` | Main minimum movement step size. | Keeps movement from stalling. |
| Min Delay (ms) | `wm_min_delay` | float (seconds in config) | UI `0.1` to `50` ms | `0.001` s | Minimum delay between windmouse updates. | UI shows milliseconds; config stores seconds. |
| Max Delay (ms) | `wm_max_delay` | float (seconds in config) | UI `0.1` to `50` ms | `0.003` s | Maximum delay between windmouse updates. | UI shows milliseconds; config stores seconds. |
| Distance Threshold | `wm_distance_threshold` | float | `10` to `200` | `50.0` | Distance threshold for behavior switching. | Often used to transition near-target behavior. |
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Main FOV used with WindMouse mode. | Shared key with other main modes. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Gravity | `wm_gravity_sec` | float | `0.1` to `30.0` | `9.0` | Secondary attraction force toward target. | Independent from main value. |
| Wind | `wm_wind_sec` | float | `0.1` to `20.0` | `3.0` | Secondary random wind component strength. | Independent from main value. |
| Max Step | `wm_max_step_sec` | float | `1` to `100` | `15.0` | Secondary maximum step size per update. | Independent from main value. |
| Min Step | `wm_min_step_sec` | float | `0.1` to `20` | `2.0` | Secondary minimum movement step size. | Independent from main value. |
| Min Delay (ms) | `wm_min_delay_sec` | float (seconds in config) | UI `0.1` to `50` ms | `0.001` s | Secondary minimum delay. | UI shows milliseconds; config stores seconds. |
| Max Delay (ms) | `wm_max_delay_sec` | float (seconds in config) | UI `0.1` to `50` ms | `0.003` s | Secondary maximum delay. | UI shows milliseconds; config stores seconds. |
| Distance Threshold | `wm_distance_threshold_sec` | float | `10` to `200` | `50.0` | Secondary distance threshold for behavior switching. | Independent from main value. |
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Secondary FOV used with WindMouse mode. | Independent from main FOV key. |

## Practical tuning tips

1. Start by balancing force terms: set `wm_gravity` first, then add `wm_wind` until movement looks natural but still controllable.
2. If path is too random, lower `wm_wind`; if path is too rigid, increase `wm_wind` in small steps.
3. Constrain movement with `wm_max_step`/`wm_min_step`: reduce max step for stability, increase min step if motion stalls near target.
4. Keep timing tight by setting `wm_min_delay` and `wm_max_delay` close together; wide delay gaps often create inconsistent feel.
5. Use `wm_distance_threshold` to control behavior transition range; raise it if close-range behavior starts too late.
6. Tune `fovsize` and `fovsize_sec` independently to avoid secondary mode stealing targets from main mode.
7. For secondary settings (`*_sec`), begin with the same shape but lower aggressiveness (slightly lower gravity and max step).
