# CVM colorBot 画面传输与处理流程（双机）

返回索引: [CVM.md](./CVM.md)

本文只说明端到端流程：画面如何进 CVM、CVM 如何处理、最后如何调用 Input API 输出移动。

## 快速总览

`主机画面 -> 传输链路(OBS-UDP / OBS-NDI / Capture Card) -> CVM取帧 -> 目标检测 -> 计算dx/dy -> Input API输出`

## 1. 典型双机链路（分开说明）

### 1.1 OBS-UDP 链路

适用:
- 现有网络环境可用，想低成本搭双机。
- 可接受网络质量对稳定性的影响。

完整流程:
1. 主机用 OBS 采集游戏画面（`Game Capture` / `Display Capture`）。
2. 在 OBS 对源加 `Crop/Pad`，只保留 FOV 区域。
3. 在 OBS 输出中用 FFmpeg URL 发送 UDP：
   `udp://<副机IP>:<端口>`
4. 在副机 CVM 选择 `Method = UDP`。
5. 在 CVM 填写与 OBS 完全一致的 `UDP IP/Port`。
6. 点击连接，`CaptureService` 开始持续收帧。
7. 画面进入追踪处理流程。

关键配置:
- OBS URL 与 CVM 的 `udp_ip` / `udp_port` 必须逐字一致。
- 建议保持裁剪后传输，避免大帧导致掉帧和解码压力。
- `target_fps` 建议从稳定值开始，再逐步上调。

### 1.2 OBS-NDI 链路

适用:
- 仅用于兼容历史环境。
- 新部署不建议选择此方案。

完整流程:
1. 主机在 OBS 开启 NDI 输出（Program/Preview 作为 NDI Source）。
2. 副机与主机处于同一局域网。
3. 在副机 CVM 选择 `Method = NDI`。
4. 点击刷新源列表，CVM 读取可见 NDI Source。
5. 选择目标 NDI Source 并连接。
6. `CaptureService` 持续从 NDI 接收帧并交给追踪流程。

关键配置:
- 确认副机能看到主机发布的 NDI Source 名称。
- NDI 源切换后建议重新确认连接状态与实际分辨率。
- 若启用 NDI 中心裁切，按目标 FOV 调整 `ndi_fov_enabled` / `ndi_fov`。

与 UDP 的简要对比:
- NDI: 更偏向画面链路稳定与源管理便利。
- UDP: 更偏向低延迟与速度表现。
- 结论: 不论如何都不推荐 NDI；NDI 仅作为历史遗留兼容路径保留。

NDI 风险补充（Valorant）:
- 社区存在「NDI 可能已被检测」的说法，目前没有详细且严谨的测试能确认 NDI 是否会被检测或导致封禁。
- 也有用户反馈在正常画面比例下可运行，但这不构成安全保证。
- 因结论不稳定且风险不可量化，实践上仍不推荐将 NDI 作为默认方案。

### 1.3 Capture Card（采集卡）链路

适用:
- 追求更直接、可预期的物理采集路径。
- 可接受硬件成本与布线复杂度。

完整流程:
1. 主机视频输出（通常 HDMI）接入采集卡输入。
2. 采集卡通过 USB 3.0 / PCIe 接到副机。
3. 在副机 CVM 选择 `Method = CaptureCard`。
4. 设置设备索引、分辨率、FPS 等参数并连接。
5. `CaptureService` 从采集卡驱动持续读取帧。
6. 帧进入与其他链路一致的追踪处理流程。

关键配置:
- 设备索引需对应正确采集设备。
- 分辨率/FPS 先从稳定组合开始，再逐步提高。
- 采集卡颜色转换相关参数建议保持默认，再按实际画面调试。

## 2. CVM 收到帧后的统一处理流程

不管前面是 UDP、NDI 还是采集卡，进入 CVM 后流程一致：

1. 取帧: `AimTracker` 从当前后端读取最新帧。
2. 检测: HSV 颜色检测生成候选目标。
3. 选目标: 选择当前有效目标（通常是离中心最近）。
4. 计算偏移: 计算中心偏移量 `dx/dy`。
5. 模式处理: 按模式（Normal/Silent/NCAF/WindMouse/Bezier）生成移动指令。
6. 入队: 将移动指令写入 `move_queue`。

## 3. Input API 调用链（输出阶段）

1. 移动线程从 `move_queue` 取指令。
2. 调用统一接口：`Mouse.move()` / `Mouse.press()` / `Mouse.release()`。
3. `Mouse` 根据当前配置路由到后端：
   `Serial` / `Arduino` / `SendInput` / `Net` / `MakV2` / `DHZ` 等。
4. 后端执行系统或硬件输入，完成鼠标移动/按键。

一句话:
`CVM算出dx/dy -> 调用Mouse接口 -> 路由Input API -> 输出真实输入`

## 4. 安全线与推荐顺序（双机前提）

先给结论（你要的安全线）:

1. `Capture Card（采集卡）`：最安全  
   原因: 游戏机侧不需要额外运行传输程序或插件，只输出视频信号。
2. `OBS-UDP`：相对安全，且推荐优先于 NDI  
   原因: UDP 路径是 OBS 原生输出链路，组件更少，整体风险面通常更小；同时通常也更快。
3. `OBS-NDI`：相对风险更高  
   原因: NDI 不是 OBS 核心原生路径，依赖额外生态与链路组件，暴露面更大。
4. 立场说明  
   NDI 在本项目中属于历史遗留物，只保留兼容性，不作为推荐方案。

重要前提:
- 只要不是 `1PC`（游戏与处理同机），整体都在相对安全线内。
- 在双机方案里，若在 `OBS-UDP` 和 `OBS-NDI` 二选一，默认更推荐 `OBS-UDP`（安全性与速度两方面都更优）。
- 更进一步：新环境请直接避免 NDI，优先使用 `Capture Card` 或 `OBS-UDP`。
- 若出现封禁但并非同类用户大面积同时触发，更常见是个体配置、行为特征或对局表现触发人工审查，而非单一路径被统一判定。

对比表:

| 链路 | 安全线（相对） | 速度体感（常见） | 结论 |
|---|---|---|---|
| Capture Card | 最高 | 高且稳定 | 安全优先首选 |
| OBS-UDP | 中高 | 通常快于 NDI | 双机网络链路首推 |
| OBS-NDI | 中 | 通常慢于 UDP | 不推荐，仅历史兼容 |

## 5. 分路径排障（按优先顺序）

### OBS-UDP

1. 双向 `ping` 确认网络互通。
2. OBS 必须已 `Start Recording`（即开始发送 UDP）。
3. 检查 URL 与 CVM `IP/Port` 是否完全一致。
4. 检查是否已做 `Crop/Pad`。
5. 降低 FPS 观察是否恢复稳定。

### OBS-NDI

1. 确认主机已发布 NDI Source。
2. 在 CVM 刷新源列表，确认能看到目标源名。
3. 选择正确源并重新连接。
4. 检查主副机是否在同网段，网络隔离是否阻断发现/传输。

### Capture Card

1. 系统设备管理器确认采集卡已识别。
2. CVM 设备索引是否选对。
3. 降低分辨率/FPS 排除带宽或驱动压力问题。
4. 检查线材、接口规格与供电稳定性。

## 6. 相关文档

- OBS UDP 设置: [OBS-UDP-setup.md](../../shared-guides/zh-CN/OBS-UDP-setup.md)
- FOV 尺寸表: [FOV-size.md](../../shared-guides/zh-CN/FOV-size.md)
