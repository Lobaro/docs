# Modbus LoRaWAN Bridge
The Lobaro Modbus LoRaWAN Bridge is a low power device that can be used to read values
out of a variety of other devices via Modbus (ASCII/RTU) over a RS-485 interface
and forward them over LoRaWAN, so that they can be accessed from an attached system.
Typical applications include reading out electric and water meters or retrieving
data from environmental sensors like temperature and humidity.

## Configuration

#### LoRaWAN


#### Modbus/UART
There are several values that define the configuration via Modbus. These 
values depend on the Slave devices that you want to read out. Please refer to your 
Modbus Devices's manual to find out the correct configuration.

| name | description | values |
|------|-------------|--------|
| `ModbusProtocol`   | Which Modbus-Protocol to use | `RTU`. `ASCII` |
| `ModbusBaud`       | UART Baud rate | `9600`, `19200`, `38400`, ... |
| `ModbusDataLendth` | UART data length | `7`, `8`, `9` |
| `ModbusStopBits`   | UART stop bits | `0.5`, `1`, `1.5`, `2` (written exactly like this) |
| `ModbusParity`     | UART parity | `NONE`, `EVEN`, `ODD` |

#### Operation
| name | description | example value |
|------|-------------|----------------|
| `ModbusCron` | Cron expression defining when to read | `0 0/15 * * * *` for every 15 minutes |

#### Register/Coil definition
Modbus defines four different object types form which values can be read:
Coils, Discrete Inputs, Input Registers, and Holding Registers.
For an introduction please refer to 
[https://en.wikipedia.org/wiki/Modbus](https://en.wikipedia.org/wiki/Modbus).
There are four configuration values to define which values should be read by 
the Modbus Bridge, one for each of the types. 

| name | description | Modbus Function |
|------|-------------|----------------------| 
| `Coils` | Coils to read (single bit read-only values) | `0x01` |
| `DiscreteInputs` | Discrete Inputs to read (single bit read/write values) | `0x02` |
| `HoldingRegisters` | Holding Registers to read (2 byte read-only values) | `0x03` |
| `InputRegisters` | Input Register to read (2 byte read/write values) | `0x04` |

Each value can define multiple different registers/coils to be read on one or multiple 
devices connected via Modbus. The format is identical for all four types.


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
