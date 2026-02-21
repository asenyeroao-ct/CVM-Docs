# CVM colorBot Video Transport and Processing Flow (Dual-PC)

Back to index: [CVM.md](./CVM.md)

This page covers end-to-end flow only: how video enters CVM, how CVM processes frames, and how Input API output is triggered.

## Quick Overview

`Main PC video -> transport path (OBS-UDP / OBS-NDI / Capture Card) -> CVM frame read -> target detection -> dx/dy compute -> Input API output`

## 1. Typical Dual-PC Paths (Separated)

### 1.1 OBS-UDP Path

Best for:
- Fast low-cost dual-PC setup.
- Environments where network quality is acceptable.

End-to-end flow:
1. Main PC captures game video in OBS (`Game Capture` / `Display Capture`).
2. OBS applies `Crop/Pad` to keep only FOV region.
3. OBS sends UDP stream via FFmpeg URL:
   `udp://<secondary PC IP>:<port>`
4. Secondary PC selects `Method = UDP` in CVM.
5. CVM uses the same `UDP IP/Port` values.
6. `CaptureService` continuously receives and decodes frames.
7. Frames enter the tracker pipeline.

Key settings:
- OBS URL and CVM `udp_ip` / `udp_port` must match exactly.
- Keep crop enabled to avoid oversized frames and decode pressure.
- Start with stable `target_fps`, then increase gradually.

### 1.2 OBS-NDI Path

Best for:
- Legacy compatibility only.
- Not recommended for new deployments.

End-to-end flow:
1. Main PC enables OBS NDI output (Program/Preview as NDI source).
2. Main and secondary PCs stay on the same LAN.
3. Secondary PC selects `Method = NDI` in CVM.
4. Refresh source list and confirm visible NDI sources.
5. Select source and connect.
6. `CaptureService` continuously receives NDI frames for tracker pipeline.

Key settings:
- Verify target NDI source name is visible on secondary PC.
- Re-check connection after source/scene switching.
- If center crop is enabled, tune `ndi_fov_enabled` / `ndi_fov` for target FOV.

Quick comparison with UDP:
- NDI: usually better for source-management convenience and stream consistency.
- UDP: usually better for lower-latency and speed.
- Conclusion: NDI is not recommended in this project; it is kept only as a legacy compatibility path.

NDI risk note (Valorant):
- There are community claims that NDI may already be detectable, but there is currently no detailed and rigorous testing confirming whether NDI is detected or leads to bans.
- Some users also report it can still run under normal aspect-ratio setups, but that is not a safety guarantee.
- Because this remains uncertain and risk is hard to quantify, NDI is still not recommended as a default path.

### 1.3 Capture Card Path

Best for:
- Hardware-direct ingest preference.
- Users prioritizing predictable physical video capture path.

End-to-end flow:
1. Main PC video output (usually HDMI) goes to capture card input.
2. Capture card connects to secondary PC via USB 3.0 / PCIe.
3. Secondary PC selects `Method = CaptureCard` in CVM.
4. Set device index/resolution/FPS and connect.
5. `CaptureService` continuously reads frames from capture driver.
6. Frames enter the same tracker pipeline as other methods.

Key settings:
- Ensure selected device index matches actual capture device.
- Start with stable resolution/FPS before pushing higher values.
- Keep capture color-conversion defaults first, then tune only if needed.

## 2. Unified Post-Receive CVM Processing

After frame ingress (UDP/NDI/CaptureCard), CVM processing is the same:

1. Frame read: `AimTracker` reads latest frame from active backend.
2. Detection: HSV detection builds target candidates.
3. Target select: choose active target (typically nearest to center).
4. Offset compute: calculate center offset `dx/dy`.
5. Mode handling: active mode (Normal/Silent/NCAF/WindMouse/Bezier) generates movement.
6. Queueing: movement commands are pushed into `move_queue`.

## 3. Input API Call Chain (Output Stage)

1. Move thread consumes commands from `move_queue`.
2. Calls unified interface: `Mouse.move()` / `Mouse.press()` / `Mouse.release()`.
3. `Mouse` routes calls to selected backend:
   `Serial` / `Arduino` / `SendInput` / `Net` / `MakV2` / `DHZ`, etc.
4. Backend emits real OS/hardware input.

In short:
`CVM computes dx/dy -> calls Mouse interface -> routes to Input API -> outputs real input`

## 4. Safety Line and Recommendation Order (Dual-PC baseline)

Bottom-line ranking (as requested):

1. `Capture Card`: safest  
   Reason: no extra streaming plugin/process is required on the gaming PC side; it only outputs video signal.
2. `OBS-UDP`: relatively safe, and preferred over NDI  
   Reason: UDP output is on the OBS native path, with fewer moving parts; in practice it is also usually faster.
3. `OBS-NDI`: relatively higher risk  
   Reason: NDI is not the core native OBS path and relies on additional ecosystem/components.
4. Position statement  
   In this project, NDI is a legacy holdover and remains only for backward compatibility.

Important baseline:
- As long as you are not running `1PC` (game + processing on same machine), you are generally within the safer dual-PC line.
- Between `OBS-UDP` and `OBS-NDI`, default recommendation is `OBS-UDP` for both relative safety and speed.
- Strong recommendation: for new environments, avoid NDI and use `Capture Card` or `OBS-UDP`.
- If bans happen as isolated cases rather than broad waves across similar users, it is more commonly tied to individual setup/behavior/match patterns and potential manual review, not a confirmed single-path-wide trigger.

Comparison:

| Path | Relative safety line | Typical speed feel | Conclusion |
|---|---|---|---|
| Capture Card | Highest | High and stable | First choice when safety is priority |
| OBS-UDP | Medium-high | Usually faster than NDI | Preferred network dual-PC path |
| OBS-NDI | Medium | Usually slower than UDP | Not recommended, legacy compatibility only |

## 5. Troubleshooting by Path (Priority Order)

### OBS-UDP

1. Check bidirectional `ping`.
2. Ensure OBS `Start Recording` is active.
3. Verify OBS URL and CVM `IP/Port` are identical.
4. Verify `Crop/Pad` is configured.
5. Lower FPS to recover stability.

### OBS-NDI

1. Confirm NDI source is published on main PC.
2. Refresh source list in CVM and verify source appears.
3. Reconnect selected source.
4. Verify LAN segment/firewall/isolation rules.

### Capture Card

1. Confirm capture card is detected by OS.
2. Verify correct device index in CVM.
3. Lower resolution/FPS to rule out bandwidth/driver limits.
4. Check cable/interface specification and power stability.

## 6. Related Guides

- OBS UDP setup: [OBS-UDP-setup.md](../../shared-guides/en/OBS-UDP-setup.md)
- FOV size reference: [FOV-size.md](../../shared-guides/en/FOV-size.md)
