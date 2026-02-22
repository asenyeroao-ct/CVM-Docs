# Aimbot 模式：PID

返回：[Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI 名称 | 配置键 | 类型 | 范围/选项 | 默认值 | 说明 | 备注 |
|---|---|---|---|---|---|---|
| Kp Min | `pid_kp_min` | float | `0.0` 到 `20.0` | `3.7` | 比例增益下限。 | 目标靠近准星中心时使用。 |
| Kp Max | `pid_kp_max` | float | `0.0` 到 `20.0` | `3.7` | 比例增益上限。 | 目标离中心越远越接近该值。 |
| Ki | `pid_ki` | float | `0.0` 到 `100.0` | `24.0` | 积分增益。 | 用于补偿持续误差。 |
| Kd | `pid_kd` | float | `0.0` 到 `10.0` | `0.11` | 微分增益。 | 抑制误差突变导致的抖动。 |
| Max Output | `pid_max_output` | float | `1.0` 到 `200.0` | `50.0` | 单轴输出限幅。 | 限制过冲和异常尖峰。 |
| X Speed | `pid_x_speed` | float | `0.1` 到 `5.0` | `1.0` | X 轴输出倍率。 | 用于让水平速度与 Y 轴不同。 |
| Y Speed | `pid_y_speed` | float | `0.1` 到 `5.0` | `1.0` | Y 轴输出倍率。 | 用于让垂直速度与 X 轴不同。 |
| FOV Size | `fovsize` | float | `1` 到 `1000` | `100` | 主瞄 PID 使用的 FOV。 | 同时用于动态 Kp 的插值。 |

## Sec Aimbot

| UI 名称 | 配置键 | 类型 | 范围/选项 | 默认值 | 说明 | 备注 |
|---|---|---|---|---|---|---|
| Kp Min | `pid_kp_min_sec` | float | `0.0` 到 `20.0` | `3.7` | 副瞄比例增益下限。 | 与主瞄独立。 |
| Kp Max | `pid_kp_max_sec` | float | `0.0` 到 `20.0` | `3.7` | 副瞄比例增益上限。 | 与主瞄独立。 |
| Ki | `pid_ki_sec` | float | `0.0` 到 `100.0` | `24.0` | 副瞄积分增益。 | 与主瞄独立。 |
| Kd | `pid_kd_sec` | float | `0.0` 到 `10.0` | `0.11` | 副瞄微分增益。 | 与主瞄独立。 |
| Max Output | `pid_max_output_sec` | float | `1.0` 到 `200.0` | `50.0` | 副瞄单轴输出限幅。 | 与主瞄独立。 |
| X Speed | `pid_x_speed_sec` | float | `0.1` 到 `5.0` | `1.0` | 副瞄 X 轴输出倍率。 | 与主瞄独立。 |
| Y Speed | `pid_y_speed_sec` | float | `0.1` 到 `5.0` | `1.0` | 副瞄 Y 轴输出倍率。 | 与主瞄独立。 |
| FOV Size | `fovsize_sec` | float | `1` 到 `1000` | `150` | 副瞄 PID 使用的 FOV。 | 同时用于动态 Kp 的插值。 |

## 说明

1. UI 中该模式显示为 `PID Controller ( RISK )`。
2. Kp 会根据目标到准星中心的距离动态插值：  
   `Kp = Kp Min + (Kp Max - Kp Min) * clamp(distance_to_center / FOV, 0, 1)`。
3. 当 `Kp Min = Kp Max` 时，行为等同固定 Kp 的 PID。
4. 单轴输出可近似理解为：  
   `output = clamp((Kp * err + Ki * integral + Kd * derivative) * axis_speed, -Max Output, Max Output)`。
5. 若 `Kp Min`/`Kp Max` 输入顺序相反，系统会自动纠正。

## 详细调参流程

1. 先建立稳定基线，再开始调增益。
   - 先将 `X Speed` 与 `Y Speed` 设为 `1.0`，避免轴向倍率干扰增益判断。
   - 调参阶段建议 `Max Output` 先用中等值（例如 `30` 到 `60`）。
   - 调参期间尽量固定 FOV，确保每次测试可对比。
2. 先做固定 Kp 调参。
   - 设 `Kp Min = Kp Max`。
   - 先把 `Ki = 0`，`Kd` 设较小值（例如 `0.05` 到 `0.15`）。
   - 逐步提高 Kp，直到响应够快；若开始振荡，再回退一点。
3. 再用 `Kd` 抑制过冲。
   - 出现冲过头、左右抖动时，小步增加 `Kd`。
   - 若跟枪变钝或噪声变大，适度回调 `Kd`。
4. 最后再加 `Ki` 消除持续偏差。
   - `Ki` 请慢慢加，过大容易出现漂移或延迟振荡。
   - 若误差接近 0 后仍持续“推着走”，优先降低 `Ki`。
5. 回到动态 Kp 范围调优。
   - `Kp Min` 偏低可提升准星中心附近稳定性。
   - `Kp Max` 偏高可提升大误差时的拉回速度。
   - 常见起点：`Kp Min` 约为 `Kp Max` 的 `50%` 到 `80%`，再按手感细调。
6. 最后微调外围参数。
   - 远距离误差拉不动、常常顶限幅时，增加 `Max Output`。
   - 移动突兀或爆冲明显时，降低 `Max Output`。
   - 仅在增益定型后，再调 `X Speed`/`Y Speed` 修正轴向不平衡。

## 症状与调整对照

| 症状 | 常见原因 | 优先调整项 |
|---|---|---|
| 准星在目标周围来回振荡 | `Kp Max` 偏高或 `Kd` 偏低 | 先降 `Kp Max`，再小幅升 `Kd` |
| 快速拉过去后反弹 | 限幅偏宽且阻尼不足 | 降 `Max Output` 或升 `Kd` |
| 远处目标跟不上 | `Kp Max` 偏低或 `Max Output` 偏低 | 先升 `Kp Max`，再看是否升 `Max Output` |
| 准星中心附近微抖 | `Kp Min` 偏高 / `Kd` 过敏感 | 降 `Kp Min`，再微降 `Kd` |
| 长期存在小偏差 | `Ki` 偏低（或为 0） | 小步升 `Ki` |
| 跟枪后出现慢性漂移或回摆 | `Ki` 偏高 | 降 `Ki` |

## 会影响手感的实现细节

1. 修改 `Ki` 或 `Kd` 会重置该模式的 PID 内部状态（积分与历史误差）。
2. 当 `Ki != 0` 时，积分项会按 `Max Output / |Ki|` 限制，因此 `Ki` 与 `Max Output` 需要联动考虑。
3. `FOV Size` 会直接影响 Kp 从 `Kp Min` 过渡到 `Kp Max` 的斜率。
4. 主瞄与副瞄 PID 参数完全独立，`*_sec` 需要单独调参。
