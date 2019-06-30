# Lobaro USB configuration adapter

![Picture USB Config Adapter](./img/lobaro-config-adapter.jpg)

The USB configuration adapter can be used for:

* Sensor initial configuration
* Firmware log diagnostics
* Firmware updates

using our free [Lobaro Maintenance Tool](./lobaro-tool.md) PC software.

!!! note
    The blue wire is not consistent the RTS pin and may be on some adapters inverted, e.g. beeing the GND wire. Check the 
    orientation of the adapter with the picture above to determinate the acutal pins and do not rely on the the wire color coding.

## Driver Download
The adapter uses a CP2102 USB to UART bridge internally. The driver is available for free download and must be installed prior first use:

[CP2102 Driver Download](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) (MacOS, Windows, Linux)

## Hardware Connection (LoRaWAN Sensors)

* ```Boot0``` of the Lobaro LoRa Hardware (STM32 based) is connected to ```DTR``` of the UART
* ```Reset``` (active low) of the Lobaro LoRa Hardware (STM32 based) is connected to ```RTS``` of the UART

Normally the handling of these uart control is done internally by the [Lobaro PC tool](lobaro-tool).

When using any other uart terminal make sure you control RTS and DTR of the UART correctly or cut the DTR/RTS wires 
from the USB adapter connection if not needed.

### Default UART Configuration

The default 8N1 UART configuration that is used by all Lobaro devices on the "Config" port:

|         |              |
|---------|--------------|
|BaudRate | 115200       |
|Parity   | No Parity    |
|StopBits | OneStopBit   |
|DataBits | 8            |

### DTR control line

* ```Low / true``` => Run Firmware after Reset (Default since BOOT0 has internal pull-down)
* ```High / false``` => Run Bootloader after Reset


### RTS control line

* ```High / false``` => Run Firmware / Bootloader (Default since RESET has internal pull-up)
* ```Low / true``` => Chip in RESET mode (not running)

## Adapter Schematic
[Picture USB Config Adapter](./img/config-adapter-schematic.png)

![Picture USB Config Adapter](./img/config-adapter-schematic.png)

