# Lobaro Maintenance Tool

![Screenshot Lobaro-Tool](./img/Lobaro_Tool_ConfigFeature.png)

!!! info "Downloads"
    * [**Lobaro Maintenance Tool Download (v1.5.1 Windows)**](https://github.com/lobaro/flash-tool-release/releases/download/1.5.1/lobaro-tool.exe){: target="_blank"}
    * [**Lobaro Maintenance Tool Download (v1.5.1 Linux)**](https://github.com/lobaro/flash-tool-release/releases/download/1.5.1/lobaro-tool-linux){: target="_blank"}
    * [**Lobaro Maintenance Tool Download (v1.5.1 Mac 64Bit)**](https://github.com/lobaro/flash-tool-release/releases/download/1.5.1/lobaro-tool-mac64){: target="_blank"}
    * [**CP2102 Driver Download**](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers){: target="_blank"}  

Supports the PC based configuration of all Lobaro IoT sensors. 

It is intended to be used in conjunction with our [**USB configuration adapter**](./usb-config-adapter.md). 

## Features

* Change static sensor configuration
* Perform firmware updates for your Lobaro devices
* Live monitoring of device diagnostic output
* Save diagnostric output into *.txt file

## System Requirements

* Operating system:
    * MacOS X
    * Windows 7/10
    * Linux
    
* Browser
    * Firefox
    * Chrome
    * Edge
       
!!! warning "USB Driver"

    The CP2102 USB driver **MUST** to be installed before using the Lobaro-tool.
    
    [**CP2102 Driver Download**](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers){: target="_blank"}       
    
## Download & Installation

### macOS / Linux 
After downloading the "lobaro-tool" file, e.g. to a directory "lobaro" in your home path. Then make the tool file executable:

```Bash
cd ~/lobaro
chmod +x lobaro-tool
./lobaro-tool
```

!!! info
    If MacOS shows up a security warning and refuses to start the tool: 
    You can solve this by right-clicking the lobaro-tool file, selecting open and overrule the warning.

### Windows
After downloading simply start the "lobaro-tool.exe" with double click. 

Alternativly download the windows installer and start this.

!!! note
    Windows might show up a security warning and ask you to proceed anyway. This is behavior is normal.


## Changelog

### 1.5.1 - 31.01.2020
- Support nRF9160 Config and Firmware update on Lobaro hardware
- Improve progress indicator and error checks during Flash

### 1.4.10 - 11.10.2019
- No relevant changes

### 1.4.9 - 17.09.2019
- Fix a bug where flashing firmware fails due to UART buffer issues

### 1.4.4 - 11.09.2019
- Fix a bug that failed to flash very big firmware files.
- Add delays when communicating with Bootloader to avoid timing issues.
- Flash command now support --verbose flag

### 1.4.3 - 30.07.2019
- Fix a bug where writing the config does not work.

### 1.4.2 - 29.07.2019
- Fix a bug where the program hangs up while connecting to a wrong serial port in "auto" mode.
- Fix a bug where the tool crashes when flashing the firmware while not connected.

### 1.4.0 - 10.08.2018

- Log Tool events like Connect, Disconnect, Read Config, etc. to UART Log
- Switch to firmware mode after connecting with "auto" port

### 1.3.4 - 09.08.2018
- Always switch to Firmware run mode after: Connect, Load Config, Restore Config.

### 1.3.3
- Internet Explorer support

### 1.3.2 - 09.07.2018
- Fix reading configs bigger than 256 Bytes (needed for wMbus Bridge)

### 1.3.0 - 21.03.2018
- Improve connection detection
- Allow to connect to specific serial port
- Do not reset to boot mode when connecting to selected serial port

### 1.2.5 - 07.03.2018
- Fix issues with loading a configuration file. Default is now "config.yaml"
- Remove horizontal scrollbars in tabs
- Add "Set Time" button to send "time=<now>" via UART
- Add send UART input below Log
- Display if Firmware or Booloader is running
- Add success message when loading config

### 1.2.4 - 07.03.2018

- Allow to replace assets e.g. the logo by placing /assets/logo.png next to the executable

### 1.2.3 - 05.03.2018
- Log Timestamp in UART log file on disk
- Allow to set http server ip and port to allow remote access

### 1.2.2 - 19.02.2018
- Fix CBOR decoding error that was introduced in 1.2.0 (see: https://github.com/ugorji/go/issues/232)

### 1.2.0 - 16.02.2018
- Fix Serial port issues that appear on MAC, Linux and in rare cases on Windows

### 1.2.0 - 15.02.2018
- Log UART output to file in $HOME/.lobaro/

### 1.1.x
- Restore Default Config
- Close button
- Closing the browser window now also shut down the server

