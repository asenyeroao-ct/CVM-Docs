# Sec Aimbot 标签参数说明

返回索引: [CVM.md](./CVM.md)

## 基础开关

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Sec Aimbot | `enableaim_sec` | bool | `true` / `false` | `false` | 副自瞄总开关。 | 满足激活逻辑时才会执行副自瞄。 |
| Enable Anti-Smoke | `anti_smoke_enabled_sec` | bool | `true` / `false` | `false` | 为副自瞄目标启用防烟雾过滤。 | 运行时与主自瞄 anti-smoke 独立过滤。 |
| Mode | `mode_sec` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier`, `PID` | `Normal` | 选择副自瞄移动模式。 | PID 的 UI 显示名称为 `PID Controller ( RISK )`。 |

## 通用参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX_sec` | float | `-100` 到 `100` | `0` | 副自瞄目标点水平偏移。 | 在选定目标后应用。 |
| Y-Offset | `aim_offsetY_sec` | float | `-100` 到 `100` | `0` | 副自瞄目标点垂直偏移。 | 在选定目标后应用。 |
| Target | `aim_type_sec` | enum (string) | `head`, `body`, `nearest` | `head` | 副自瞄目标选择策略。 | 与主自瞄 `aim_type` 独立。 |
| Keybind | `selected_mouse_button_sec` | int 或 string | 鼠标按键（`0..4`）或键盘 token | `2` | 副自瞄激活绑定。 | 通过点击绑定设置；输入由当前 Input API 状态捕获（不是固定 Win32 监听器）。 |
| Type | `aimbot_activation_type_sec` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | 定义按键如何控制副自瞄激活。 | UI 文案: Hold to Enable/Disable, Toggle, Press to Enable。 |

## FOV 与 ADS

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| FOV Size | `fovsize_sec` | float | `1` 到 `1000` | `150` | 副自瞄基础 FOV 半径。 | 只要副自瞄启用就始终生效。 |
| Enable ADS FOV | `ads_fov_enabled_sec` | bool | `true` / `false` | `false` | 为副模式启用 ADS 专用 FOV 覆盖。 | ADS 控件位于 FOV 区域。 |
| ADS FOV Size | `ads_fovsize_sec` | float | `1` 到 `1000` | `150` | 按住副 ADS 键时使用的 FOV 半径。 | 仅在启用 ADS FOV 时显示。 |
| ADS Keybind | `ads_key_sec` | string | 鼠标键名或键盘 token | `Right Mouse Button` | 用于从副基础 FOV 切换到 ADS FOV 的绑定。 | 通过点击绑定设置，使用当前 Input API 状态捕获。 |

## Silent 模式重要说明

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Sec Silent Parameter Set | `normal_x_speed_sec`, `normal_y_speed_sec`, `fovsize_sec` | mixed | 见模式文档 | `2`, `2`, `150` | 当前 UI 实现下，副 Silent 使用副自瞄速度 + FOV 键，而不是独立 silent 延迟键。 | 这是当前版本的预期行为。 |

## 模式文档

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)
- [PID](./aimbot/pid.md)

## 与主自瞄联动建议

1. `selected_mouse_button_sec` 建议与主自瞄按键不同，除非你明确要共享激活。
2. 副自瞄初始速度/平滑建议低于主自瞄，降低突发方向冲突。
3. 将副自瞄作为兜底配置：更宽 `fovsize_sec`、更保守目标策略、更慢移动。
4. 若切换手感不稳定，先分别验证每种激活类型，再组合使用。
5. `General` 里采集/颜色相关参数改动后，需重新验证副自瞄表现。

## 副自瞄起步模板

| 目标 | 建议起点 |
|---|---|
| 稳定兜底追踪 | `aim_type_sec=body`，速度略低、平滑更高 |
| 近距离快速辅助 | `aim_type_sec=nearest`，中等速度、中等平滑 |
| 低噪混合场景 | 保持 `fovsize_sec` 适中，避免过大 offset |

## Silent 模式补充说明

当前副 Silent 依赖 `normal_x_speed_sec`、`normal_y_speed_sec`、`fovsize_sec` 这组键。若表现与主 Silent 差异明显，请先检查这三个键，再调整其他 silent 时间参数。
