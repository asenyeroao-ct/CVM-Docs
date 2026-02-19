# Aimbot Mode: NCAF

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Alpha (Speed Curve) | `ncaf_alpha` | float | `0.1` to `5.0` | `1.5` | Curve shape parameter for NCAF speed response. | Higher alpha changes speed ramp profile. |
| Snap Boost Factor | `ncaf_snap_boost` | float | `0.01` to `2.0` | `0.3` | Extra boost near snap engagement conditions. | Can make lock-on more aggressive. |
| Max Step | `ncaf_max_step` | float | `1` to `200` | `50.0` | Per-update maximum movement step. | Caps sudden movement jumps. |
| Min Speed Multiplier | `ncaf_min_speed_multiplier` | float | `0.01` to `1.0` | `0.01` | Lower bound for adaptive speed multiplier. | Prevents movement from becoming too slow. |
| Max Speed Multiplier | `ncaf_max_speed_multiplier` | float | `1.0` to `20.0` | `10.0` | Upper bound for adaptive speed multiplier. | Prevents movement from becoming too fast. |
| Prediction Interval (ms) | `ncaf_prediction_interval` | float (seconds in config) | UI `1` to `100` ms | `0.016` s | Target prediction interval for NCAF timing. | UI shows milliseconds; config stores seconds. |
| Snap Radius (Outer) | `ncaf_snap_radius` | float | `10` to `500` | `150.0` | Outer engagement radius for snap behavior. | Should be larger than near radius. |
| Near Radius (Inner) | `ncaf_near_radius` | float | `5` to `400` | `50.0` | Inner precision radius for near-target handling. | Should stay smaller than snap radius. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Alpha (Speed Curve) | `ncaf_alpha_sec` | float | `0.1` to `5.0` | `1.5` | Secondary NCAF curve shape parameter. | Independent from main value. |
| Snap Boost Factor | `ncaf_snap_boost_sec` | float | `0.01` to `2.0` | `0.3` | Secondary snap boost parameter. | Independent from main value. |
| Max Step | `ncaf_max_step_sec` | float | `1` to `200` | `50.0` | Secondary per-update max movement step. | Independent from main value. |
| Min Speed Multiplier | `ncaf_min_speed_multiplier_sec` | float | `0.01` to `1.0` | `0.01` | Secondary lower speed multiplier bound. | Independent from main value. |
| Max Speed Multiplier | `ncaf_max_speed_multiplier_sec` | float | `1.0` to `20.0` | `10.0` | Secondary upper speed multiplier bound. | Independent from main value. |
| Prediction Interval (ms) | `ncaf_prediction_interval_sec` | float (seconds in config) | UI `1` to `100` ms | `0.016` s | Secondary prediction interval. | UI shows milliseconds; config stores seconds. |
| Snap Radius (Outer) | `ncaf_snap_radius_sec` | float | `10` to `500` | `150.0` | Secondary outer snap radius. | Should be larger than secondary near radius. |
| Near Radius (Inner) | `ncaf_near_radius_sec` | float | `5` to `400` | `50.0` | Secondary inner near radius. | Should stay smaller than secondary snap radius. |

## Practical tuning tips

1. Lock geometry first: choose `ncaf_snap_radius` and `ncaf_near_radius` with a clear gap (`near < snap`).
2. Tune movement cap next: reduce `ncaf_max_step` if motion looks jumpy before changing multipliers.
3. Then shape speed response with `ncaf_alpha`; small changes (`0.1` to `0.2`) are usually enough.
4. Use multipliers as safety bounds: raise `ncaf_min_speed_multiplier` only if close tracking stalls, lower `ncaf_max_speed_multiplier` if bursts are too aggressive.
5. Adjust `ncaf_snap_boost` last; too high values can cause sudden snapping near threshold transitions.
6. `ncaf_prediction_interval` should roughly match your frame rhythm; if prediction feels unstable, lower it gradually.
7. Mirror the same tuning order for `*_sec` keys, but keep secondary settings slightly less aggressive for stable fallback behavior.
