# 1-Wire LoRaWAN Bridge
`Order number: 8000072 1m Cable / 8000120 10m Cable` <br>
![1-wire-lorawan](files/Multi-Tempsensor-LoRaWAN-DS18B20-open-2.jpg){: style="height:350px;"}

!!! info "Consider using the latest firmware on your hardware"
    * [**See available firmware downloads**](firmware.md){: target="_blank"}

## Target Measurement / Purpose

Supports up to 32 [DS18x20 1-Wire](https://www.maximintegrated.com/en/products/sensors/DS18B20.html){: target="_blank"} temperature sensors. The temperature form all sensors in read regualarly and send via LoRaWAN uplink.
When the payload gets too big for a single LoRaWAN message, it is split into multiple uplinks.


## Configuration
Without configuration the sensors will be transmitted ordered by the 48 Bit id, ignoring the Sensorfamily prefix and the Checksum.
 
| name | description | example value |
|------|-------------|----------------|
| `SendInternalTemp` | Toggle output of internal sensor. Will alway be sent first.  | `true` or `false` |
| `SendSensorId`     | Include Sensor ID in upload. Changes Payload format and Port.      | `true` or `false` |
| `SensorIdOrder`    | Semicolon separated list of 48 Bit IDs in hex (up to 25) | `22ffffff0000;44ffffff0000;11ffffff0000` |
| `TestMode`         | Run device in Testing Mode to help identify sensors. Must be `false` for normal operations. | `true` or `false` |

## Temperature and Error values
Temperature is transmitted in 10th of degrees Centigrade (d°C), to avoid having to deal with floating point numbers.
Error conditions for individual sensors are transmitted as temperatures below -300°C.

| temperature | hex value | error condition |
|-------------|-----------|-----------------|
| -899.0°C    | `0xdce2`  | Sensor not found (for sensors set in `SensorIdOrder`). |
| -997.0°C    | `0xd90e`  | Communication timed out. |
| -998.0°C    | `0xd904`  | Temperature read out error. |
| -999.0°C    | `0xd8fa`  | No temperature value. |

## Sensor IDs
A complete sensor ID consists of 8 bytes:

```
byte    0: family code
bytes 1-6: serial number
byte    7: crc cecksum 
```

Only the serial number is used by the device. Family code and checksum are omitted for uploads and in configuration.

## Internal Sensor
There is a temperature sensor included in the board of the device. If `SendInternalTemp` is set to `true`, that sensor 
will be included in uploads. It will always be the first sensor in the list.

## Sensor order 
The sensors will be ordered by their IDs (without the leading byte indicating the sensor type). You can fix the 
upload order by listing sensors in `SensorIdOrder`. Sensors included here will always be included in upload messages, 
whether the sensor is found or not. If the sensor cannot be found, a temperature value of -899.0°C (=`0xdce2`) is sent.

If the internal sensor is included (by `SendInternalTemp`=`true`), the internal sensor will still be sent first, 
before the first sensor listed in `SensorIdOrder`.

Any sensors that are found and are neither internal nor listed in `SensorIdOrder` will be appended after the last 
sensor from `SensorIdOrder`, ordered by their IDs.

## Payload

Example payloads for each port:

### Status Message (Port 1)

Payload: `00040001070ce3`

Decoded:
```json
{
  "temp": 26.3,
  "vBat": 3.299,
  "version": "v0.4.0"
}
```

### Data Message (Port 2)
Payload with temperature measurements when Sensor-ID is included.

Structure:

| name        | len | type       | description |
| ----------- | :-- | ---------- | ----------- |
| success     |   1 | `uint8`    | 0 = Read error, 1 = Success |
| sensor id   |   6 | `uint8[6]` | 6-Byte 1-Wire Sensor Id |
| temperature |   2 | `int16 BE` | Temperature in 1/10 °C |
| ...         |     |            | ... sensor id and temperature fields repeat ...  |

When the total length of data exceeds 50 bytes, the message will be split and uploaded 
in multiple packets using this format.

Example Payload:
`01551e46920d0200da96b446920c0200d7dafc46920d0200d5202e4692050200dc`

Decoded:
```json
{
  "sensors": [
    {
      "id": "551e46920d02",
      "temp": 21.8
    },
    {
      "id": "96b446920c02",
      "temp": 21.5
    },
    {
      "id": "dafc46920d02",
      "temp": 21.3
    },
    {
      "id": "202e46920502",
      "temp": 22
    }
  ],
  "success": true
}
```

### Data Message (Port 3)
Payload with temperature measurements when Sensor-ID is omitted.

Before v0.4.0 this format is sent on Port 2.

Structure:

| name        | len | type       | description |
| ----------- | :-- | ---------- | ----------- |
| success     |   1 | `uint8`    | 0 = Read error, 1 = Success |
| temperature |   2 | `int16 BE` | Temperature in 1/10 °C |
| ...         |     |            | ... temperature field repeats ...  |

When the total length of data exceeds 50 bytes, the message will be split and uploaded 
in multiple packets using this format.

Example Payload:
`0100f500f500f800f500f300f8`

Decoded:
```json
{
    "sensors": [
      24.5,
      24.5,
      24.8,
      24.5,
      24.3,
      24.8
    ],
    "success": true}
```

## Parser

### TheThingsNetwork (TTN) / ChirpStack
```javascript
function readVersion(bytes) {
    if (bytes.length<3) {
        return null;
    }
    return "v" + bytes[0] + "." + bytes[1] + "." + bytes[2];
}
function parse_sint16(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    var t = bytes[0] << 8 | bytes[1] << 0;
    if( (t & 1<<15) > 0){ // temp is negative (16bit 2's complement)
        t = ((~t)& 0xffff)+1; // invert 16bits & add 1 => now positive value
        t=t*-1;
    }
    return t;
}
function parse_uint16(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    var t = bytes[0] << 8 | bytes[1] << 0;
    return t;
}

function parse_hex(bytes, idx, end) {
    var chars = "0123456789abcdef";
    bytes = bytes.slice(idx || 0, end || null);
    var s = "";
    for (var i=0; i<bytes.length; i++) {
        var byte = bytes[i];
        s += chars.charAt(byte>>4);
        s += chars.charAt(byte & 0xf);
    }
    return s;
}

function DecoderPort1(bytes) {
    return {
        "version":readVersion(bytes),
        "temp": parse_sint16(bytes, 3) / 10,
        "vBat": parse_uint16(bytes, 5) / 1000,
    };
}

function DecoderPort2(bytes) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var sensors = [];
    var success = false;

    var pos = 0;
    if ( bytes.length ) {
        pos+=1;
        success = bytes[0] !== 0;
    }
    var left = bytes.length - pos;
    while (left>=8) {
        var sensor = {
            //'id_': bytes.slice(pos, pos+6),
            'id': parse_hex(bytes, pos, pos+6),
            'temp': parse_sint16(bytes, pos+6) / 10.0
        };
        sensors.push(sensor);
        pos += 8;
        left = bytes.length - pos;
    }
    // if (port === 1) decoded.led = bytes[0];

    var decoded = {};
    decoded['success'] = success;
    decoded['sensors'] = sensors;
    return decoded;
}

function DecoderPort3(bytes) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var sensors = [];
    var success = false;

    var pos = 0;
    if ( bytes.length ) {
        pos+=1;
        success = bytes[0] !== 0;
    }
    var left = bytes.length - pos;
    var nr=0;
    while (left>=2) {
      var temp = parse_sint16(bytes, pos) / 10.0
        sensors.push(temp);
        pos += 2;
        left = bytes.length - pos;
    }
    // if (port === 1) decoded.led = bytes[0];

    var decoded = {};
    decoded['success'] = success;
    decoded['sensors'] = sensors;
    return decoded;
}

function Decoder(bytes, port) {
    switch (port) {
        case 1:
            return DecoderPort1(bytes);
        case 2:
            return DecoderPort2(bytes);
        case 3:
            return DecoderPort3(bytes);
    }
    return {
        "error": "Invalid port",
        "port": port
    };
}

// Wrapper for Lobaro Platform
function Parse(input) {
    // Decode an incoming message to an object of fields.
    var b = bytes(atob(input.data));
    var decoded = Decoder(b, input.fPort);

    return decoded;
}

// Wrapper for Loraserver / ChirpStack
function Decode(fPort, bytes) {
    return Decoder(bytes, fPort);
}

// Wrapper for Digimondo niota platform
/*
module.exports = function (payload, meta) {
    const port = meta.lora.fport;
    const buf = Buffer.from(payload, 'hex');

    return Decoder(buf, port);
}
*/
```
## CE Declaration of Conformity

[CE Declaration of Conformity](files/ce-OneWire-lorawan.pdf) (pdf).

