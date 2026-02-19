# OBS UDP Setup

Shared guide index: [lang.md](../lang.md)

Follow these steps to stream your cropped FOV from the main PC to the secondary PC via UDP.

## Step 1: Configure FFmpeg URL

Path: `Settings -> Output -> Recording`

![OBS Output Mode](../lib/obs/OBS_OutputMode.png)

Set:

1. `Output Mode` to `Advanced`.
2. `Type` to `Custom Output (FFmpeg)`.
3. `FFmpeg Output Type` to `Output to URL`.
4. `File path or URL` to:

```text
udp://<sec PC IP>:<port>
```

Example:

```text
udp://192.168.0.1:1234
```

Notes:

- `<sec PC IP>` is the IPv4 address of your secondary PC.
- `<port>` is any unused port you choose, and a 4-digit port is recommended (for example `1234`, `2345`).

## Step 2: Container and Encoding Settings

Use:

1. `Container Format`: `mjpeg (Video)`
2. `Container Format Description`: `MIME multipart JPEG`
3. `Keyframe interval (frames)`: `0`

Other fields can stay at defaults initially.

![OBS FFmpeg Settings](../lib/obs/OBS_FFmpegSettings.png)

## Step 3: Set Resolution and FPS (FOV size)

Path: `Settings -> Video`

![OBS Resolution Settings](../lib/obs/OBS_Resolution.png)

Set:

1. `Base (Canvas) Resolution` to your FOV-size resolution.
2. `Output (Scaled) Resolution` to the same FOV-size resolution.
3. `FPS` higher than `144`.

Recommended FPS values: `160` / `165` / `180` / `240` (based on GPU performance).

## Step 4: Apply Crop/Pad Filter

To send only the FOV region:

1. Select your `Game Capture` or `Display Capture` source in OBS.
2. Click `Filters`.
3. Add `Crop/Pad`.
4. Fill crop values (Left/Top/Right/Bottom) according to your FOV-size table.

This prevents oversized frames and reduces UDP drop risk.

![OBS Crop Filter](../lib/obs/Crop.png)

## Step 5: Start Recording (Start UDP stream)

When everything is set correctly, click `Start Recording`.

OBS will begin sending the UDP stream from the main PC to the secondary PC.

![OBS Start Recording](../lib/obs/StartRecording.png)

## Secondary PC Setup

Use the exact same IP and port values used in OBS.

Example:

1. OBS output URL:

```text
udp://192.168.0.1:1234
```

2. Secondary PC input:
- `IP`: `192.168.0.1`
- `Port`: `1234`

If any value does not match exactly, the stream will not be received.

## How to Find Secondary PC IP

1. Press `Win + R`, type `cmd`, and press Enter.
2. Run:

```bash
ipconfig
```

3. Find `IPv4 Address` under your active adapter.

## Why `frame queue is empty` appears

Common causes:

1. Main PC and secondary PC are not connected on network.
2. OBS recording was not started.
3. IP or port is incorrect in UI.
4. OBS output/filter/encoder settings are wrong.

Check connectivity both directions:

```bash
ping <secondary PC IP>
ping <main PC IP>
```

## Extra Note for UDP Users

UDP still requires `Crop/Pad`. Without cropping, frame size can be too large for stable transport, causing empty queues or dropped frames.

## Gray screen with frequent error: `Corrupt JPEG data: premature end of data segment`

Cause: FPS is too high and decoder cannot keep up.

Fix:

1. Lower OBS FPS.
2. Prioritize stable decode first, then increase FPS gradually.

## FOV Size Reference

See: [FOV-size.md](./FOV-size.md)


