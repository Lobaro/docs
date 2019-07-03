# LoRaWAN
LoRaWAN stands for **Lo**ng **Ra**nge **W**ide **A**rea **N**etwork and is a low power 
wireless network protocol. It uses the wireless transmission technology *LoRa*, 
owned by Semtech Corporation. Although LoRa is a proprietary and patented LoRaWAN can 
is openly specified by the [LoRa Alliance](https://lora-alliance.org/) and can 
be used freely if you have the necessary hardware. It is using publicly available 
frequencies to transmit its data.

To operate LoRaWAN Sensors you need a **LoRaWAN Network Server**. A free to use, open source network server is The Things Network.
They provide a very good documentation about the overall architecture and features of LoRaWAN:

[LoRaWAN Documentation (by TTN)](https://www.thethingsnetwork.org/docs/){: target="_blank"}


## Timestamps

Many of our devices include timestamps somewhere in their payloads. The encoding of 
timestamps in our payload is the same in all our LoRaWAN devices except some of our oldest. 
There are some details you should be aware of.

1. Format: Points in time are represented as [UNIX-timestamps](https://en.wikipedia.org/wiki/Unix_time)
   within our products. That is a integer value indicating the number of seconds that have passed 
   since 0:00h on January 1 1970 in UTC.
2. Encoding: We encode the UNIX-timestamp in a signed 40 bit big endian integer (`int40`, 5 bytes long).
   The 40 bit integer is unconventional but used with good reasoning. UNIX-timestamps have traditionally 
   been stored as signed 32 bit integers (`int32`). This poses the problem that points in time 
   [later than January 2038 cannot be expressed](https://en.wikipedia.org/wiki/Year_2038_problem).
   A simple solution to this is switching to store timestamps in singed 64 bit integers. This is 
   a suitable solution for modern computers and our devices do that internally for calculations. 
   The problem is that 64 bit integers take 8 bytes of space, and with LoRaWAN every byte is 
   precious. 3 of those 8 bytes will be zeros for thousands of years to come, so we chose to 
   increase the size of our timestamps by only a single byte to 40 bit. This lets us store 
   timestamps up unto the year 19391, which we dare to say is enough. <br>
   Storing numerical values with a number of bits that is not a power of 2 might be unusual, but there 
   is nothing wrong with it. If you have problems decoding 40 bit values you can find an example 
   implementation in JavaScript below in our example TTN parser.
3. Device's internal clock: The timestamp uploaded by the devices is always referring to the 
   device's internal clock. That clock is not always in sync with actual time. In fact when you 
   power up the device it has no way to know what time it is. It sets the internal clock to 
   2010-01-01T00:00:00. For many applications this is not a problem! If you set the device's 
   cron to execute at 0h, 6h, 12h, and 18h it will activate every 6 hours at the same times 
   every day. But when exactly the device is activated depends on the the time it was first 
   powered on. <br>
   If you require your device to run in sync with actual time you can set its clock 
   using the configuration adapter and the Lobaro Tool. LoRaWAN 1.1 also introduced time 
   synchronization over the air which is supported by some of our devices. You will need 
   to use a LoRaWAN Network Server that also supports this feature.


## Parser

LoRaWAN Application Servers need to decode sensor payloads. This is done with custom parser code.
We provide TTN compatible JavaScript parsers as reference implementation for all our devices.

Some common help functions for this parsers can be found in the following example:

```javascript
function toHexString(byteArray) {
    var s = '';
    byteArray.forEach(function (byte) {
        s += ('0' + (byte & 0xFF).toString(16)).slice(-2);
    });
    return s;
}

function signed(val, bits) {
    if ((val & 1 << (bits - 1)) > 0) { // value is negative (16bit 2's complement)
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

function int40_BE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[0] << 32 | bytes[1] << 24 | bytes[2] << 16 | bytes[3] << 8 | bytes[4] << 0, 40);
}

function int16_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[1] << 8 | bytes[0] << 0, 16);
}

function int24_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[2] << 16 | bytes[1] << 8 | bytes[0] << 0, 24);
}

function int32_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(bytes[3] << 24 | bytes[2] << 16 | bytes[1] << 8 | bytes[0] << 0, 32);
}

function int64_LE(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return signed(
        bytes[7] << 56 | bytes[6] << 48 | bytes[5] << 40 | bytes[4] << 32 |
        bytes[3] << 24 | bytes[2] << 16 | bytes[1] << 8 | bytes[0] << 0, 32);
}

function toNumber(bytes) {
    var res = 0;
    for (var i = 0, s = 0; i < bytes.length; i++) {
        res |= bytes[i] << s;
        s += 8;
    }
    return res;
}

function bit(byte, idx) {
    return (byte & (0x01 << idx)) != 0;
}

function readVersion(bytes, idx) {
    bytes = bytes.slice(idx || 0);
    return "v" + bytes[0] + "." + bytes[1] + "." + bytes[2];
}

// EXAMPLE PARSER:

// 0001000566566D38000000000600E600EA0C02400040E740C7
// 00 01 00 05 66 56 6D 38 00 00 00 00 06 00 E6 00 EA 0C 02 400040E740C7
/*
Alarm
----------
reason: Button 2 (5)
sensorTime: 946689638
vBat: 3306
temperature: 230
mems: <64,-6336,-14528>
button1State: 0
button2State: 1
alarmAgeSec: 6
 */
function Decoder(bytes, port) {
    // Decode an uplink message from a buffer
    // (array) of bytes to an object of fields.
    var decoded = {
        "version": readVersion(bytes, 0),
        "reason": bytes[3],
        "sensorTime": int64_LE(bytes, 4),
        "alarmAgeSec": int16_LE(bytes, 12),
        "temperature": int16_LE(bytes, 14) / 10,
        "vBat": int16_LE(bytes, 16) / 1000,
        "button1State": bit(bytes[18], 0),
        "button2State": bit(bytes[18], 1),
        "memsX": int16_LE(bytes, 19),
        "memsY": int16_LE(bytes, 21),
        "memsZ": int16_LE(bytes, 23),
    };

    // if (port === 1) decoded.led = bytes[0];

    return decoded;
}
```