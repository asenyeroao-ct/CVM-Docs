# GStreamer Installation Guide

Shared guide index: [lang.md](../lang.md)

## Overview

GStreamer is required for using the "Capture Card (GStreamer)" mode in CVM colorBot. This guide will help you install GStreamer on Windows.

![GStreamer Icon](../lib/GStreamer/GStreamer_icon128.png)

## Step 1: Download GStreamer Installer

1. Download the GStreamer MSVC installer for Windows x64:
   - **Direct Download**: [gstreamer-1.0-msvc-x86_64-1.28.0.exe](https://gstreamer.freedesktop.org/data/pkg/windows/1.28.0/msvc/gstreamer-1.0-msvc-x86_64-1.28.0.exe)
   - **Official Site**: https://gstreamer.freedesktop.org/download/#windows

2. The installer includes both runtime and development packages.

![Download Page](../lib/GStreamer/GStreamer1_install.png)

## Step 2: Run the Installer

1. Run the downloaded installer (`gstreamer-1.0-msvc-x86_64-1.28.0.exe`).
2. Click `Next` to start the installation wizard.

![Installer Welcome](../lib/GStreamer/GStreamer2_install.png)

## Step 3: Choose Installation Type

1. Select **"Complete"** installation (recommended) to install all components.
2. Click `Next` to continue.

![Installation Type](../lib/GStreamer/GStreamer3_install.png)

## Step 4: Select Installation Directory

1. The default installation path is `C:\gstreamer\1.0\msvc_x86_64\`.
2. You can change the installation directory if needed.
3. Click `Install` to begin installation.

![Installation Directory](../lib/GStreamer/GStreamer4_install.png)

## Step 5: Installation Progress

1. Wait for the installation to complete.
2. This may take a few minutes depending on your system.

![Installation Progress](../lib/GStreamer/GStreamer5_install.png)

## Step 6: Complete Installation

1. Once installation is complete, click `Finish`.
2. The installer will automatically add GStreamer to your system PATH.

![Installation Complete](../lib/GStreamer/GStreamer6_install.png)

## Step 7: Verify Installation

1. Open a new Command Prompt or PowerShell window.
2. Run the following command to verify GStreamer is installed:

```bash
gst-launch-1.0 --version
```

2. You should see output similar to:

```
gst-launch-1.0 version 1.28.0
GStreamer 1.28.0
```

![Verify Installation](../lib/GStreamer/GStreamer7_install.png)

## Step 8: Using GStreamer in CVM colorBot

1. After installation, restart CVM colorBot if it's running.
2. In the General tab, under Capture Controls, select **"Capture Card (GStreamer)"** from the Method dropdown.
3. Configure your capture card settings (Device Index, Resolution, FPS, etc.).
4. Click `CONNECT` to start using GStreamer capture.

![CVM colorBot Usage](../lib/GStreamer/GStreamer8_install.png)

## Troubleshooting

### GStreamer Not Detected

If CVM colorBot cannot detect GStreamer:

1. **Check PATH**: Ensure GStreamer is in your system PATH. The installer should add it automatically, but you may need to restart your computer.
2. **Manual PATH**: If needed, add `C:\gstreamer\1.0\msvc_x86_64\bin` to your system PATH manually.
3. **Restart Application**: Close and restart CVM colorBot after installation.

### Installation Issues

- If the installer fails, try running it as Administrator.
- Ensure you have sufficient disk space (GStreamer requires several hundred MB).
- Check Windows Event Viewer for detailed error messages.

### DLL Not Found Errors

- Ensure you downloaded the **MSVC** version (not MinGW) for best compatibility.
- Verify the installation path is correct.
- Try reinstalling if DLL errors persist.

## Additional Resources

- **GStreamer Official Documentation**: https://gstreamer.freedesktop.org/documentation/
- **GStreamer Windows Installation Guide**: https://gstreamer.freedesktop.org/documentation/installing/on-windows.html
