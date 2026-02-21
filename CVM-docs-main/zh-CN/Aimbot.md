# Aimbot 标签参数说明

返回索引: [CVM.md](./CVM.md)

## 基础开关

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Aimbot | `enableaim` | bool | `true` / `false` | `true` | 主自瞄总开关。 | 关闭后主自瞄逻辑不执行。 |
| Enable Anti-Smoke | `anti_smoke_enabled` | bool | `true` / `false` | `false` | 为主自瞄启用防烟雾过滤。 | 运行时与副自瞄 anti-smoke 独立过滤。 |
| Mode | `mode` | enum (string) | `Normal`, `Silent`, `NCAF`, `WindMouse`, `Bezier` | `Normal` | 选择主自瞄移动模式。 | 模式参数详见下方模式文档。 |

## 通用参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Offset | `aim_offsetX` | float | `-100` 到 `100` | `0` | 目标点水平偏移。 | 用于武器/模型对齐微调。 |
| Y-Offset | `aim_offsetY` | float | `-100` 到 `100` | `0` | 目标点垂直偏移。 | 用于头身位补偿调校。 |
| Target | `aim_type` | enum (string) | `head`, `body`, `nearest` | `head` | 选择目标点策略。 | 影响移动前的最终瞄准点。 |
| Keybind | `selected_mouse_button` | int 或 string | 鼠标按键（`0..4`）或键盘 token | `1` | 主自瞄激活绑定。 | 通过点击绑定设置；输入由当前 Input API 状态捕获（不是固定 Win32 监听器）。 |
| Type | `aimbot_activation_type` | enum (string) | `hold_enable`, `hold_disable`, `toggle`, `use_enable` | `hold_enable` | 定义按键如何控制主自瞄激活。 | UI 文案: Hold to Enable/Disable, Toggle, Press to Enable。 |

## FOV 与 ADS

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| FOV Size | `fovsize` | float | `1` 到 `1000` | `100` | 主自瞄基础 FOV 半径。 | 只要主自瞄启用就始终生效。 |
| Enable ADS FOV | `ads_fov_enabled` | bool | `true` / `false` | `false` | 启用 ADS 专用 FOV 覆盖。 | ADS 控件位于 FOV 区域。 |
| ADS FOV Size | `ads_fovsize` | float | `1` 到 `1000` | `100` | 按住 ADS 键时使用的 FOV 半径。 | 仅在启用 ADS FOV 时显示。 |
| ADS Keybind | `ads_key` | string | 鼠标键名或键盘 token | `Right Mouse Button` | 用于从基础 FOV 切换到 ADS FOV 的绑定。 | 通过点击绑定设置，使用当前 Input API 状态捕获。 |

## 模式文档

- [Normal](./aimbot/normal.md)
- [Silent](./aimbot/silent.md)
- [NCAF](./aimbot/ncaf.md)
- [WindMouse](./aimbot/windmouse.md)
- [Bezier](./aimbot/bezier.md)

## 主副自瞄分工（推荐）

| 目标 | 主自瞄（`Main Aimbot`） | 副自瞄（`Sec Aimbot`） |
|---|---|---|
| 主战斗行为 | 更直接、更快速的模式 | 更保守的兜底模式 |
| 常见目标策略 | 按玩法选择 `head` 或 `nearest` | 常用 `body` 或略大 FOV 提升稳定性 |
| 按键设计 | 主交战按键（常见为右键） | 使用独立按键避免重叠 |
| 平滑/速度取向 | 优先响应速度 | 优先稳定性 |

## 激活类型行为

| 激活类型 | 行为 | 实战建议 |
|---|---|---|
| `hold_enable` | 仅按住按键时激活 | 手动控制最可预测 |
| `hold_disable` | 默认激活，按住按键时临时停用 | 适合需要短暂脱离自瞄的场景 |
| `toggle` | 按一次启用，再按一次关闭 | 适合长时间追踪阶段 |
| `use_enable` | 使用 UI 实现中的启用式按键行为 | 使用该模式时尽量减少热键冲突 |

## 避免冲突检查清单

1. 除非你明确要测试覆盖行为，否则不要把主副自瞄绑定到同一按键。
2. 一套参数偏激进、另一套偏稳定，避免输出意图重复。
3. 主副同时启用时，确认 `fovsize` 与 `fovsize_sec` 不会在复杂场景里无意重叠。
4. 修改 `in_game_sens` 后，先重调 offset 和速度，再调整高级模式参数。
5. 防烟雾开关建议开/关分别复测，因为过滤变化会影响目标连续性。
