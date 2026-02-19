# Aimbot 妯″紡: Normal

杩斿洖: [Aimbot](../Aimbot.md) | [Sec Aimbot](../Sec-Aimbot.md)

## Main Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed` | float | `0.1` 鍒?`2000` | `3` | 涓昏嚜鐬勬按骞宠窡韪€熷害銆?| 鏁板€艰秺澶э紝鍝嶅簲瓒婂揩銆?|
| Y-Speed | `normal_y_speed` | float | `0.1` 鍒?`2000` | `3` | 涓昏嚜鐬勫瀭鐩磋窡韪€熷害銆?| 寤鸿涓?X-Speed 骞宠　璋冩暣銆?|
| Smoothing | `normalsmooth` | float | `1` 鍒?`30` | `30` | 涓昏嚜鐬勭Щ鍔ㄥ钩婊戝己搴︺€?| 鏁板€艰秺楂橀€氬父瓒婄ǔ浣嗘洿鎱€?|
| FOV Size | `fovsize` | float | `1` 鍒?`1000` | `100` | 涓昏嚜鐬勮閲庤寖鍥村ぇ灏忋€?| 鎺у埗鍊欓€夌洰鏍囧尯鍩熴€?|
| FOV Smooth | `normalsmoothfov` | float | `1` 鍒?`30` | `30` | 涓昏嚜鐬?FOV 杩囨浮骞虫粦銆?| 鍙噺灏?FOV 杈圭紭绐佸彉銆?|

## Sec Aimbot

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| X-Speed | `normal_x_speed_sec` | float | `0.1` 鍒?`2000` | `2` | 鍓嚜鐬勬按骞宠窡韪€熷害銆?| 涓庝富鑷瀯閫熷害鐙珛銆?|
| Y-Speed | `normal_y_speed_sec` | float | `0.1` 鍒?`2000` | `2` | 鍓嚜鐬勫瀭鐩磋窡韪€熷害銆?| 涓庝富鑷瀯閫熷害鐙珛銆?|
| Smoothing | `normalsmooth_sec` | float | `1` 鍒?`30` | `20` | 鍓嚜鐬勭Щ鍔ㄥ钩婊戝己搴︺€?| 涓庝富鑷瀯骞虫粦鐙珛銆?|
| FOV Size | `fovsize_sec` | float | `1` 鍒?`1000` | `150` | 鍓嚜鐬勮閲庤寖鍥村ぇ灏忋€?| 涓庝富鑷瀯 FOV 鐙珛銆?|
| FOV Smooth | `normalsmoothfov_sec` | float | `1` 鍒?`30` | `20` | 鍓嚜鐬?FOV 杩囨浮骞虫粦銆?| 涓庝富鑷瀯 FOV 骞虫粦鐙珛銆?|

## Practical tuning tips

1. 先保持默认值，在固定测试场景连续测试 2-3 分钟后再改参数。
2. 先单独调 `normal_x_speed`（只看水平目标），再调 `normal_y_speed`（补垂直偏差）。
3. 如果出现过冲，先小步降低速度（每次约 `0.2` 到 `0.5`），再考虑加平滑。
4. `normalsmooth` 用来压微抖；如果变得太迟缓，先小降平滑再微加速度。
5. `fovsize` 只在目标重捕慢时再加大；若误选目标多，优先缩小 FOV。
6. 如果副自瞄是兜底，建议 `*_sec` 参数比主自瞄稍慢、稍平滑。
7. 游戏内灵敏度变化后（`in_game_sens`）通常要重新微调速度与平滑。
