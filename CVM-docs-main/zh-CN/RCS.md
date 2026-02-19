# RCS 标签参数说明

返回索引: [CVM.md](./CVM.md)

## RCS 参数

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable RCS | `enablercs` | bool | `true` / `false` | `false` | 压枪系统总开关。 | 实际生效仍取决于运行时开火状态判断。 |
| Pull Speed | `rcs_pull_speed` | int | `1` 到 `20` | `10` | 垂直压枪强度。 | 数值越高，下拉补偿越强。 |
| Activation Delay (ms) | `rcs_activation_delay` | int | `50` 到 `500` | `100` | 检测到开火后启动 RCS 的延迟。 | 可避免短点射时过早补偿。 |
| Rapid Click Threshold (ms) | `rcs_rapid_click_threshold` | int | `100` 到 `1000` | `200` | 用于识别快速点击模式的时间阈值。 | 影响点射/连发下的稳定性。 |
| Release Y-Axis on Fire | `rcs_release_y_enabled` | bool | `true` / `false` | `false` | 在特定开火条件下临时释放 Y 轴补偿。 | 需与下方持续时间参数配合。 |
| Release Duration (s) | `rcs_release_y_duration` | float | `0.1` 到 `5.0` | `1.0` | Y 轴释放窗口持续时长。 | 若释放时间过短可适当增大。 |


## 推荐调参顺序

1. 先开启 RCS，`rcs_pull_speed` 先保持默认附近。
2. 先调 `rcs_activation_delay`，让启动时机匹配武器开火节奏。
3. 再以小步长调整 `rcs_pull_speed`，控制垂直上飘。
4. 按点射/连发习惯校准 `rcs_rapid_click_threshold`。
5. 仅在核心压枪稳定后，再启用 Y 轴释放相关参数。

## 常见现象定位

| 现象 | 常见原因 | 建议调整 |
|---|---|---|
| 前几发被过度下拉 | 启动过早 | 增大 `rcs_activation_delay` |
| 长压仍明显上飘 | 下拉力度不足 | 逐步提高 `rcs_pull_speed` |
| 点射手感忽快忽慢 | 快速点击阈值不匹配 | 重调 `rcs_rapid_click_threshold` |
| 对枪中途突然不压 Y 轴 | Y 轴释放窗口过宽 | 关闭 `rcs_release_y_enabled` 或缩短 `rcs_release_y_duration` |
| 停火后补偿拖沓 | 延迟与释放时机不协调 | 重新平衡 `rcs_activation_delay` 与释放参数 |

## 稳定测试建议

- 每次只测一种武器配置，并尽量固定测试距离。
- 每次改动后至少做 20-30 发可复现测试再下结论。
- 灵敏度或采集/FPS 流程有变化时，需要重新验证 RCS。
