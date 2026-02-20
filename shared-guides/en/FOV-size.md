# FOV Size Reference

Shared guide index: [README.md](./README.md)

Use these tables to map base resolution to cropped resolution.

## How to Use

`Base Resolution` corresponds to your desktop or game resolution. Find the table that matches your desktop resolution, then select your desired FOV value.

**Example**:
- If your desktop resolution is `2560x1440`, refer to the "## 2560x1440" table
- Then select your desired FOV value, for example `128`
- The corresponding `Cropped Resolution` is `1216x656`, which are the crop values you need to fill in the OBS `Crop/Pad` filter

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

## Resolution Reference Tables

Select the reference table for your resolution:

- [1920x1080 (1K)](./FovSize-res/1k.md)
- [2560x1440 (2K)](./FovSize-res/2k.md)
- [3440x1440 (21:9)](./FovSize-res/21-9.md)
- [3840x2160 (4K)](./FovSize-res/4k.md)

