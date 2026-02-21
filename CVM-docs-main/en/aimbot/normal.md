# Aimbot Mode: Normal

Back: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed` | float | `0.1` to `2000` | `3` | Main horizontal tracking speed. | Higher values react faster. |
| Y-Speed | `normal_y_speed` | float | `0.1` to `2000` | `3` | Main vertical tracking speed. | Balance with X-Speed for stable tracking. |
| Smoothing | `normalsmooth` | float | `1` to `30` | `30` | Main movement smoothing intensity. | Higher values generally feel steadier/slower. |
| FOV Size | `fovsize` | float | `1` to `1000` | `100` | Main aiming field-of-view size. | Controls candidate area size. |
| FOV Smooth | `normalsmoothfov` | float | `1` to `30` | `30` | Main FOV transition smoothing. | Helps reduce abrupt FOV edge behavior. |

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed_sec` | float | `0.1` to `2000` | `2` | Secondary horizontal tracking speed. | Independent from main speed. |
| Y-Speed | `normal_y_speed_sec` | float | `0.1` to `2000` | `2` | Secondary vertical tracking speed. | Independent from main speed. |
| Smoothing | `normalsmooth_sec` | float | `1` to `30` | `20` | Secondary movement smoothing intensity. | Independent from main smoothing. |
| FOV Size | `fovsize_sec` | float | `1` to `1000` | `150` | Secondary aiming field-of-view size. | Independent from main FOV. |
| FOV Smooth | `normalsmoothfov_sec` | float | `1` to `30` | `20` | Secondary FOV transition smoothing. | Independent from main FOV smooth. |

## Practical tuning tips

1. Start from defaults, then test in a controlled scenario for at least 2-3 minutes before changing any value.
2. Tune `normal_x_speed` first using only horizontal target movement, then tune `normal_y_speed` for vertical corrections.
3. If tracking overshoots, reduce speed by small steps (`0.2` to `0.5`) before increasing smoothing.
4. Use `normalsmooth` to remove micro-jitter; if aiming becomes too slow, decrease smoothing and slightly raise speed.
5. Increase `fovsize` only when target reacquisition is too slow; keep it smaller if unwanted targets are frequently selected.
6. Keep secondary values (`*_sec`) slightly slower and smoother than main values when Sec Aimbot is intended as fallback.
7. Re-check after sensitivity changes in game: `in_game_sens` changes usually require re-tuning speed and smoothing.
