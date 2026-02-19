# Aimbot Mode: Silent

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Distance (Multiplier) | `silent_distance` | float | `0.1` to `10.0` | `1.0` | Scales silent movement distance toward target. | Higher multiplier can increase correction amount. |
| Delay (ms) | `silent_delay` | float | `0.001` to `300.0` ms | `100.0` | Delay before silent action executes. | Stored in milliseconds in current config value. |
| Move Delay (ms) | `silent_move_delay` | float | `0.001` to `300.0` ms | `500.0` | Delay before move-to-target phase. | Larger values slow silent sequence response. |
| Return Delay (ms) | `silent_return_delay` | float | `0.001` to `300.0` ms | `500.0` | Delay before return phase after silent move. | Tune with move delay for sequence timing. |
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Main FOV used in Silent mode. | Shared key with other main modes. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed_sec` | float | `0.1` to `2000` | `2` | Secondary Silent uses this X speed key in current UI. | No separate `silent_*_sec` keys exposed in tab UI. |
| Y-Speed | `normal_y_speed_sec` | float | `0.1` to `2000` | `2` | Secondary Silent uses this Y speed key in current UI. | No separate `silent_*_sec` keys exposed in tab UI. |
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Secondary Silent uses this FOV key in current UI. | Independent from main `fovsize`. |

## Practical tuning tips

1. Set `fovsize` first; too large a FOV makes silent activation less predictable in crowded scenes.
2. Tune timing in this order: `silent_delay` -> `silent_move_delay` -> `silent_return_delay`.
3. If action triggers too late, reduce `silent_delay` first; if movement feels sluggish, then reduce `silent_move_delay`.
4. If return phase feels unnatural, adjust `silent_return_delay` separately instead of changing all delays together.
5. Keep `silent_distance` near `1.0` initially; increase slowly only when correction is consistently too short.
6. For Secondary Silent, tune `normal_x_speed_sec`, `normal_y_speed_sec`, and `fovsize_sec` conservatively since no dedicated `silent_*_sec` UI keys are exposed.
7. Validate with multiple target distances (near, mid, far) because silent timing that works at close range may fail at longer range.
