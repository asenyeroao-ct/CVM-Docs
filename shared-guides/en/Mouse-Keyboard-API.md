# Mouse Backend Keyboard API

Back to shared guides: [README.md](./README.md)

This project now exposes a unified keyboard API in `src/utils/mouse`.

## Unified Functions

Use either module-level functions or `Mouse` methods:

```python
from src.utils.mouse import (
    key_down, key_up, key_press, is_key_pressed,
    mask_key, unmask_key, unmask_all_keys,
)
from src.utils.mouse import Mouse

key_down("A")
key_up("A")
key_press("SPACE")
print(is_key_pressed("LCTRL"))

mouse = Mouse()
mouse.key_press("ENTER")
```

## Accepted Key Formats

- Integer code: `65`, `4`
- Single key name: `"A"`, `"1"`
- Named key: `"SPACE"`, `"ENTER"`, `"ESC"`, `"LCTRL"`, `"RALT"`, `"F5"`
- Explicit prefix:
  - `"VK:65"` / `"VK_65"`
  - `"HID:4"` / `"HID_4"`
- Token format: `"KEY_A"`

## Backend Support

- `SendInput`: `key_down`, `key_up`, `key_press`, `is_key_pressed`
- `KmboxA`: keyboard send functions via module methods if available; `is_key_pressed` falls back to local Win32 key state
- `Net`: `key_down`, `key_up`, `key_press`, `is_key_pressed`, `mask_key`, `unmask_key`, `unmask_all_keys` (if exported by module)
- `DHZ`: `key_down`, `key_up`, `key_press`, `is_key_pressed`, `mask_key`, `unmask_key`, `unmask_all_keys`
- `Serial`, `MakV2`, `MakV2Binary`: `key_down`, `key_up`, `key_press` send ASCII keyboard commands; `is_key_pressed` currently returns `False`
- `Arduino`: keyboard functions are currently not implemented in the firmware bridge

## Notes

- `is_key_pressed` behavior depends on backend protocol support.
- For cross-backend compatibility, prefer named keys (`"A"`, `"SPACE"`, `"ENTER"`) or explicit prefixes (`VK:` / `HID:`).
