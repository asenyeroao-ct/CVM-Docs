# Aimbot 妯″紡: WindMouse

杩斿洖: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Gravity | `wm_gravity` | float | `0.1` 鍒?`30.0` | `9.0` | 涓?WindMouse 鍚戠洰鏍囧惛寮曞姏銆?| 鍊艰秺楂樿矾寰勮秺鐩存帴銆?|
| Wind | `wm_wind` | float | `0.1` 鍒?`20.0` | `3.0` | 涓?WindMouse 闅忔満鎵板姩寮哄害銆?| 鍊艰秺楂樿矾寰勯殢鏈烘€ц秺寮恒€?|
| Max Step | `wm_max_step` | float | `1` 鍒?`100` | `15.0` | 鍗曟鏇存柊鏈€澶ф闀裤€?| 闄愬埗绐佸彂浣嶇Щ銆?|
| Min Step | `wm_min_step` | float | `0.1` 鍒?`20` | `2.0` | 鍗曟鏇存柊鏈€灏忔闀裤€?| 闃叉绉诲姩鍋滄粸銆?|
| Min Delay (ms) | `wm_min_delay` | float (seconds in config) | UI `0.1` 鍒?`50` ms | `0.001` s | 鏇存柊闂撮殧鏈€灏忓欢杩熴€?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Max Delay (ms) | `wm_max_delay` | float (seconds in config) | UI `0.1` 鍒?`50` ms | `0.003` s | 鏇存柊闂撮殧鏈€澶у欢杩熴€?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Distance Threshold | `wm_distance_threshold` | float | `10` 鍒?`200` | `50.0` | 璺濈闃堝€硷紝鐢ㄤ簬鍒囨崲琛屼负銆?| 甯哥敤浜庤繎璺濈琛屼负杞崲銆?|
| FOV Size | `fovsize` | float | `1` 鍒?`1000` | `100` | 涓?WindMouse 妯″紡 FOV銆?| 涓庝富鍏朵粬妯″紡鍏辩敤姝ら敭銆?|

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Gravity | `wm_gravity_sec` | float | `0.1` 鍒?`30.0` | `9.0` | 鍓?WindMouse 鍚戠洰鏍囧惛寮曞姏銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Wind | `wm_wind_sec` | float | `0.1` 鍒?`20.0` | `3.0` | 鍓?WindMouse 闅忔満鎵板姩寮哄害銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Max Step | `wm_max_step_sec` | float | `1` 鍒?`100` | `15.0` | 鍓崟娆℃洿鏂版渶澶ф闀裤€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Min Step | `wm_min_step_sec` | float | `0.1` 鍒?`20` | `2.0` | 鍓崟娆℃洿鏂版渶灏忔闀裤€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Min Delay (ms) | `wm_min_delay_sec` | float (seconds in config) | UI `0.1` 鍒?`50` ms | `0.001` s | 鍓洿鏂伴棿闅旀渶灏忓欢杩熴€?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Max Delay (ms) | `wm_max_delay_sec` | float (seconds in config) | UI `0.1` 鍒?`50` ms | `0.003` s | 鍓洿鏂伴棿闅旀渶澶у欢杩熴€?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Distance Threshold | `wm_distance_threshold_sec` | float | `10` 鍒?`200` | `50.0` | 鍓窛绂婚槇鍊硷紝鐢ㄤ簬鍒囨崲琛屼负銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| FOV Size | `fovsize_sec` | float | `1` 鍒?`1000` | `150` | 鍓?WindMouse 妯″紡 FOV銆?| 涓庝富 `fovsize` 鐙珛銆?|

## Practical tuning tips

1. 先平衡力项：先调 `wm_gravity`，再加 `wm_wind` 到“自然但可控”的程度。
2. 轨迹太飘就降 `wm_wind`，轨迹太死板就小步升 `wm_wind`。
3. 用 `wm_max_step`/`wm_min_step` 限制节奏：不稳就降最大步长，近点卡顿就升最小步长。
4. `wm_min_delay` 与 `wm_max_delay` 建议设得接近，避免节奏忽快忽慢。
5. `wm_distance_threshold` 控制行为切换距离，近距切换太晚就适当调高。
6. `fovsize` 与 `fovsize_sec` 建议独立调，避免副模式抢主模式目标。
7. 副参数（`*_sec`）可先复制主参数，再略降激进度（如 gravity、max step）。
