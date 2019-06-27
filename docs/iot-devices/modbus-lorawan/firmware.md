# Modbus LoRaWAN Bridge

## Firmware

**Downloads:**

* [app-modbus-lora-bridge-0.3.1.hex](firmware/app-modbus-lora-bridge-0.3.1.hex).

## Changelog

### [Unreleased]
**Added**

* Writing values to holding registers and coils.

**Fixed**

* Flushing to avoid invalid byte received from switching from TX to RX.
* Modbus mode ASCII now counts received bytes correctly.
* DataLength of 7 bits can now correctly be set in config again.

### [0.3.1] &ndash; 2019-05-24
**Fixed**

* Increased robustness of data reception on higher Baud rates.
  
### [0.3.0] &ndash; 2019-05-15
**Added**

* Initial release of Firmware for new Hardware revision (with RS485-addon).
* Update Modbus to support all 4 types of registers.
* Parity bit must not be substracted from Data bits anymore. `8E1` can now be confiured with `8 Data bits, EVEN parity, 1 Stop bit`.

