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
4. 建议先小幅提高 `Kp Max`，再调 `Kd`，最后再加 `Ki`。
5. 如果出现明显振荡，优先降低 `Kp Max` 或提高 `Kd`。
