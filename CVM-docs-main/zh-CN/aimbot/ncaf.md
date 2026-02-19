# Aimbot 妯″紡: NCAF

杩斿洖: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Alpha (Speed Curve) | `ncaf_alpha` | float | `0.1` 鍒?`5.0` | `1.5` | NCAF 閫熷害鏇茬嚎褰㈢姸鍙傛暟銆?| 鍊艰秺楂樹細鏀瑰彉閫熷害鐖崌鏇茬嚎銆?|
| Snap Boost Factor | `ncaf_snap_boost` | float | `0.01` 鍒?`2.0` | `0.3` | 瑙﹀彂 snap 鏉′欢鏃剁殑棰濆鍔犻€熷洜瀛愩€?| 鍙彁鍗囬攣瀹氭椂鐨勬縺杩涚▼搴︺€?|
| Max Step | `ncaf_max_step` | float | `1` 鍒?`200` | `50.0` | 鍗曟鏇存柊鏈€澶хЩ鍔ㄦ闀裤€?| 闄愬埗绐佸彂澶у箙浣嶇Щ銆?|
| Min Speed Multiplier | `ncaf_min_speed_multiplier` | float | `0.01` 鍒?`1.0` | `0.01` | 鑷€傚簲閫熷害鍊嶇巼涓嬮檺銆?| 闃叉绉诲姩杩囨參銆?|
| Max Speed Multiplier | `ncaf_max_speed_multiplier` | float | `1.0` 鍒?`20.0` | `10.0` | 鑷€傚簲閫熷害鍊嶇巼涓婇檺銆?| 闃叉绉诲姩杩囧揩銆?|
| Prediction Interval (ms) | `ncaf_prediction_interval` | float (seconds in config) | UI `1` 鍒?`100` ms | `0.016` s | NCAF 鐩爣棰勬祴鏃堕棿闂撮殧銆?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Snap Radius (Outer) | `ncaf_snap_radius` | float | `10` 鍒?`500` | `150.0` | 澶栧眰 snap 浣滅敤鍗婂緞銆?| 搴斿ぇ浜?near radius銆?|
| Near Radius (Inner) | `ncaf_near_radius` | float | `5` 鍒?`400` | `50.0` | 鍐呭眰杩戣窛绂荤簿缁嗘帶鍒跺崐寰勩€?| 搴斿皬浜?snap radius銆?|

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Alpha (Speed Curve) | `ncaf_alpha_sec` | float | `0.1` 鍒?`5.0` | `1.5` | 鍓?NCAF 閫熷害鏇茬嚎鍙傛暟銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Snap Boost Factor | `ncaf_snap_boost_sec` | float | `0.01` 鍒?`2.0` | `0.3` | 鍓?NCAF snap 鍔犻€熷洜瀛愩€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Max Step | `ncaf_max_step_sec` | float | `1` 鍒?`200` | `50.0` | 鍓?NCAF 鍗曟鏇存柊鏈€澶ф闀裤€?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Min Speed Multiplier | `ncaf_min_speed_multiplier_sec` | float | `0.01` 鍒?`1.0` | `0.01` | 鍓?NCAF 閫熷害鍊嶇巼涓嬮檺銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Max Speed Multiplier | `ncaf_max_speed_multiplier_sec` | float | `1.0` 鍒?`20.0` | `10.0` | 鍓?NCAF 閫熷害鍊嶇巼涓婇檺銆?| 涓庝富鑷瀯鍙傛暟鐙珛銆?|
| Prediction Interval (ms) | `ncaf_prediction_interval_sec` | float (seconds in config) | UI `1` 鍒?`100` ms | `0.016` s | 鍓?NCAF 棰勬祴鏃堕棿闂撮殧銆?| UI 浠ユ绉掓樉绀猴紝閰嶇疆浠ョ淇濆瓨銆?|
| Snap Radius (Outer) | `ncaf_snap_radius_sec` | float | `10` 鍒?`500` | `150.0` | 鍓?NCAF 澶栧眰 snap 鍗婂緞銆?| 搴斿ぇ浜庡壇 near radius銆?|
| Near Radius (Inner) | `ncaf_near_radius_sec` | float | `5` 鍒?`400` | `50.0` | 鍓?NCAF 鍐呭眰 near 鍗婂緞銆?| 搴斿皬浜庡壇 snap radius銆?|

## Practical tuning tips

1. 先定几何关系：先定 `ncaf_snap_radius`，再定明显更小的 `ncaf_near_radius`。
2. 再调运动上限：若跳变明显，优先降低 `ncaf_max_step`。
3. 再调曲线形状：`ncaf_alpha` 建议小步改（每次 `0.1` 到 `0.2`）。
4. 倍率做边界保护：近距离跟不上可微升 `ncaf_min_speed_multiplier`；过猛可降 `ncaf_max_speed_multiplier`。
5. `ncaf_snap_boost` 建议最后调，过高会在阈值附近产生突兀吸附。
6. `ncaf_prediction_interval` 要接近你的帧节奏，预测不稳就逐步下调。
7. `*_sec` 参数按同顺序调，但建议比主自瞄略保守，确保副模式稳定兜底。
