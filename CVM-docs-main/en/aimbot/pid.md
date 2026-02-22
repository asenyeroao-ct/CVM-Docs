# Aimbot Mode: PID

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Kp Min | `pid_kp_min` | float | `0.0` to `20.0` | `3.7` | Lower bound of proportional gain. | Used when target is near crosshair center. |
| Kp Max | `pid_kp_max` | float | `0.0` to `20.0` | `3.7` | Upper bound of proportional gain. | Used as distance to center increases. |
| Ki | `pid_ki` | float | `0.0` to `100.0` | `24.0` | Integral gain. | Compensates persistent tracking error. |
| Kd | `pid_kd` | float | `0.0` to `10.0` | `0.11` | Derivative gain. | Damps sudden error changes. |
| Max Output | `pid_max_output` | float | `1.0` to `200.0` | `50.0` | Output clamp per axis. | Limits overshoot and unstable spikes. |
| X Speed | `pid_x_speed` | float | `0.1` to `5.0` | `1.0` | X-axis output multiplier. | Lets horizontal speed differ from Y axis. |
| Y Speed | `pid_y_speed` | float | `0.1` to `5.0` | `1.0` | Y-axis output multiplier. | Lets vertical speed differ from X axis. |
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Main FOV used with PID mode. | Also used for dynamic Kp interpolation. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Kp Min | `pid_kp_min_sec` | float | `0.0` to `20.0` | `3.7` | Secondary lower bound of proportional gain. | Independent from main value. |
| Kp Max | `pid_kp_max_sec` | float | `0.0` to `20.0` | `3.7` | Secondary upper bound of proportional gain. | Independent from main value. |
| Ki | `pid_ki_sec` | float | `0.0` to `100.0` | `24.0` | Secondary integral gain. | Independent from main value. |
| Kd | `pid_kd_sec` | float | `0.0` to `10.0` | `0.11` | Secondary derivative gain. | Independent from main value. |
| Max Output | `pid_max_output_sec` | float | `1.0` to `200.0` | `50.0` | Secondary output clamp per axis. | Independent from main value. |
| X Speed | `pid_x_speed_sec` | float | `0.1` to `5.0` | `1.0` | Secondary X-axis output multiplier. | Independent from main value. |
| Y Speed | `pid_y_speed_sec` | float | `0.1` to `5.0` | `1.0` | Secondary Y-axis output multiplier. | Independent from main value. |
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Secondary FOV used with PID mode. | Also used for dynamic Kp interpolation. |

## Notes

1. In the UI, this mode is labeled as `PID Controller ( RISK )`.
2. Kp is interpolated by distance-to-center:
   `Kp = Kp Min + (Kp Max - Kp Min) * clamp(distance_to_center / FOV, 0, 1)`.
3. If `Kp Min` equals `Kp Max`, behavior is equivalent to fixed-Kp PID.
4. Start with small `Kp Max`, tune `Kd`, and increase `Ki` last.
5. If movement oscillates, reduce `Kp Max` or increase `Kd`.
