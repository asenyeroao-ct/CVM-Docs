# RCS Tab Parameters

Back to index: [CVM.md](./CVM.md)

## RCS Parameters

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable RCS | `enablercs` | bool | `true` / `false` | `false` | Master switch for recoil compensation system. | RCS behavior still depends on runtime fire-state checks. |
| Pull Speed | `rcs_pull_speed` | int | `1` to `20` | `10` | Vertical recoil pull strength. | Higher value means stronger downward compensation. |
| Activation Delay (ms) | `rcs_activation_delay` | int | `50` to `500` | `100` | Delay before RCS starts after fire detection. | Prevents overreaction on short taps. |
| Rapid Click Threshold (ms) | `rcs_rapid_click_threshold` | int | `100` to `1000` | `200` | Window used to classify rapid click behavior. | Helps tune activation stability for tap/burst patterns. |
| Release Y-Axis on Fire | `rcs_release_y_enabled` | bool | `true` / `false` | `false` | Temporarily releases Y-axis compensation under specific fire conditions. | Works together with duration setting below. |
| Release Duration (s) | `rcs_release_y_duration` | float | `0.1` to `5.0` | `1.0` | Duration of Y-axis release window. | Increase if release window feels too short. |


## Recommended Tuning Order

1. Enable RCS and keep `rcs_pull_speed` near default first.
2. Tune `rcs_activation_delay` to match your weapon fire start pattern.
3. Adjust `rcs_pull_speed` in small steps until vertical drift is controlled.
4. Calibrate `rcs_rapid_click_threshold` based on tap/burst rhythm.
5. Use Y-axis release options only after core pull behavior is stable.

## Symptom Guide

| Symptom | Likely Cause | Suggested Adjustment |
|---|---|---|
| First bullets over-correct downward | Activation starts too early | Increase `rcs_activation_delay` |
| Long spray still climbs upward | Pull strength too weak | Increase `rcs_pull_speed` gradually |
| Tap firing feels inconsistent | Rapid click window mismatch | Re-tune `rcs_rapid_click_threshold` |
| Compensation drops unexpectedly mid-fight | Y release window too permissive | Disable `rcs_release_y_enabled` or shorten `rcs_release_y_duration` |
| Compensation feels sticky after firing stops | Delay/release not aligned with fire rhythm | Rebalance `rcs_activation_delay` and release settings |

## Notes for Reliable Testing

- Test with one weapon profile at a time and keep distance constant.
- Run at least 20-30 controlled shots per change before judging results.
- Re-test after any major sensitivity or FPS/capture pipeline change.
