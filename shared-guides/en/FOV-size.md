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

## Custom Resolution FOV Calculator

Enter custom width/height and select FOV to get cropped resolution and OBS Crop/Pad values.

<div id="fov-calc-en" class="fov-calculator">
  <div style="display:grid;grid-template-columns:auto 1fr;gap:0.5rem 1rem;align-items:center;max-width:360px;margin:1rem 0;">
    <label for="fov-basew-en">Width (BaseW)</label>
    <input type="number" id="fov-basew-en" min="1" value="2560" placeholder="e.g. 2560">
    <label for="fov-baseh-en">Height (BaseH)</label>
    <input type="number" id="fov-baseh-en" min="1" value="1440" placeholder="e.g. 1440">
    <label for="fov-select-en">FOV</label>
    <select id="fov-select-en">
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
      <option value="custom">Custom...</option>
    </select>
    <span id="fov-custom-wrap-en" style="display:none;grid-column:2;">
      <input type="number" id="fov-custom-en" min="1" placeholder="FOV value" style="width:100px;">
    </span>
  </div>
  <div id="fov-result-en" style="margin:1rem 0;padding:0.75rem;background:var(--md-default-bg-color);border-radius:6px;border:1px solid var(--md-default-fg-color--lightest);"></div>
</div>

<script>
(function() {
  var baseW = document.getElementById('fov-basew-en');
  var baseH = document.getElementById('fov-baseh-en');
  var sel = document.getElementById('fov-select-en');
  var customWrap = document.getElementById('fov-custom-wrap-en');
  var customIn = document.getElementById('fov-custom-en');
  var result = document.getElementById('fov-result-en');
  function calc() {
    var w = parseInt(baseW.value, 10) || 0;
    var h = parseInt(baseH.value, 10) || 0;
    var fov = sel.value === 'custom' ? (parseInt(customIn.value, 10) || 0) : (parseInt(sel.value, 10) || 0);
    if (w <= 0 || h <= 0 || fov <= 0) {
      result.innerHTML = 'Please enter valid width, height and FOV (positive integers).';
      return;
    }
    if (fov >= w || fov >= h) {
      result.innerHTML = 'FOV must be less than width and height.';
      return;
    }
    var outW = Math.floor((w - fov) / 2);
    var outH = Math.floor((h - fov) / 2);
    var leftRight = Math.floor((w + fov) / 4);
    var topBottom = Math.floor((h + fov) / 4);
    result.innerHTML = '<strong>Cropped Resolution</strong>: ' + outW + '×' + outH +
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

## Resolution Reference Tables

Select the reference table for your resolution:

- [1920x1080 (1K)](./FovSize-res/1k.md)
- [2560x1440 (2K)](./FovSize-res/2k.md)
- [3440x1440 (21:9)](./FovSize-res/21-9.md)
- [3840x2160 (4K)](./FovSize-res/4k.md)

