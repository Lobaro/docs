# Modbus LoRaWAN Bridge

## Firmware

**Downloads:**

* [app-modbus-lora-bridge-1.0.0.hex](firmware/app-modbus-lora-bridge-1.0.0.hex).
* [app-modbus-lora-bridge-0.3.1.hex](firmware/app-modbus-lora-bridge-0.3.1.hex).

## Changelog

### [1.0.0]
**Added**

- LoRaWAN 1.1 support
- Remote configuration via LoRaWAN on port 128.
- Clock synchronisation via LoRaWAN.

**Changed**

- Random delay before Uplink (to prevent persistent collisions when using multiple devices).
- Modbus responses longer than payload now get split up (additional parts on port 5).

### [0.4.1]
**Fixed**

- Changed error indication bit on error 11 from `0xf0` to `0x80`.
- Fixed issue when parsing multiple Modbus commands from config.

### [0.4.0]
**Added**

- Writing values to holding registers and coils.
- Execution of arbitrary Modbus commands triggered by LoRaWAN Downlink messages.
- Support for LoRaWAN Operation Mode Class C (for short reaction time to Downlinks).
- Automated register writing and broadcasts possible through new configuration.

**Changed**

- Automated reading (triggered by cron) is now configured by entering actual Modbus commands (more flexibility and usage of already existing Modbus syntax &ndash; *this breaks old configurations*).
- Upload format changed to sending raw response to Modbus commands (*this breaks existing integrations*).

**Fixed**

- Flushing to avoid invalid byte received from switching from TX to RX.
- Modbus mode ASCII now counts received bytes correctly.
- DataLength of 7 bits can now correctly be set in config again.

### [0.3.1] &ndash; 2019-05-24
**Fixed**

- Increased robustness of data reception on higher Baud rates.
  
### [0.3.0] &ndash; 2019-05-15
**Added**

- Initial release of Firmware for new Hardware revision (with RS485-addon).
- Update Modbus to support all 4 types of registers.

**Changed**

- Parity bit must not be substracted from Data bits anymore. `8E1` can now be confiured with `8 Data bits, EVEN parity, 1 Stop bit`.


### [0.1.0] &ndash; 2018-08-13
**Added**

- Original hardware release (with RS-485 on holding PCB).
