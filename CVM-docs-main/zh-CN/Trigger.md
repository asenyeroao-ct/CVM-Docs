# Trigger 标签参数说明

返回索引: [CVM.md](./CVM.md)

## 核心参数（共用）

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Triggerbot | `enabletb` | bool | `true` / `false` | `false` | 扳机逻辑总开关。 | 实际触发仍受激活模式和按键状态控制。 |
| Trigger Type | `trigger_type` | enum (string) | `current`, `rgb` | `current` | 选择 Trigger 引擎。 | `Classic Trigger` 使用现有 HSV 逻辑；`RGB Trigger` 使用 RGB 预设匹配。 |
| FOV Size | `tbfovsize` | float | `1` 到 `300` | `5` | Trigger 检测视野大小。 | 作为 ROI 缩放来源。 |

## Classic Trigger 模式（`trigger_type = current`）

### 延迟与按住

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Delay Range (min) | `tbdelay_min` | float | `0.0` 到 `1.0` 秒 | `0.08` | 开火前最小延迟。 | 运行时在 `[min, max]` 取值。 |
| Delay Range (max) | `tbdelay_max` | float | `0.0` 到 `1.0` 秒 | `0.15` | 开火前最大延迟。 | 保持 `max >= min`。 |
| Hold Range (min) | `tbhold_min` | int | `5` 到 `500` ms | `40` | 每次点击最短按住时长。 | 运行时在 `[min, max]` 取值。 |
| Hold Range (max) | `tbhold_max` | int | `5` 到 `500` ms | `60` | 每次点击最长按住时长。 | 保持 `max >= min`。 |

### 判定条件

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Min Pixels | `trigger_min_pixels` | int | `1` 到 `200` | `4` | 进入开火逻辑前要求的最小命中像素数。 | 抑制噪点误触发。 |
| Min Ratio | `trigger_min_ratio` | float | `0.0` 到 `1.0` | `0.03` | Trigger 判定最小像素占比。 | ROI 内目标占比门限。 |
| Confirm Frames | `trigger_confirm_frames` | int | `1` 到 `10` | `2` | 连续满足判定所需帧数。 | 值越大越能抑制抖动误触发。 |

### Burst 设置

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Cooldown Range (min) | `tbcooldown_min` | float | `0.0` 到 `5.0` 秒 | `0.0` | 连发序列间最小冷却。 | 运行时在 `[min, max]` 取值。 |
| Cooldown Range (max) | `tbcooldown_max` | float | `0.0` 到 `5.0` 秒 | `0.0` | 连发序列间最大冷却。 | 保持 `max >= min`。 |
| Burst Count Range (min) | `tbburst_count_min` | int | `1` 到 `10` | `1` | 每次连发最少发数。 | 运行时在 `[min, max]` 取整。 |
| Burst Count Range (max) | `tbburst_count_max` | int | `1` 到 `10` | `1` | 每次连发最多发数。 | 保持 `max >= min`。 |
| Burst Interval Range (min) | `tbburst_interval_min` | float | `0` 到 `500` ms | `0.0` | 连发内两发最短间隔。 | 运行时在 `[min, max]` 取值。 |
| Burst Interval Range (max) | `tbburst_interval_max` | float | `0` 到 `500` ms | `0.0` | 连发内两发最长间隔。 | 保持 `max >= min`。 |

## RGB 模式（`trigger_type = rgb`）

### RGB 预设列表

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| RGB Preset | `rgb_color_profile` | enum (string) | `red`, `yellow`, `purple` | `purple` | 选择固定 RGB 目标色与容差预设。 | 预设值在代码中固定。 |

预设定义：
- `red`: `RGB(180, 40, 40)`，容差 `30`
- `yellow`: `RGB(230, 230, 80)`，容差 `35`
- `purple`: `RGB(161, 69, 163)`，容差 `25~30`（运行时取上限）

### RGB 时间参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Delay Range (min) | `rgb_tbdelay_min` | float | `0.0` 到 `1.0` 秒 | `0.08` | RGB 模式最小开火延迟。 | 运行时在 `[min, max]` 取值。 |
| Delay Range (max) | `rgb_tbdelay_max` | float | `0.0` 到 `1.0` 秒 | `0.15` | RGB 模式最大开火延迟。 | 保持 `max >= min`。 |
| Hold Range (min) | `rgb_tbhold_min` | int | `5` 到 `500` ms | `40` | RGB 模式最短按住时长。 | 运行时在 `[min, max]` 取值。 |
| Hold Range (max) | `rgb_tbhold_max` | int | `5` 到 `500` ms | `60` | RGB 模式最长按住时长。 | 保持 `max >= min`。 |
| Cooldown Range (min) | `rgb_tbcooldown_min` | float | `0.0` 到 `5.0` 秒 | `0.0` | RGB 模式最小冷却时间。 | 运行时在 `[min, max]` 取值。 |
| Cooldown Range (max) | `rgb_tbcooldown_max` | float | `0.0` 到 `5.0` 秒 | `0.0` | RGB 模式最大冷却时间。 | 保持 `max >= min`。 |

RGB 模式行为：
- 使用 `tbfovsize` 生成中心 ROI。
- 不依赖目标列表预过滤。
- 只执行单次点击循环（press -> hold -> release），不使用 Burst Count / Burst Interval。

## 激活参数（共用）

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Keybind | `selected_tb_btn` | int | `0..4` 映射鼠标按键 | `1` | Trigger 激活按键。 | 映射: `0=左键`, `1=右键`, `2=中键`, `3=侧键4`, `4=侧键5`。 |
| Trigger Mode | `trigger_activation_type` | enum (string) | `hold_enable`, `hold_disable`, `toggle` | `hold_enable` | Trigger 逻辑激活模式。 | UI 文案: Hold to Enable/Disable, Toggle。 |

## Strafe Helper（共用，条件显示）

以下 API 支持该功能：
- `SendInput`
- `Net`
- `KmboxA`
- `DHZ`

当该区块隐藏时，`trigger_strafe_mode` 会被强制设为 `off`。

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Mode | `trigger_strafe_mode` | enum (string) | `off`, `auto`, `manual_wait` | `off` | 选择 Strafe Helper 行为。 | `off`：关闭；`auto`：在射击前注入反向移动键；`manual_wait`：仅在中性移动输入下允许 Trigger。 |
| Auto Lead (ms) | `trigger_strafe_auto_lead_ms` | int | `0` 到 `50` | `8` | 反向键按下到触发射击的延迟。 | 仅在 `auto` 模式显示。 |
| Neutral Wait (ms) | `trigger_strafe_manual_neutral_ms` | int | `0` 到 `300` | `0` | 允许 Trigger 前要求的中性移动持续时间。 | 仅在 `manual_wait` 模式显示。 |

`manual_wait` 的中性定义：
- 水平轴：`A+D` 同时按下或同时松开。
- 垂直轴：`W+S` 同时按下或同时松开。
- 仅当两个轴都中性时才允许 Trigger。

## 快速调参流程

1. 先用 `trigger_type=current`，并把 `tbfovsize` 设小一些进行可控测试。
2. 先将延迟/按住区间设为 `min=max`，观察可复现行为。
3. 判定阈值按顺序调：`trigger_min_pixels` -> `trigger_min_ratio` -> `trigger_confirm_frames`。
4. 基线稳定后，再逐步拉开 min/max 区间增加随机性。
5. 使用 RGB 模式时，先确认颜色预设匹配，再独立调 RGB 时间参数。

## 现象与参数对应

| 现象 | 优先检查键 | 调整方向 |
|---|---|---|
| 噪点也会频繁触发 | `trigger_min_pixels`、`trigger_min_ratio`、`trigger_confirm_frames` | 逐步上调 |
| 触发明显偏慢 | `tbdelay_min/max`、`trigger_confirm_frames` | 先降延迟，再降确认帧 |
| 连发过于激进 | `tbburst_count_*`、`tbburst_interval_*`、`tbcooldown_*` | 降连发数，增间隔/冷却 |
| RGB 模式漏触发 | `rgb_color_profile`、`tbfovsize`、`rgb_tbdelay_*` | 先校颜色预设，再调 FOV 与延迟 |
| 每次开局手感差异大 | 各区间参数跨度过大 | 收窄区间后复测 |

## 区间参数使用建议

- 所有区间键都保持 `max >= min`；不确定时先设成相等再慢慢放开。
- `tbfovsize` 过大时，误命中概率会上升，需要配合更严格的判定阈值。
- 点射武器通常更适合较短 hold + 中等 cooldown 的节奏。
