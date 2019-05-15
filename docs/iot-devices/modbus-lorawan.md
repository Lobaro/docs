# Modbus LoRaWAN Bridge
The Lobaro Modbus LoRaWAN Bridge is a low power device that can be used to read values
out of a variety of other devices via Modbus (ASCII/RTU) over a RS-485 interface
and forward them over LoRaWAN, so that they can be accessed from an attached system.
Typical applications include reading out electric and water meters or retrieving
data from environmental sensors like temperature and humidity.

## Configuration

#### LoRaWAN

#### UART


## Payload formats
The Modbus Bridge sends different kinds of messages over different LoRaWAN ports:

* Port 1: Status messages
* Port 11: Data from reading Coils..
* Port 12: Data from reading Discrete Inputs.
* Port 13: Data from reading Holding Registers.
* Port 14: Data from reading Input Registers.

#### Status messages

The Modbus Bridge sends a status messages report on the health of the device itself.
This messages are sent along when the device is sending data packages with a 
maximum of one status message per day. 

Status messages are transmitted on port 1 and have a fixed length of 14 bytes.

| name | pos | len | type | description | example |
|------|--:|--:|------|-------------|---------|
|version| 0 | 3 | `uint8[3]`| Version of firmware running on the device |`[1, 0, 4]` &equiv; `v1.0.4` |
|flag| 3 | 1 | `uint8` | Status flag, for internal use | `0` |  
|temperature| 4 | 2 | `int16` | Device's internal temperature in tenth °C | `246` &equiv; `24.6°C` |  
|voltage| 6 | 2 | `int16` | Voltage supplied by power source in mV | `3547` &equiv; `3.547V` |
|timestamp| 8 | 5 | `int40` | Internal date/time at creation of the status packet as UNIX timestamp | `1533055905` |
|mode | 13 | 1 | `uint8` | Operation mode the device runs | `1` |

#### Data messages
