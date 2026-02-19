# Sec Aimbot 标签参数说明

返回索引: [CVM.md](./CVM.md)

## 基础开关

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Sec Aimbot | `enableaim_sec` | bool | `true` / `false` | `false` | 副自瞄总开关。 | 当激活条件满足时副自瞄才会执行。 |
| Enable Anti-Smoke | `anti_smoke_enabled_sec` | bool | `true` / `false` | `false` | 为副自瞄启用防烟雾过滤。 | 运行时与主自瞄 anti-smoke 分别独立过滤。 |
| Mode | `mode_sec` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier` | `Normal` | 选择副自瞄移动模式。 | 模式参数详见下方 mode 文档。 |

## 通用参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX_sec` | float | `-100` 到 `100` | `0` | 副自瞄目标点水平偏移。 | 在目标选定后应用。 |
| Y-Offset | `aim_offsetY_sec` | float | `-100` 到 `100` | `0` | 副自瞄目标点垂直偏移。 | 在目标选定后应用。 |
| Target | `aim_type_sec` | enum (string) | `head`, `body`, `nearest` | `head` | 副自瞄目标点策略。 | 与主自瞄 `aim_type` 独立。 |
| Keybind | `selected_mouse_button_sec` | int | `0..4` 映射鼠标按键 | `2` | 副自瞄激活按键。 | 映射: `0=左键`, `1=右键`, `2=中键`, `3=侧键4`, `4=侧键5`。 |
| Type | `aimbot_activation_type_sec` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | 定义按键触发副自瞄的方式。 | UI 文案: Hold to Enable/Disable, Toggle, Press to Enable。 |

## Silent 模式关键说明

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Sec Silent Parameter Set | `normal_x_speed_sec`, `normal_y_speed_sec`, `fovsize_sec` | mixed | 见 mode 文档 | `2`, `2`, `150` | 当前 UI 实现下，副自瞄 Silent 使用副自瞄速度 + FOV 参数，而不是独立 silent 延迟参数。 | 这是当前版本的预期行为。 |

## 模式文档

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)

## 与主自瞄联动建议

1. `selected_mouse_button_sec` 建议与主自瞄按键分开，除非你明确需要共享触发。
2. 副自瞄初始速度/平滑建议低于主自瞄，降低方向冲突概率。
3. 把副自瞄当作兜底配置：更宽松的 `fovsize_sec`、更稳的目标策略、更慢的移动。
4. 若切换手感不稳定，先分别验证每种激活类型，再做组合使用。
5. `General` 中采集与颜色相关参数变动后，要重新复测副自瞄表现。

## 副自瞄起步模板

| 目标 | 建议起点 |
|---|---|
| 稳定兜底追踪 | `aim_type_sec=body`，速度略低、平滑略高 |
| 近距离快速辅助 | `aim_type_sec=nearest`，中等速度、中等平滑 |
| 杂色场景降噪 | `fovsize_sec` 保持中等，避免过大 offset |

## Silent 模式补充说明

当前版本的副自瞄 Silent 依赖 `normal_x_speed_sec`、`normal_y_speed_sec`、`fovsize_sec` 这组参数。如果副 Silent 与主 Silent 表现差异较大，请先复查这三个键，再调整其他 silent 时间参数。
