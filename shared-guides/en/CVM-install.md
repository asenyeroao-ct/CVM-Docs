# CVM colorBot Installation Guide

Shared guide index: [README.md](./README.md)

This guide will help you complete the full installation and setup process for CVM colorBot.

## Prerequisites

Before starting the CVM colorBot installation, ensure your system meets the following requirements:

- **Operating System**: Windows 10 or higher
- **Python**: Python 3.11 or higher (Python 3.11.9 recommended)
- **Internet Connection**: Required for downloading dependency packages (during first installation)

If you haven't installed Python yet, please refer to the [Python Installation Guide](./Python-install.md) to complete Python installation first.

## Step 1: Download the Project

1. Download or clone the CVM colorBot project from GitHub.
2. Extract the project to your chosen directory (e.g., `D:\CVM-colorBot-main`).
3. Verify the project directory contains the following files:
   - `main.py` (main program entry point)
   - `setup.bat` (automated installation script)
   - `run.bat` (application launcher script)
   - `requirements.txt` (dependency package list)

## Step 2: Run the Automated Installation Script

1. Locate the `setup.bat` file in the project root directory.
2. Double-click to run `setup.bat`, or execute it in Command Prompt:
   ```bash
   setup.bat
   ```

3. The installation script will automatically perform the following operations:
   - Check if Python is installed and in PATH
   - Create a virtual environment (`venv` folder)
   - Upgrade pip to the latest version
   - Install all necessary dependency packages

4. Wait for the installation to complete. The installation process may take several minutes, depending on your network speed.

## Step 3: Verify Installation

After installation completes, verify the following items:

1. **Check Virtual Environment**:
   - Confirm the `venv` folder has been created in the project directory
   - Confirm the `venv\Scripts\` directory exists

2. **Check Dependency Packages** (optional):
   In Command Prompt, execute:
   ```bash
   venv\Scripts\activate
   pip list
   ```
   You should see the following main packages:
   - `customtkinter`
   - `numpy`
   - `opencv-python`
   - `pyserial`
   - `mss`
   - And other dependency packages

## Step 4: Launch the Application

After installation, you can launch CVM colorBot using the following methods:

### Method 1: Using the Launcher Script (Recommended)

1. Double-click the `run.bat` file in the project root directory.
2. The application will automatically launch using Python from the virtual environment.

### Method 2: Manual Launch

1. Open Command Prompt (CMD) or PowerShell.
2. Navigate to the project directory:
   ```bash
   cd D:\CVM-colorBot-main
   ```
3. Launch the application:
   ```bash
   venv\Scripts\python.exe main.py
   ```

## Step 5: Initial Configuration

When launching CVM colorBot for the first time, it's recommended to perform the following basic configuration:

1. **Select Capture Mode**:
   - In the `General` tab, select an appropriate capture mode
   - Available modes: `UDP`, `NDI`, `Capture Card`, `MSS`
   - For detailed setup, refer to the corresponding setup guides:
     - [OBS UDP Setup](./OBS-UDP-setup.md)
     - [MAKCU Setup](./MAKCU-setup.md)

2. **Configure Mouse API**:
   - In the `General` tab, select `Input API`
   - If using MAKCU device, select `Serial` (unless using `3.8 left (firewall)` firmware or `makxdV2` hardware, then select `MakV2Binary`)
   - Configure related connection parameters (e.g., COM port, baud rate, etc.)
   - For detailed setup, refer to [MAKCU Setup](./MAKCU-setup.md)

3. **Adjust Basic Settings**:
   - Note: FOV size is adjusted in OBS (refer to [OBS UDP Setup](./OBS-UDP-setup.md) and [FOV Size Reference](./FOV-size.md)), not in CVM
   - Configure aimbot mode and parameters in CVM
   - Enable or disable features as needed

## Troubleshooting

### Issue 1: Python Not Found

**Error Message**: `[ERROR] Python is not installed or not in PATH`

**Solution**:
1. Verify Python is correctly installed (run `python --version` to check)
2. Verify Python is added to system PATH
3. Refer to the [Python Installation Guide](./Python-install.md) to reinstall Python

### Issue 2: Virtual Environment Creation Failed

**Error Message**: `[ERROR] Failed to create virtual environment`

**Solution**:
1. Verify you have sufficient disk space
2. Verify the project directory has write permissions
3. Try running `setup.bat` as Administrator
4. Check if Python installation is complete

### Issue 3: Dependency Package Installation Failed

**Error Message**: `[ERROR] Failed to install dependencies`

**Solution**:
1. Check if network connection is working
2. Verify pip is updated to the latest version:
   ```bash
   python -m pip install --upgrade pip
   ```
3. Try manually installing dependency packages:
   ```bash
   venv\Scripts\activate
   pip install -r requirements.txt
   ```
4. If a specific package installation fails, try installing it individually:
   ```bash
   pip install <package-name>
   ```

### Issue 4: Application Won't Start

**Error Message**: `[ERROR] Program exited with error`

**Solution**:
1. Verify the virtual environment was created correctly
2. Verify all dependency packages are installed correctly
3. Check if `config.json` file exists and has correct format
4. Review detailed error messages in console output
5. Verify system meets all prerequisites

### Issue 5: Module Import Error

**Error Message**: `ModuleNotFoundError` or `ImportError`

**Solution**:
1. Verify you're using Python from the virtual environment:
   ```bash
   venv\Scripts\python.exe main.py
   ```
2. Reinstall missing packages:
   ```bash
   venv\Scripts\activate
   pip install <missing-package-name>
   ```
3. If the problem persists, try running `setup.bat` again

## Next Steps

After installation, it's recommended to:

1. **Read Related Setup Guides**:
   - [OBS UDP Setup](./OBS-UDP-setup.md) - Configure UDP capture mode
   - [MAKCU Setup](./MAKCU-setup.md) - Configure MAKCU hardware device
   - [Kmbox Net Connection Guide](./Kmbox-Net-setup.md) - Configure Kmbox Net device
   - [FOV Size Reference](./FOV-size.md) - Understand FOV size settings

2. **Familiarize Yourself with the Application Interface**:
   - Explore features in each tab
   - Understand the purpose of each configuration option
   - Test basic functions to ensure they work properly

3. **Optimize Settings**:
   - Adjust parameters based on your hardware and needs
   - Save configuration files for future use
   - Test different modes to find the most suitable settings

## Important Notes

- **Windows Only**: CVM colorBot is designed for Windows and does not support other operating systems
- **Regular Updates**: It's recommended to regularly check and update the project and dependency packages
- **Backup Configuration**: Regularly backup your `config.json` and configuration files
- **System Permissions**: Some features may require Administrator privileges (e.g., serial port access)

## Need Help?

If you encounter issues during installation:

1. Check the "Troubleshooting" section in this document
2. Review the project's GitHub Issues
3. Verify your system and Python version meet requirements
4. Provide detailed error messages for problem diagnosis
