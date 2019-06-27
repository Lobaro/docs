# GPS Tracker LoRaWAN 

![Product Image](files/GPS_LoRaWAN_Tracker_Opencase_frei.jpg){: style="height: 300px"}

## Documentation

* [User Manual (en)](files/lorawan-gps-tracker_en.pdf)
* [CE Conformity](files/scan-ce-conformity-gps-lorawan.pdf)

## Parser

### The Things Network

For Version 4.x and 5.x

```javascript
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

function int24_BE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[0] << 16 | bytes[1] << 8 | bytes[2] << 0, 24);
}

function int32_BE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[0] << 24 | bytes[1] << 16 | bytes[2] << 8 | bytes[3] << 0, 32);
}

function DecoderPort2(bytes) {
    // Format for firmware 5.0.x
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.

    var Temp = int16_BE(bytes, 0) / 10;
    var VBat = int16_BE(bytes, 2) / 1000;
    var Lat = int32_BE(bytes, 4) / 100000;
    var Lon = int32_BE(bytes, 8) / 100000;
    var Alt = int24_BE(bytes, 12) / 100;
    var Valid = bytes[15] & 0x01;
    var Active = bytes[15] & 0x02;
    var SatCnt = bytes[16];

    return {
        Temp: Temp || 0,
        VBat: VBat || 0,
        Lat: Lat || 0,
        Lon: Lon || 0,
        Alt: Alt || 0,
        Valid: Valid,
        Active: Active,
        SatCnt: SatCnt
    };
}

function DecoderPort1(bytes) {
    // legacy format, firmware 4.x
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.

    var button_number = bytes[0];
    var Temp10tel = (bytes[1] << 8) | (bytes[2] << 0);
    var VBat1000tel = (bytes[3] << 8) | (bytes[4] << 0);
    var lat_deg = bytes[5];
    var lat_min = bytes[6];
    var lat_10000min = (bytes[7] << 8) | (bytes[8] << 0);
    var long_deg = bytes[9];
    var long_min = bytes[10];
    var long_10000min = (bytes[11] << 8) | (bytes[12] << 0);
    var gpsValid = (bytes[13] !== 0);  // Missing byte = true

    return {
        button_number:button_number || 0,
        Temp10tel: Temp10tel || 0,
        VBat1000tel: VBat1000tel || 0,
        lat_deg: lat_deg || 0,
        lat_min: lat_min || 0,
        lat_10000min: lat_10000min || 0,
        long_deg: long_deg || 0,
        long_min: long_min || 0,
        long_10000min: long_10000min || 0,
        gps_valid: gpsValid || false
    };
}

function Decoder(bytes, port) {
    switch (port) {
        case 1:
            return DecoderPort1(bytes);
        case 2:
            return DecoderPort2(bytes);
        default:
            return {"error":"Unsupported port","port":port};
    }
}
```