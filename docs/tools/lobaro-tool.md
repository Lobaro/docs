# Lobaro Maintenance Tool

![Screenshot Lobaro-Tool](./img/Lobaro_Tool_ConfigFeature.png)

!!! info "Downloads"
    [**Lobaro Maintenance Tool Download**](https://files.lobaro.com/index.php/s/jJULuRooWzLnYO9?path=%2Fv1.4.x){: target="_blank"}
    
    [**CP2102 Driver Download**](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers){: target="_blank"}  

Supports the PC based configuration of all Lobaro IoT sensors. 

It is intend to be used in conjunction with our [**USB configuration adapter**](./usb-config-adapter.md). 

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
### Windows
After downloading simply start the "lobaro-tool.exe" with double click. 

Alternativly download the windows installer and start this.

!!! note
    Windows might show up a security warning and ask you to proceed anyway. This is behavior is normal.





