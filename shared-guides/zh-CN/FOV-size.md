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

## 自訂解析度 FOV 計算器

輸入自訂寬高、選擇 FOV，即時計算裁剪解析度與 OBS Crop/Pad 數值。

<div id="fov-calc-zh" class="fov-calculator">
  <div style="display:grid;grid-template-columns:auto 1fr;gap:0.5rem 1rem;align-items:center;max-width:360px;margin:1rem 0;">
    <label for="fov-basew">寬度 (BaseW)</label>
    <input type="number" id="fov-basew" min="1" value="2560" placeholder="例如 2560">
    <label for="fov-baseh">高度 (BaseH)</label>
    <input type="number" id="fov-baseh" min="1" value="1440" placeholder="例如 1440">
    <label for="fov-select">FOV</label>
    <select id="fov-select">
      <option value="128">128</option>
      <option value="160">160</option>
      <option value="192">192</option>
      <option value="224">224</option>
      <option value="256">256</option>
      <option value="288">288</option>
      <option value="320">320</option>
      <option value="352">352</option>
      <option value="384">384</option>
      <option value="416">416</option>
      <option value="448">448</option>
      <option value="480">480</option>
      <option value="512">512</option>
      <option value="544">544</option>
      <option value="576">576</option>
      <option value="608">608</option>
      <option value="640">640</option>
      <option value="custom">自訂...</option>
    </select>
    <span id="fov-custom-wrap" style="display:none;grid-column:2;">
      <input type="number" id="fov-custom" min="1" placeholder="FOV 值" style="width:100px;">
    </span>
  </div>
  <div id="fov-result-zh" style="margin:1rem 0;padding:0.75rem;background:var(--md-default-bg-color);border-radius:6px;border:1px solid var(--md-default-fg-color--lightest);"></div>
</div>

<script>
(function() {
  var baseW = document.getElementById('fov-basew');
  var baseH = document.getElementById('fov-baseh');
  var sel = document.getElementById('fov-select');
  var customWrap = document.getElementById('fov-custom-wrap');
  var customIn = document.getElementById('fov-custom');
  var result = document.getElementById('fov-result-zh');
  function calc() {
    var w = parseInt(baseW.value, 10) || 0;
    var h = parseInt(baseH.value, 10) || 0;
    var fov = sel.value === 'custom' ? (parseInt(customIn.value, 10) || 0) : (parseInt(sel.value, 10) || 0);
    if (w <= 0 || h <= 0 || fov <= 0) {
      result.innerHTML = '請輸入有效的寬、高與 FOV（正整數）。';
      return;
    }
    if (fov >= w || fov >= h) {
      result.innerHTML = 'FOV 須小於寬度與高度，請調整。';
      return;
    }
    var outW = Math.floor((w - fov) / 2);
    var outH = Math.floor((h - fov) / 2);
    var leftRight = Math.floor((w + fov) / 4);
    var topBottom = Math.floor((h + fov) / 4);
    result.innerHTML = '<strong>裁剪解析度 (Cropped Resolution)</strong>: ' + outW + '×' + outH +
      ' &nbsp;|&nbsp; <strong>OBS Crop/Pad</strong>: Left=Right=' + leftRight + ', Top=Bottom=' + topBottom;
  }
  sel.addEventListener('change', function() {
    customWrap.style.display = sel.value === 'custom' ? 'block' : 'none';
    if (sel.value !== 'custom') customIn.value = '';
    calc();
  });
  customIn.addEventListener('input', calc);
  baseW.addEventListener('input', calc);
  baseH.addEventListener('input', calc);
  calc();
})();
</script>

## 解析度對照表

請根據您的解析度選擇對應的對照表：

- [1920x1080 (1K)](./FovSize-res/1k.md)
- [2560x1440 (2K)](./FovSize-res/2k.md)
- [3440x1440 (21:9)](./FovSize-res/21-9.md)
- [3840x2160 (4K)](./FovSize-res/4k.md)

