# General 标签参数说明

返回索引: [CVM.md](./CVM.md)

## Hardware API

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Input API | `mouse_api` | enum (string) | `Serial`, `Arduino`, `SendInput`, `Net`, `KmboxA`, `MakV2`, `MakV2Binary`, `DHZ` | `Serial` | 选择鼠标控制后端。 | MAKCU 与 Ferrum 都使用此选项。 |
| Auto Connect Mouse API On Startup | `auto_connect_mouse_api` | bool | `true` / `false` | `false` | 启动时自动连接已选鼠标后端。 | 适合固定硬件环境。 |
| Auto Switch Serial to 4M On Startup | `serial_auto_switch_4m` | bool | `true` / `false` | `false` | 启动时在 Serial 自动连接后尝试切换到 4M 波特率。 | 仅在 `mouse_api=Serial` 时生效。 |
| COM Mode | `serial_port_mode` | enum (string) | `Auto`, `Manual` | `Auto` | Serial API 的串口选择模式。 | 仅在 `mouse_api=Serial` 时生效。 |
| COM Port | `serial_port` | string | 例如 `COM3` | `""` | Serial API 手动串口号。 | 仅 Serial + Manual 模式使用。 |
| COM Port (optional) | `arduino_port` | string | 例如 `COM4` | `""` | Arduino 串口号。 | 留空时由后端逻辑自动探测。 |
| Baud | `arduino_baud` | int | 正整数 | `115200` | Arduino 波特率。 | 输入非法会回退为 `115200`。 |
| IP Address | `net_ip` | string | IPv4 / 主机名 | `192.168.2.188` | Net API 的目标地址。 | 与 `net_port`、`net_uuid` 配合使用。 |
| Port | `net_port` | string | 数字字符串 | `6234` | Net API 的目标端口。 | 在配置中按字符串保存。 |
| UUID | `net_uuid` | string | 设备 UUID/MAC 类字符串 | `""` | Net API 的设备标识。 | `net_mac` 是已弃用别名，会与 `net_uuid` 同步。 |
| VID/PID | `kmboxa_vid_pid` | string | 例如 `66882021`、`6688/2021`、`V984D741`、`0x1A20/0x07E5` | `"0/0"` | KmboxA 的组合设备 ID 输入，用于 API init。 | UI 为单一输入框；后端会解析并同步 `kmboxa_vid` + `kmboxa_pid` 以保持兼容。 |
| Port (optional) | `makv2_port` | string | 例如 `COM5` | `""` | MakV2 API 串口号。 | 留空时允许后端走默认探测流程。 |
| Baud | `makv2_baud` | int | 正整数 | `4000000` | MakV2 API 波特率。 | 输入非法会回退为 `4000000`。 |
| IP Address | `dhz_ip` | string | IPv4 / 主机名 | `192.168.2.188` | DHZ API 的目标地址。 | 通过 UDP 风格命令协议发送。 |
| Port | `dhz_port` | string | 数字字符串 | `5000` | DHZ API 的目标端口。 | 在配置中按字符串保存。 |
| Random Shift | `dhz_random` | int | 整数 `>=0` | `0` | DHZ 命令凯撒移位随机参数。 | 输入非法会回退为 `0`。 |

## Capture Controls

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Method | runtime (`capture.mode`) | enum (string) | `NDI`, `UDP`, `CaptureCard`, `MSS` | runtime value | 选择视频采集后端。 | 来自 `CaptureService` 的运行时状态；本表中不作为固定配置键。 |
| Target FPS | `target_fps` | float | `1` 到 `1000` | `80` | 限制追踪/处理循环频率。 | UI 输入非法会重置为 `80`。 |
| NDI Source | `last_ndi_source` | string or null | 已发现的 NDI 源名 | `null` | 记忆首选 NDI 源以便重连。 | 连接选中 NDI 源时更新。 |
| Enable Center Crop (NDI) | `ndi_fov_enabled` | bool | `true` / `false` | `false` | 开启 NDI 画面的中心正方形裁切。 | 裁切尺寸由 `ndi_fov` 控制。 |
| FOV (NDI half-size) | `ndi_fov` | int | 滑块 `16` 到 `1920` | `320` | NDI 裁切区域的半边长。 | 实际裁切边长为 `2 * ndi_fov`。 |
| UDP IP Address | `udp_ip` | string | IPv4 / 主机名 | `127.0.0.1` | UDP 流采集地址。 | 这里填写 second PC（运行本软件的这台电脑）的 IP。 |
| UDP Port | `udp_port` | string | 数字字符串 | `1234` | UDP 流采集端口。 | 在配置中按字符串保存。 |
| Enable Center Crop (UDP) | `udp_fov_enabled` | bool | `true` / `false` | `false` | 开启 UDP 画面的中心正方形裁切。 | 裁切尺寸由 `udp_fov` 控制。 |
| FOV (UDP half-size) | `udp_fov` | int | 滑块 `16` 到 `1920` | `320` | UDP 裁切区域的半边长。 | 实际裁切边长为 `2 * udp_fov`。 |
| Device Index | `capture_device_index` | int | 整数 `>=0` | `0` | 采集卡设备索引。 | 仅 CaptureCard 模式使用。 |
| Resolution Width | `capture_width` | int | 正整数 | `1920` | 采集卡宽度设置。 | 与 `capture_height` 成对使用。 |
| Resolution Height | `capture_height` | int | 正整数 | `1080` | 采集卡高度设置。 | 与 `capture_width` 成对使用。 |
| FPS | `capture_fps` | float | 正数 | `240` | 采集卡输入 FPS 目标。 | 实际值可能被硬件或驱动限制。 |
| CaptureCard Force BGR | `capture_card_force_bgr` | bool | `true` / `false` | `true` | 在进入 HSV 检测前，强制将 CaptureCard 帧标准化为 BGR。 | 建议保持开启，以减少不同采集卡/驱动导致的颜色漂移。 |
| CaptureCard Set Convert RGB | `capture_card_set_convert_rgb` | bool | `true` / `false` | `true` | 尝试通过 `CAP_PROP_CONVERT_RGB` 请求后端执行颜色转换。 | 不同后端支持度不同，不支持时会自动忽略。 |
| CaptureCard Probe Frames | `capture_card_probe_frames` | int | 整数 `>=1` | `3` | 初始化时每个 FourCC 的探测帧数量。 | 探测失败会自动 fallback 到下一个优先 FourCC。 |
| CaptureCard Debug Color Log | `capture_card_debug_color_log` | bool | `true` / `false` | `false` | 输出 CaptureCard 色彩格式调试日志。 | 仅建议排障时开启，日志量较大。 |
| Range X | `capture_range_x` | int | 最小 `128` | `128` | CaptureCard 工作流中的采集区域宽度基线。 | 失焦时 UI 会强制最小值 `128`。 |
| Range Y | `capture_range_y` | int | 最小 `128` | `128` | CaptureCard 工作流中的采集区域高度基线。 | 失焦时 UI 会强制最小值 `128`。 |
| Monitor Index | `mss_monitor_index` | int | 整数 `>=1` | `1` | MSS 目标显示器索引。 | 索引顺序依赖本机显示器列表。 |
| FOV X (MSS half-width) | `mss_fov_x` | int | 滑块 `16` 到 `1920` | `320` | MSS 中心区域半宽。 | 实际宽度为 `2 * mss_fov_x`。 |
| FOV Y (MSS half-height) | `mss_fov_y` | int | 滑块 `16` 到 `1080` | `320` | MSS 中心区域半高。 | 实际高度为 `2 * mss_fov_y`。 |

## Settings

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| In-Game Sensitivity | `in_game_sens` | float | `0.1` 到 `20` | `0.235` | 用于移动换算的游戏内灵敏度参考值。 | 请与游戏内真实灵敏度匹配。 |
| Target Color | `color` | enum (string) | `yellow`, `purple`, `red`, `custom` | `purple` | 选择 HSV 目标颜色方案。 | 选择 `custom` 时会启用 Custom HSV 区块。 |

## Custom HSV

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| H Min | `custom_hsv_min_h` | int | `0` 到 `179` | `0` | 自定义掩码 Hue 下限。 | 仅在 `color=custom` 时生效。 |
| S Min | `custom_hsv_min_s` | int | `0` 到 `255` | `0` | 自定义掩码 Saturation 下限。 | 仅在 `color=custom` 时生效。 |
| V Min | `custom_hsv_min_v` | int | `0` 到 `255` | `0` | 自定义掩码 Value 下限。 | 仅在 `color=custom` 时生效。 |
| H Max | `custom_hsv_max_h` | int | `0` 到 `179` | `179` | 自定义掩码 Hue 上限。 | 仅在 `color=custom` 时生效。 |
| S Max | `custom_hsv_max_s` | int | `0` 到 `255` | `255` | 自定义掩码 Saturation 上限。 | 仅在 `color=custom` 时生效。 |
| V Max | `custom_hsv_max_v` | int | `0` 到 `255` | `255` | 自定义掩码 Value 上限。 | 仅在 `color=custom` 时生效。 |

## Detection Parameters

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Merge Distance | `detection_merge_distance` | int | UI 滑块 `50` 到 `500` | `12` | 合并邻近检测框的距离阈值。 | Tooltip 推荐 `200` 到 `300`（说明文本默认示例 `250`）。 |
| Min Contour Points | `detection_min_contour_points` | int | UI 滑块 `3` 到 `100` | `5` | 过滤点数过少的轮廓（降噪）。 | Tooltip 推荐 `3` 到 `10`（默认 `5`）。 |

## Mouse Lock

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Lock Main Aimbot X-Axis | `mouse_lock_main_x` | bool | `true` / `false` | `false` | 主自瞄主动控制时锁定物理鼠标 X 轴。 | 当自瞄停止驱动移动时自动释放。 |
| Lock Main Aimbot Y-Axis | `mouse_lock_main_y` | bool | `true` / `false` | `false` | 主自瞄主动控制时锁定物理鼠标 Y 轴。 | 当自瞄停止驱动移动时自动释放。 |
| Lock Sec Aimbot X-Axis | `mouse_lock_sec_x` | bool | `true` / `false` | `false` | 副自瞄主动控制时锁定物理鼠标 X 轴。 | 当自瞄停止驱动移动时自动释放。 |
| Lock Sec Aimbot Y-Axis | `mouse_lock_sec_y` | bool | `true` / `false` | `false` | 副自瞄主动控制时锁定物理鼠标 Y 轴。 | 当自瞄停止驱动移动时自动释放。 |

## Button Mask

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Enable Button Mask | `button_mask_enabled` | bool | `true` / `false` | `false` | 启用按键级别遮罩/锁定策略。 | 下方按键项的总开关。 |
| L-Click | `mask_left_button` | bool | `true` / `false` | `false` | 左键遮罩策略。 | 内部索引 `0`。 |
| R-Click | `mask_right_button` | bool | `true` / `false` | `false` | 右键遮罩策略。 | 内部索引 `1`。 |
| M-Click | `mask_middle_button` | bool | `true` / `false` | `false` | 中键遮罩策略。 | 内部索引 `2`。 |
| Side 4 | `mask_side4_button` | bool | `true` / `false` | `false` | 侧键 4 遮罩策略。 | 内部索引 `3`。 |
| Side 5 | `mask_side5_button` | bool | `true` / `false` | `false` | 侧键 5 遮罩策略。 | 内部索引 `4`。 |

## 推荐设置顺序

1. 先确定 `mouse_api`，并确认设备可稳定连接与重连。
2. 选择采集方式（`NDI`/`UDP`/`CaptureCard`/`MSS`），先固定稳定的分辨率与帧率。
3. `target_fps` 以实际采集吞吐为准，不要只按显示器刷新率设置。
4. 在调自瞄/扳机前，先确认目标颜色配置（`color`）正确。
5. 使用 `custom` 颜色时，先收紧 HSV 区间，再调检测合并和降噪阈值。
6. 鼠标锁轴与按钮屏蔽建议在基础追踪稳定后再开启。

## 关键参数联动

- `target_fps` 与采集 FPS：`target_fps` 远高于真实采集帧率时，容易出现重复帧导致手感抖动。
- `ndi_fov` / `udp_fov` / `mss_fov_x` 与模式 FOV（`fovsize`/`fovsize_sec`）：采集裁切过小会让目标直接不进入检测区。
- `color=custom` 与 `detection_min_contour_points`：HSV 范围过宽且轮廓点阈值过低时，误检通常会明显增加。
- `detection_merge_distance` 与 `trigger_min_pixels`：合并距离过大可能抬高像素计数，导致扳机提前触发。
- 鼠标锁（`mouse_lock_*`）与激活模式（`hold_disable`/`toggle`）：务必实测释放条件，避免出现输入被意外锁住。

## 快速排查

| 现象 | 先检查 | 再调整 |
|---|---|---|
| 首次启动能连设备，重启后偶发连不上 | `auto_connect_mouse_api`、COM 模式和端口是否一致 | `serial_auto_switch_4m` 与对应后端波特率 |
| 画面能看到目标色，但检测不到 | `color` 预设、采集色彩转换相关开关 | Custom HSV 区间、`detection_min_contour_points` |
| 复杂背景下误检太多 | 采集裁切大小、颜色预设是否合理 | 提高 `trigger_min_ratio`，并上调轮廓/像素阈值 |
| 不同模式下行为差异很大 | 当前采集后端与模式相关键 | 复查共享参数（`fovsize`、offset、激活键） |
| 延迟感明显或时快时慢 | 实际采集 FPS 与 `target_fps` 是否匹配 | 再调各模式文档中的延迟与平滑参数 |
