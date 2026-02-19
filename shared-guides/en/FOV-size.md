# FOV Size Reference

Shared guide index: [README.md](./README.md)

Use these tables to map base resolution to cropped resolution.
## Formula (Any Aspect Ratio)

If your resolution is not listed below, calculate it with this formula.

Given:

- `BaseW` = base width
- `BaseH` = base height
- `FOV` = selected FOV value

Output resolution:

```text
CroppedW = (BaseW - FOV) / 2
CroppedH = (BaseH - FOV) / 2
```

If you need OBS `Crop/Pad` values (center crop):

```text
Left   = Right  = (BaseW + FOV) / 4
Top    = Bottom = (BaseH + FOV) / 4
```

Validation:

- `FOV < BaseW` and `FOV < BaseH` (otherwise output becomes zero/negative)
- Use even numbers to avoid decimals (`32`-step values are commonly used)

Example (also works for 21:9):

```text
BaseW = 5120, BaseH = 1440, FOV = 320
CroppedW = (5120 - 320) / 2 = 2400
CroppedH = (1440 - 320) / 2 = 560
Left=Right=(5120+320)/4=1360
Top=Bottom=(1440+320)/4=440
```

## 1920x1080

| FOV | Base Resolution | Cropped Resolution |
| --- | --- | --- |
| 128 | 1920x1080 | 896x476 |
| 160 | 1920x1080 | 880x460 |
| 192 | 1920x1080 | 864x444 |
| 224 | 1920x1080 | 848x428 |
| 256 | 1920x1080 | 832x412 |
| 288 | 1920x1080 | 816x396 |
| 320 | 1920x1080 | 800x380 |
| 352 | 1920x1080 | 784x364 |
| 384 | 1920x1080 | 768x348 |
| 416 | 1920x1080 | 752x332 |
| 448 | 1920x1080 | 736x316 |
| 480 | 1920x1080 | 720x300 |
| 512 | 1920x1080 | 704x284 |
| 544 | 1920x1080 | 688x268 |
| 576 | 1920x1080 | 672x252 |
| 608 | 1920x1080 | 656x236 |
| 640 | 1920x1080 | 640x220 |

## 2560x1440

| FOV | Base Resolution | Cropped Resolution |
| --- | --- | --- |
| 128 | 2560x1440 | 1216x656 |
| 160 | 2560x1440 | 1200x640 |
| 192 | 2560x1440 | 1184x624 |
| 224 | 2560x1440 | 1168x608 |
| 256 | 2560x1440 | 1152x592 |
| 288 | 2560x1440 | 1136x576 |
| 320 | 2560x1440 | 1120x560 |
| 352 | 2560x1440 | 1104x544 |
| 384 | 2560x1440 | 1088x528 |
| 416 | 2560x1440 | 1072x512 |
| 448 | 2560x1440 | 1056x496 |
| 480 | 2560x1440 | 1040x480 |
| 512 | 2560x1440 | 1024x464 |
| 544 | 2560x1440 | 1008x448 |
| 576 | 2560x1440 | 992x432 |
| 608 | 2560x1440 | 976x416 |
| 640 | 2560x1440 | 960x400 |

## 3440x1440

| FOV | Base Resolution | Cropped Resolution |
| --- | --- | --- |
| 128 | 3440x1440 | 1656x656 |
| 160 | 3440x1440 | 1640x640 |
| 192 | 3440x1440 | 1624x624 |
| 224 | 3440x1440 | 1608x608 |
| 256 | 3440x1440 | 1592x592 |
| 288 | 3440x1440 | 1576x576 |
| 320 | 3440x1440 | 1560x560 |
| 352 | 3440x1440 | 1544x544 |
| 384 | 3440x1440 | 1528x528 |
| 416 | 3440x1440 | 1512x512 |
| 448 | 3440x1440 | 1496x496 |
| 480 | 3440x1440 | 1480x480 |
| 512 | 3440x1440 | 1464x464 |
| 544 | 3440x1440 | 1448x448 |
| 576 | 3440x1440 | 1432x432 |
| 608 | 3440x1440 | 1416x416 |
| 640 | 3440x1440 | 1400x400 |

## 3840x2160

| FOV | Base Resolution | Cropped Resolution |
| --- | --- | --- |
| 128 | 3840x2160 | 1856x1016 |
| 160 | 3840x2160 | 1840x1000 |
| 192 | 3840x2160 | 1824x984 |
| 224 | 3840x2160 | 1808x968 |
| 256 | 3840x2160 | 1792x952 |
| 288 | 3840x2160 | 1776x936 |
| 320 | 3840x2160 | 1760x920 |
| 352 | 3840x2160 | 1744x904 |
| 384 | 3840x2160 | 1728x888 |
| 416 | 3840x2160 | 1712x872 |
| 448 | 3840x2160 | 1696x856 |
| 480 | 3840x2160 | 1680x840 |
| 512 | 3840x2160 | 1664x824 |
| 544 | 3840x2160 | 1648x808 |
| 576 | 3840x2160 | 1632x792 |
| 608 | 3840x2160 | 1616x776 |
| 640 | 3840x2160 | 1600x760 |

