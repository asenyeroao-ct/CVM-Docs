# Aimbot 妯″紡: Silent

杩斿洖: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Distance (Multiplier) | `silent_distance` | float | `0.1` 鍒?`10.0` | `1.0` | 鐩爣淇璺濈鍊嶇巼銆?| 鍊艰秺澶э紝淇骞呭害鍙兘瓒婂ぇ銆?|
| Delay (ms) | `silent_delay` | float | `0.001` 鍒?`300.0` ms | `100.0` | Silent 鍔ㄤ綔寮€濮嬪墠寤惰繜銆?| 褰撳墠閰嶇疆浠ユ绉掑€间繚瀛樸€?|
| Move Delay (ms) | `silent_move_delay` | float | `0.001` 鍒?`300.0` ms | `500.0` | 绉诲姩鍒扮洰鏍囬樁娈靛欢杩熴€?| 鍊艰秺澶э紝Silent 搴忓垪鍝嶅簲瓒婃參銆?|
| Return Delay (ms) | `silent_return_delay` | float | `0.001` 鍒?`300.0` ms | `500.0` | 鍥炰綅闃舵寤惰繜銆?| 寤鸿涓?Move Delay 鑱斿悎璋冩暣銆?|
| FOV Size | `fovsize` | float | `1` 鍒?`1000` | `100` | 涓?Silent 妯″紡浣跨敤鐨?FOV銆?| 涓庝富鍏朵粬妯″紡鍏辩敤姝ら敭銆?|

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed_sec` | float | `0.1` 鍒?`2000` | `2` | 褰撳墠 UI 涓嬪壇 Silent 浣跨敤璇?X 閫熷害閿€?| 鏍囩椤垫湭鏆撮湶鐙珛 `silent_*_sec` 鍙傛暟銆?|
| Y-Speed | `normal_y_speed_sec` | float | `0.1` 鍒?`2000` | `2` | 褰撳墠 UI 涓嬪壇 Silent 浣跨敤璇?Y 閫熷害閿€?| 鏍囩椤垫湭鏆撮湶鐙珛 `silent_*_sec` 鍙傛暟銆?|
| FOV Size | `fovsize_sec` | float | `1` 鍒?`1000` | `150` | 褰撳墠 UI 涓嬪壇 Silent 浣跨敤璇?FOV 閿€?| 涓庝富 `fovsize` 鐙珛銆?|

## Practical tuning tips

1. 先定 `fovsize`；FOV 过大时，复杂场景下 Silent 触发会更不稳定。
2. 延迟建议按顺序调：`silent_delay` -> `silent_move_delay` -> `silent_return_delay`。
3. 如果触发明显偏晚，先减 `silent_delay`；如果移动阶段拖沓，再减 `silent_move_delay`。
4. 回位不自然时，只改 `silent_return_delay`，不要一次联动全部延迟。
5. `silent_distance` 建议先围绕 `1.0` 微调，确认修正不足再逐步增加。
6. 副 Silent 目前用的是 `normal_x_speed_sec`、`normal_y_speed_sec`、`fovsize_sec`，请保守调节。
7. 务必在近/中/远三种距离都验证一次，避免只在单一距离有效。
