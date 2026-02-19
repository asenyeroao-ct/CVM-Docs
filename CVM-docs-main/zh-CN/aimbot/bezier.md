# Aimbot 妯″紡: Bezier

杩斿洖: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Segments | `bezier_segments` | int | `1` 鍒?`30` | `8` | Bezier 璺緞閲囨牱娈垫暟銆?| 娈垫暟鏇撮珮閫氬父鎻掑€兼洿骞虫粦銆?|
| Ctrl X | `bezier_ctrl_x` | float | `0.0` 鍒?`100.0` | `16.0` | 鏇茬嚎璺緞姘村钩鎺у埗鐐瑰奖鍝嶉噺銆?| 涓?`Ctrl Y` 涓€璧峰喅瀹氬姬绾垮舰鐘躲€?|
| Ctrl Y | `bezier_ctrl_y` | float | `0.0` 鍒?`100.0` | `16.0` | 鏇茬嚎璺緞鍨傜洿鎺у埗鐐瑰奖鍝嶉噺銆?| 涓?`Ctrl X` 涓€璧峰喅瀹氬姬绾垮舰鐘躲€?|
| Speed | `bezier_speed` | float | `0.1` 鍒?`20.0` | `1.0` | 娌?Bezier 鏇茬嚎绉诲姩鐨勯€熷害鍊嶇巼銆?| 鏁板€艰秺楂橈紝娌挎洸绾跨Щ鍔ㄨ秺蹇€?|
| Delay (ms) | `bezier_delay` | float (seconds in config) | UI `0.1` 鍒?`50.0` ms | `0.002` s | Bezier 鍒嗘姝ヨ繘寤惰繜銆?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| FOV Size | `fovsize` | float | `1` 鍒?`1000` | `100` | 涓?Bezier 妯″紡浣跨敤鐨?FOV銆?| 涓庝富鍏朵粬妯″紡鍏辩敤姝ら敭銆?|

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Segments | `bezier_segments_sec` | int | `1` 鍒?`30` | `8` | 鍓?Bezier 璺緞娈垫暟銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Ctrl X | `bezier_ctrl_x_sec` | float | `0.0` 鍒?`100.0` | `16.0` | 鍓洸绾挎按骞虫帶鍒剁偣褰卞搷閲忋€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Ctrl Y | `bezier_ctrl_y_sec` | float | `0.0` 鍒?`100.0` | `16.0` | 鍓洸绾垮瀭鐩存帶鍒剁偣褰卞搷閲忋€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Speed | `bezier_speed_sec` | float | `0.1` 鍒?`20.0` | `1.0` | 鍓部鏇茬嚎绉诲姩閫熷害鍊嶇巼銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Delay (ms) | `bezier_delay_sec` | float (seconds in config) | UI `0.1` 鍒?`50.0` ms | `0.002` s | 鍓?Bezier 鍒嗘姝ヨ繘寤惰繜銆?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| FOV Size | `fovsize_sec` | float | `1` 鍒?`1000` | `150` | 鍓?Bezier 妯″紡浣跨敤鐨?FOV銆?| 涓庝富 `fovsize` 鐙珛銆?|

## Practical tuning tips

1. 先调曲线形状再调速度：先定 `bezier_ctrl_x`、`bezier_ctrl_y`。
2. `bezier_segments` 用来提高轨迹细腻度，建议逐步增加，过高会有延迟感。
3. 形状稳定后，再用 `bezier_speed` 调整体响应速度。
4. 如果整体偏慢，优先减 `bezier_delay`，再考虑提高速度。
5. 调曲线参数时先保持中等 `fovsize`，避免频繁换目标干扰判断。
6. 副模式可先复制主参数，再把 `bezier_speed_sec` 略降做稳妥兜底。
7. 调整 FPS/轮询节奏后要复测，因为 Bezier 步进时序对更新频率很敏感。
