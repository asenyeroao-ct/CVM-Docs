# Aimbot 标签参数说明

返回索引: [CVM.md](./CVM.md)

## 基础开关

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Aimbot | `enableaim` | bool | `true` / `false` | `true` | 主自瞄总开关。 | 关闭后主自瞄逻辑不执行。 |
| Enable Anti-Smoke | `anti_smoke_enabled` | bool | `true` / `false` | `false` | 为主自瞄启用防烟雾过滤。 | 运行时与副自瞄 anti-smoke 分别独立过滤。 |
| Mode | `mode` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier` | `Normal` | 选择主自瞄移动模式。 | 模式参数详见下方 mode 文档。 |

## 通用参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX` | float | `-100` 到 `100` | `0` | 目标点水平偏移。 | 用于武器/模型对齐微调。 |
| Y-Offset | `aim_offsetY` | float | `-100` 到 `100` | `0` | 目标点垂直偏移。 | 可用于头身位补偿。 |
| Target | `aim_type` | enum (string) | `head`, `body`, `nearest` | `head` | 选择目标瞄准点策略。 | 影响移动前的最终目标点。 |
| Keybind | `selected_mouse_button` | int | `0..4` 映射鼠标按键 | `1` | 主自瞄激活按键。 | 映射: `0=左键`, `1=右键`, `2=中键`, `3=侧键4`, `4=侧键5`。 |
| Type | `aimbot_activation_type` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | 定义按键触发主自瞄的方式。 | UI 文案: Hold to Enable/Disable, Toggle, Press to Enable。 |

## 模式文档

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)

## 主副自瞄分工建议

| 目标 | 主自瞄（`Aimbot`） | 副自瞄（`Sec Aimbot`） |
|---|---|---|
| 主战斗行为 | 偏直接、偏快速的模式 | 偏保守、偏兜底的模式 |
| 常见目标策略 | 按玩法选择 `head` 或 `nearest` | 常用 `body` 或更大的 FOV 保稳定 |
| 按键设计 | 主交战按键（常见为右键） | 使用独立按键避免重叠 |
| 平滑/速度取向 | 优先响应速度 | 优先稳定与可控 |

## 激活类型行为说明

| 激活类型 | 行为 | 适用场景 |
|---|---|---|
| `hold_enable` | 按住才启用 | 手动控制最直观 |
| `hold_disable` | 默认启用，按住时临时关闭 | 适合需要短时脱离自瞄 |
| `toggle` | 按一次开，再按一次关 | 适合持续追踪阶段 |
| `use_enable` | 按当前 UI 实现的启用式逻辑触发 | 使用时注意与其他热键冲突 |

## 避免冲突检查清单

1. 除非你明确需要覆盖行为，否则不要给主副自瞄绑定同一个按键。
2. 一套参数偏激进，另一套参数偏稳定，避免输出意图重复。
3. 主副都启用时，确认 `fovsize` 与 `fovsize_sec` 不会在复杂场景下互相抢目标。
4. 修改 `in_game_sens` 后，先重调 offset 与速度，再改模式高级参数。
5. 防烟雾开关建议分开复测，因为过滤变化会影响目标连续性。
