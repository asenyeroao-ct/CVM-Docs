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
4. Per-axis output is effectively:
   `output = clamp((Kp * err + Ki * integral + Kd * derivative) * axis_speed, -Max Output, Max Output)`.
5. `Kp Min`/`Kp Max` are auto-corrected if entered in reverse order.

## Detailed PID tuning workflow

1. Prepare a stable baseline before tuning gains.
   - Set `X Speed` and `Y Speed` to `1.0` first, then tune gains.
   - Use a moderate `Max Output` (for example `30` to `60`) to avoid extreme spikes while tuning.
   - Keep FOV fixed during tuning so gain behavior is comparable across attempts.
2. Start with fixed-Kp behavior.
   - Set `Kp Min = Kp Max`.
   - Set `Ki = 0` and use a small `Kd` (for example `0.05` to `0.15`).
   - Increase Kp until tracking becomes fast enough, then back off slightly if oscillation starts.
3. Add damping with `Kd`.
   - Increase `Kd` in small steps when you see overshoot or left-right shaking.
   - If motion becomes sluggish or noisy, reduce `Kd` a bit.
4. Add `Ki` last to remove persistent bias.
   - Increase `Ki` slowly; large `Ki` can cause drifting or delayed oscillation.
   - If crosshair keeps "pushing" after error is near zero, reduce `Ki`.
5. Re-enable dynamic Kp range.
   - Keep `Kp Min` lower for near-center stability.
   - Raise `Kp Max` for faster large-error correction.
   - Typical pattern: `Kp Min` around `50%` to `80%` of `Kp Max`, then refine by feel.
6. Re-tune outer parameters.
   - Raise `Max Output` if movement saturates too early.
   - Lower `Max Output` if bursts are too harsh.
   - Adjust `X Speed`/`Y Speed` only after gain tuning, to correct axis imbalance.

## Symptom-to-adjustment guide

| Symptom | Most likely cause | What to adjust first |
|---|---|---|
| Crosshair oscillates around target | `Kp Max` too high or `Kd` too low | Lower `Kp Max`, then increase `Kd` slightly |
| Fast snap then bounce-back | Clamp too loose + insufficient damping | Lower `Max Output` or raise `Kd` |
| Slow to catch far targets | `Kp Max` too low or `Max Output` too low | Increase `Kp Max`, then `Max Output` |
| Near-center micro jitter | `Kp Min` too high / `Kd` too sensitive | Lower `Kp Min`, reduce `Kd` slightly |
| Steady small offset not corrected | `Ki` too low (or zero) | Increase `Ki` a little |
| Drift or delayed wobble after tracking | `Ki` too high | Reduce `Ki` |

## Implementation details that affect tuning

1. `Ki` and `Kd` changes reset PID internal state (integral/history) for that aim mode.
2. Integral term is limited by `Max Output / |Ki|` when `Ki != 0`, so `Ki` and `Max Output` are coupled.
3. `FOV Size` directly changes how quickly Kp moves from `Kp Min` to `Kp Max` by distance.
4. Main and secondary PID settings are fully independent; tune `*_sec` separately.
