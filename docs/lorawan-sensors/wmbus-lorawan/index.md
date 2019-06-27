# Wireless mBUS over LoRaWAN Bridge

![Lobaro wmbus LoRaWAN Bridge](files/Übersicht-wmbus-lora.jpg){: style="width:100%"}


## Target Measurement / Purpose
Forwarding of wireless M-BUS messages via LoRaWAN.

** Features **

* wMBUS C1/T1 and S1 modes
* 868 MHz only (433 MHz on request)


## PDF Documentation

* [PDF Manual (en) for firmware V1.5.1](files/lorawan-wmbus-bridge_en.pdf){: target="_blank"}
* [Product Flyer](files/W-MBus Lora Bridge Flyer.pdf){: target="_blank"}
* [Declaration of Conformity](files/scan-ce-conformity-wmbus-lorawan.pdf){: target="_blank"}


## Parser
### TTN

```javascript
function readVersion(bytes, i) {
    if (bytes.length < 3) {
        return null;
    }
    return "v" + bytes[i] + "." + bytes[i + 1] + "." + bytes[i + 2];
}

function Decoder(bytes, port) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var decoded = {};

    if (port === 9) {
        decoded.devStatus = bytes[0];
        decoded.devID = bytes[1] | bytes[2] << 8 | bytes[3] << 16 | bytes[4] << 24;
        decoded.dif = bytes[5];
        decoded.vif = bytes[6];
        decoded.data0 = bytes[7];
        decoded.data1 = bytes[8];
        decoded.data2 = bytes[9];
    }

    // example decoder for status packet by lobaro
    if (port === 1 && bytes.length == 9) { // status packet
        decoded.FirmwareVersion = String.fromCharCode.apply(null, bytes.slice(0, 5)); // byte 0-4
        decoded.Vbat = (bytes[5] | bytes[6] << 8) / 1000.0; // byte 6-7 (originally in mV)
        decoded.Temp = (bytes[7] | bytes[8] << 8) / 10.0; // byte 8-9 (originally in 10th degree C)
        decoded.msg = "Firmware Version: v" + decoded.FirmwareVersion + " Battery: " + decoded.Vbat + "V Temperature: " + decoded.Temp + "°C";
    } else if (port === 1 && bytes.length == 7) {
        decoded.FirmwareVersion = readVersion(bytes, 0); // byte 0-2
        decoded.Vbat = (bytes[3] | bytes[4] << 8) / 1000.0; // originally in mV
        decoded.Temp = (bytes[5] | bytes[6] << 8) / 10.0; // originally in 10th degree C
        decoded.msg = "Firmware Version: " + decoded.FirmwareVersion + " Battery: " + decoded.Vbat + "V Temperature: " + decoded.Temp + "°C";
    }

    return decoded;
}


```
[^ 1]: Energy Meter, Water Meter and House Icons made by Freepik from www.flaticon.com is licensed by CC 3.0 BY
Heater Icon made by Nikita Golubev from www.flaticon.com is licensed by CC 3.0 BY
Map Icon Icons made by Darius Dan from www.flaticon.com is licensed by CC 3.0 BY
LoRaWAN Logo © LoRa Alliance, NB-IoT Logo © GSM Association, wM-Bus Logo © www.m-bus.com
