# Trigger Tab Parameters

Back to index: [CVM.md](./CVM.md)

## Core (Shared)

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Triggerbot | `enabletb` | bool | `true` / `false` | `false` | Master switch for triggerbot logic. | Trigger logic still depends on activation mode and keybind state. |
| Trigger Type | `trigger_type` | enum (string) | `current`, `rgb` | `current` | Selects which trigger engine runs. | `Current` uses HSV pipeline; `RGB Trigger` uses fixed RGB preset matching. |
| FOV Size | `tbfovsize` | float | `1` to `300` | `5` | Trigger detection field-of-view size. | Used as ROI scale source in trigger processing. |

## Current Mode (`trigger_type = current`)

### Delay and Hold

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Delay Range (min) | `tbdelay_min` | float | `0.0` to `1.0` seconds | `0.08` | Minimum pre-fire delay. | Runtime picks value in `[min, max]`. |
| Delay Range (max) | `tbdelay_max` | float | `0.0` to `1.0` seconds | `0.15` | Maximum pre-fire delay. | Keep `max >= min`. |
| Hold Range (min) | `tbhold_min` | int | `5` to `500` ms | `40` | Minimum hold duration per click cycle. | Runtime picks value in `[min, max]`. |
| Hold Range (max) | `tbhold_max` | int | `5` to `500` ms | `60` | Maximum hold duration per click cycle. | Keep `max >= min`. |

### Trigger Conditions

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Min Pixels | `trigger_min_pixels` | int | `1` to `200` | `4` | Minimum detected pixels required before firing logic is considered valid. | Noise guard for tiny false positives. |
| Min Ratio | `trigger_min_ratio` | float | `0.0` to `1.0` | `0.03` | Minimum pixel ratio threshold for trigger validation. | Relative gate for target occupancy in ROI. |
| Confirm Frames | `trigger_confirm_frames` | int | `1` to `10` | `2` | Consecutive valid frames required before firing. | Higher value reduces flicker-triggered shots. |

### Burst Settings

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Cooldown Range (min) | `tbcooldown_min` | float | `0.0` to `5.0` seconds | `0.0` | Minimum cooldown between burst sequences. | Runtime picks value in `[min, max]`. |
| Cooldown Range (max) | `tbcooldown_max` | float | `0.0` to `5.0` seconds | `0.0` | Maximum cooldown between burst sequences. | Keep `max >= min`. |
| Burst Count Range (min) | `tbburst_count_min` | int | `1` to `10` | `1` | Minimum number of shots per burst. | Runtime picks integer in `[min, max]`. |
| Burst Count Range (max) | `tbburst_count_max` | int | `1` to `10` | `1` | Maximum number of shots per burst. | Keep `max >= min`. |
| Burst Interval Range (min) | `tbburst_interval_min` | float | `0` to `500` ms | `0.0` | Minimum interval between shots in same burst. | Runtime picks value in `[min, max]`. |
| Burst Interval Range (max) | `tbburst_interval_max` | float | `0` to `500` ms | `0.0` | Maximum interval between shots in same burst. | Keep `max >= min`. |

## RGB Mode (`trigger_type = rgb`)

### RGB Preset List

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| RGB Preset | `rgb_color_profile` | enum (string) | `red`, `yellow`, `purple` | `purple` | Selects fixed RGB target color and tolerance profile. | Preset values are fixed in code. |

Preset definitions:
- `red`: `RGB(180, 40, 40)`, tolerance `30`
- `yellow`: `RGB(230, 230, 80)`, tolerance `35`
- `purple`: `RGB(161, 69, 163)`, tolerance `25~30` (runtime uses upper bound)

### RGB Timing

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Delay Range (min) | `rgb_tbdelay_min` | float | `0.0` to `1.0` seconds | `0.08` | Minimum pre-fire delay in RGB mode. | Runtime picks value in `[min, max]`. |
| Delay Range (max) | `rgb_tbdelay_max` | float | `0.0` to `1.0` seconds | `0.15` | Maximum pre-fire delay in RGB mode. | Keep `max >= min`. |
| Hold Range (min) | `rgb_tbhold_min` | int | `5` to `500` ms | `40` | Minimum click hold duration in RGB mode. | Runtime picks value in `[min, max]`. |
| Hold Range (max) | `rgb_tbhold_max` | int | `5` to `500` ms | `60` | Maximum click hold duration in RGB mode. | Keep `max >= min`. |
| Cooldown Range (min) | `rgb_tbcooldown_min` | float | `0.0` to `5.0` seconds | `0.0` | Minimum cooldown after a shot in RGB mode. | Runtime picks value in `[min, max]`. |
| Cooldown Range (max) | `rgb_tbcooldown_max` | float | `0.0` to `5.0` seconds | `0.0` | Maximum cooldown after a shot in RGB mode. | Keep `max >= min`. |

RGB mode behavior:
- Uses center ROI derived from `tbfovsize`.
- Does not depend on target list gating.
- Fires single click cycle (press -> hold -> release), no burst count/interval settings.

## Activation (Shared)

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Keybind | `selected_tb_btn` | int | `0..4` mapped to mouse buttons | `1` | Triggerbot activation button. | Mapping: `0=Left`, `1=Right`, `2=Middle`, `3=Side4`, `4=Side5`. |
| Trigger Mode | `trigger_activation_type` | enum (string) | `hold_enable`, `hold_disable`, `toggle` | `hold_enable` | Activation style for trigger logic. | UI labels: Hold to Enable/Disable, Toggle. |

## Strafe Helper (Shared, Conditional)

Supported APIs for this feature:
- `SendInput`
- `Net`
- `KmboxA`
- `DHZ`

When hidden, `trigger_strafe_mode` is forced to `off`.

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Mode | `trigger_strafe_mode` | enum (string) | `off`, `auto`, `manual_wait` | `off` | Selects strafe-helper behavior. | `off`: disabled; `auto`: inject opposite movement key before trigger shot; `manual_wait`: allow trigger only under neutral movement input. |
| Auto Lead (ms) | `trigger_strafe_auto_lead_ms` | int | `0` to `50` | `8` | Delay between injected opposite key-down and trigger shot. | Visible only in `auto` mode. |
| Neutral Wait (ms) | `trigger_strafe_manual_neutral_ms` | int | `0` to `300` | `0` | Required neutral movement duration before trigger is allowed. | Visible only in `manual_wait` mode. |

Manual wait neutral definition:
- Horizontal axis is neutral when `A+D` are both pressed or both released.
- Vertical axis is neutral when `W+S` are both pressed or both released.
- Trigger is allowed only when both axes are neutral.

## Quick Tuning Workflow

1. Start with `trigger_type=current` and a small `tbfovsize` for controlled testing.
2. Set delay/hold ranges with `min=max` first to observe deterministic behavior.
3. Tune validation gates in this order: `trigger_min_pixels` -> `trigger_min_ratio` -> `trigger_confirm_frames`.
4. Add randomness by widening min/max ranges only after baseline behavior is stable.
5. If you use RGB mode, verify preset fit first, then tune RGB timing separately from current mode.

## Symptom to Parameter Map

| Symptom | Primary Keys | Direction |
|---|---|---|
| Fires too often on noise | `trigger_min_pixels`, `trigger_min_ratio`, `trigger_confirm_frames` | Increase gradually |
| Feels late to fire | `tbdelay_min/max`, `trigger_confirm_frames` | Reduce delay first, then lower confirm frames |
| Burst too aggressive | `tbburst_count_*`, `tbburst_interval_*`, `tbcooldown_*` | Lower count, increase interval/cooldown |
| RGB mode misses obvious targets | `rgb_color_profile`, `tbfovsize`, `rgb_tbdelay_*` | Verify color preset, then retune FOV and delay |
| Inconsistent behavior between sessions | Min/max ranges too wide | Narrow ranges and re-validate |

## Practical Range Guardrails

- Keep `max >= min` for all ranged keys; if uncertain, set equal values and expand later.
- Large ROI (`tbfovsize`) increases chance of incidental color hits; pair with stricter validation thresholds.
- For tap-heavy weapons, shorter hold and moderate cooldown usually produce cleaner trigger rhythm.
