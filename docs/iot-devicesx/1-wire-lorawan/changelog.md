# Changelog

**Firmware:** app-lorawan-onewire-bridge


v0.2.0 (15.02.2019)
--------------------
* Add parameter "SendSensorId" to allow skipping sensor IDs in payload

v0.0.3
--------
* Bugfixes in LoraWAN stack
* LoRaWAN Support for RX1 DataRate Offset

v0.0.2
--------
* Don't send sensor type with ID (support 6 sensors in 50 bytes)
* Add TestMode config value
* Add SendInternalTemp config value
* Internal sensor is send first if SendInternalTemp = true