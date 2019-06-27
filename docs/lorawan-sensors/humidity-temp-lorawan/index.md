# Humidity & Temperature LoRaWAN Sensor

![Humidity & Temperature LoRaWAN Sensor](files/LoRaWAN-HumiditySensor.jpg){: style="height:300px;border:1px black solid"}

## Target Measurement / Purpose
Temperature and relative humidity measurements with external probe and LoRaWAN.

## PDF Documentation

* [User Manual (en)](files/lorawan-humidity-sensor_en.pdf){: target="_blank"}
* [CE Conformity](files/scan-ce-conformity-am2305-lorawan.pdf){: target="_blank"}


## Sensor Specification

![Sensor Specification Data](files/am2305_spezificationdata.png)


## Parser

### The Things Network (JavaScript)

```javascript
/**
 * TTN-compatible data decoder for the Lobaro LoRaWAN Humidity Sensor.
 */

function readVersion(bytes) {
    if (bytes.length<3) {
        return null;
    }
    return "v" + bytes[0] + "." + bytes[1] + "." + bytes[2];
}

function int40_BE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return bytes[0] << 32 |
        bytes[1] << 24 | bytes[2] << 16 | bytes[3] << 8 | bytes[4] << 0;
}

function signed(val, bits) {

    if ((val & 1 << (bits-1)) > 0) { // value is negative (16bit 2's complement)

        var mask = Math.pow(2, bits) - 1;

        val = (~val & mask) + 1; // invert all bits & add 1 => now positive value

        val = val * -1;

    }

    return val;

}

function int16_BE(bytes, idx) {

    bytes = bytes.slice(idx || 0);

    return signed(bytes[0] << 8 | bytes[1] << 0, 16);

}

function int16_BE_1c(bytes, idx) {
    bytes = bytes.slice(idx || 0);

    var v = (bytes[0]&0x7f) << 8 | bytes[1] << 0;
    if (bytes[0]&0x80) {
      return -v;
    } else {
      return v;
    }
}
 

function port1(bytes) {
    return {
        "port":1,
        "version":readVersion(bytes),
        "flags":bytes[3],
        "temp": int16_BE(bytes, 4) / 10,
        "vBat": int16_BE(bytes, 6) / 1000,
        "timestamp": int40_BE(bytes, 8),
        "operationMode": bytes[13]
    };
}

function port2(bytes) {
    return {
        "port":2,
        "timestamp": int40_BE(bytes, 0),
        "error":!!(bytes[1]&0x01),
        "humidity":int16_BE(bytes, 6)/10.0,
        "temperature":int16_BE_1c(bytes, 8)/10.0
    };
}

function Decoder(bytes, port) {
    switch (port) {
        case 1:
            return port1(bytes);
        case 2:
            return port2(bytes);
    }
    return {"error":"invalid port", "port":port};
}

```