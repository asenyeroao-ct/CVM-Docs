# FOV Size 对照表

共享指南索引: [README.md](./README.md)

以下表格用于根据基础分辨率选择对应裁剪输出分辨率。

## 如何使用

`Base Resolution` 對應你的桌面或遊戲解析度。根據你的桌面解析度找到對應的表格，然後選擇你要的 FOV 值。

**示例**:
- 如果你的桌面解析度是 `2560x1440`，請查看「## 2560x1440」表格
- 然後選擇你要的 FOV 值，例如 `128`
- 對應的 `Cropped Resolution` 為 `1216x656`，這就是你在 OBS `Crop/Pad` 濾鏡中需要填寫的裁剪值

## 通用算法（适用于任意画面比例）

如果你的分辨率不在下方表格中，可以按下面算法自己计算。

已知:

- `BaseW` = 基础宽度（Base Resolution Width）
- `BaseH` = 基础高度（Base Resolution Height）
- `FOV` = 你要使用的 FOV 值

计算输出分辨率:

```text
CroppedW = (BaseW - FOV) / 2
CroppedH = (BaseH - FOV) / 2
```

如果你要填写 OBS `Crop/Pad`（居中裁剪）:

```text
Left   = Right  = (BaseW + FOV) / 4
Top    = Bottom = (BaseH + FOV) / 4
```

校验条件:

- `FOV < BaseW` 且 `FOV < BaseH`（否则计算结果会变成 0 或负数）
- 建议使用偶数值，避免出现小数（常见使用 `32` 的倍数）

示例（21:9 也适用）:

```text
BaseW = 5120, BaseH = 1440, FOV = 320
CroppedW = (5120 - 320) / 2 = 2400
CroppedH = (1440 - 320) / 2 = 560
Left=Right=(5120+320)/4=1360
Top=Bottom=(1440+320)/4=440
```

## 解析度對照表

請根據您的解析度選擇對應的對照表：

- [1920x1080 (1K)](./FovSize-res/1k.md)
- [2560x1440 (2K)](./FovSize-res/2k.md)
- [3440x1440 (21:9)](./FovSize-res/21-9.md)
- [3840x2160 (4K)](./FovSize-res/4k.md)

