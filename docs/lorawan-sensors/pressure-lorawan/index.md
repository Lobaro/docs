# Pressure Sensor LoRaWAN

![LoRaWAN Pressure Sensor](files/Pegelsonde1.png){: style="height:500px;display: block; margin: 0 auto;"}

!!! info "Consider using the latest firmware on your hardware"
    * [**See available firmware downloads**](firmware.md){: target="_blank"}

## Target Measurement / Purpose
Precise liquid level measurement, e.g. for tanks, via LoRaWAN.

**Features**

* Cable length 1-15m
* 0..1,5 Bar (15m water level)
* Waterproof IP66 Housing
* Multi-year Battery life, ultra low power (< 10µA)

## Payload Format

Port: 1
Payload: 8 Bytes

Temperature is transmitted in 1/100&deg;C, battery voltage in Millivolt and pressure in Bar.

| PRESSURE | PRESSURE | PRESSURE | PRESSURE | TEMP | TEMP | V_BATT | V_BATT |
|------|------|------|------|-----|------|------|------|
|flaot32|float32|float32|float32|int16|int16|int16|int16|
|Byte 0|Byte 1|Byte 2|Byte 3|LSB|MSB|LSB|MSB|    

![Payload Format](files/payload-format.png)

### Parser: The Things Network

```javascript
function decodeFloat32(bytes) {
    var sign = (bytes & 0x80000000) ? -1 : 1;
    var exponent = ((bytes >> 23) & 0xFF) - 127;
    var significand = (bytes & ~(-1 << 23));

    if (exponent == 128)
        return sign * ((significand) ? Number.NaN : Number.POSITIVE_INFINITY);

    if (exponent == -127) {
        if (significand == 0) return sign * 0.0;
        exponent = -126;
        significand /= (1 << 22);
    } else significand = (significand | (1 << 23)) / (1 << 23);

    return sign * significand * Math.pow(2, exponent);
}

function decodeInt16(bytes) {
    if ((bytes & 1 << 15) > 0) { // value is negative (16bit 2's complement)
        bytes = ((~bytes) & 0xffff) + 1; // invert 16bits & add 1 => now positive value
        bytes = bytes * -1;
    }
    return bytes;
}

function int16_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return bytes[0] << 0 | bytes[1] << 8;
}

function int32_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return bytes[0] << 0 | bytes[1] << 8 | bytes[2] << 16 | bytes[3] << 24;
}

function Decoder(bytes, port) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var decoded = {
        pressure: decodeFloat32(int32_LE(bytes, 0)),
        temp: decodeInt16(int16_LE(bytes,4)) / 100,
        v_batt: decodeInt16(int16_LE(bytes,6)) / 1000,
    };

    // if (port === 1) decoded.led = bytes[0];

    return decoded;
}
```
