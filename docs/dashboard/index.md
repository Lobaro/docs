# Lobaro Dashboard

The Lobaro Dashboard can be found at <a href="https://dashboard.lobaro.com" target="_blank">dashboard.lobaro.com</a>

## Overview

The Dashboard shows device data received via various data sources like LoRaWAN, NB.IoT, GSM, etc..

Incoming data from a connected Sensor received by a `Datasource` and mapped to a single `Device` 
where the `Raw Data` is saved and a `Parser` gets executed. The result of the parser is saved as `Device Data`
used for visualization inside the dashboard.

```mermaid
graph LR;
    data>Sensor Data]-->ds(Datasource)
    device(device)
    ds-->device
    device-->parser(Parser)
    device-->raw[Raw Data]
    parser-->parsed[Device Data]
```



## Parser

A Parser takes raw input from the Sensor API and converts the data into a unified format used by the Dashboard.
In addition the parser can access an API to set device level properties and additional meta information outside of the actual data record.

Parsers are organized in 3 levels:

* Hardcoded default parser
* DeviceType parser
* Device parser

When no parser on device level is defined, the parser for the device type will be executed. When no parser for the device type is defined, a hardcoded default parser will be executed.

Parsers are written in JavaScript.

**Example**

```
function Parse(input) {
  // Decode an incoming message to an object of fields.
  var decoded = {input: input};

  return decoded;
}
```

### JS Parser API

All functions are optional. Not calling them will not change any data.

Update the physical location of the sensor
```
Device.setLocation(lon, lat)
```

Set an arbitary device property, displayed on the "Overview" tab of the device
```
Device.setProperty("key", "value);
```

Set the Sensor time of the current data record. Used for display, filter, sorting
```
Record.setTime(new Date());
```

