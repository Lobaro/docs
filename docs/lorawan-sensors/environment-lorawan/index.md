# Environment LoRaWAN Sensor

!!! info "Consider using the latest firmware on your hardware"
    * [**See available firmware downloads**](firmware.md){: target="_blank"}

## Target Measurement / Purpose

High precision measuring of Temperature, Humidity, and Pressure, using the Bosch BME680 environmental sensor.


## Configuration
For configuration of the LoRaWAN parameters, please 
refer to the chapter [LoRaWAN configuration](/background/lorawan.html#lorawan-configuration) 
in our LoRaWAN background article.
 
| name | description | example value |
|------|-------------|----------------|
| `MeasureCron` | Cron expression<sup>&dagger;</sup> defining when to read. | `0 0/15 * * * *` for every 15 minutes |

<sup>&dagger;</sup> See also our [Introduction to Cron expressions](/background/cron-expressions.html).



## Payload

Example payloads for each port:

### Status Message (Port 1)

Payload: (No Example yet)

Decoded:
```json
{
  "temp": 20.4,
  "vBat": 3.0,
  "version": "v0.0.1"
}
```

### Data Message (Port 2)

Structure:

| name        | pos | len | type       | description |
| ----------- | :-- | :-- | ---------- | ----------- |
| timestamp   |   0 |   5 | `uint40`   | Time of measurement (see [timestamps in our LoRaWAN devices](/background/lorawan.html#timestamp)) |
| error flag  |   5 |   1 | `byte`     | `1`: error, `0`: success |
| humidity    |   6 |   2 | `uint16 BE`| Humidity in 1/10 % rH |
| temperature |   8 |   2 | `int16 BE` | Temperature in 1/10 °C |
| pressure    |  10 |   2 | `uint16 BE`| Pressure in 10 Pa |

Example Payload:
`00386d440700015400f527d1`

Decoded:
```json
{
  "error": false,
  "humidity": 34,
  "pressure": 1019.3,
  "temperature": 24.5,
  "time": 946684935000
}
```

## Parser

### TheThingsNetwork (TTN)
```javascript
// TODO
```
