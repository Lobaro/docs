# 1-Wire LoRaWAN Bridge (Temperature Sensors)

## Firmware

**Downloads:**

* [app-lorawan-onewire-bridge-0.3.0.hex](firmware/app-lorawan-onewire-bridge-0.3.0.hex)

## Changelog

### v0.3.0 (24.07.2019)
--------------------
* Temp sensors will be printed and sent sorted by ID

### v0.2.0 (15.02.2019)
* Add parameter "SendSensorId" to allow skipping sensor IDs in payload

### v0.0.3
* Bugfixes in LoraWAN stack
* LoRaWAN Support for RX1 DataRate Offset

### v0.0.2
* Don't send sensor type with ID (support 6 sensors in 50 bytes)
* Add TestMode config value
* Add SendInternalTemp config value
* Internal sensor is send first if SendInternalTemp = true