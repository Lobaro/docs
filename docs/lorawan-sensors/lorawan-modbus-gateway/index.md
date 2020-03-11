# LoRaWAN Modbus Gateway

The Lobaro LoRaWAN Modbus Gateway is a LoRaWAN Gateway with integrated LoRaWAN Network Server providing sensor data via Modbus.

## Hardware Components

* LoRaWAN Gateway
* External SSD for storage
* USB-Modbus Adapter

The internal SD-Card is write protected. All variable data is stored on the external SSD to avoid SD-Card damage over time.

## Software Components

* Chirpstack Network Server
    * Semtech Packet Forwarder
    * Chirpstack Gateway Bridge
    * Chirpstack Network Server
    * Chirpstack Application Server
    * Postgres
    * Redis
* Lobaro Modbus Server

Usually you will not need to change anything inside the Chirpstack Application Server. 
All devices are managed by the Lobaro Modbus Server.

## Remote access {: #remote-access}

Per default the gateway obtains the IP address via DHCP. 
If configured with a fixed IP address, the gateway has a label with the configured IP address and subnet.

The gateway can be accessed via SSH on port 22. Default login credentials are provided by Lobaro.

## Lobaro Modbus Server

The Lobaro Modbus Server (`lobaro-modbus-server`) is responsible for fetching data from the local LoRaWAN Network Server
and provides received data via modbus.

To change any filed on the SD Card (including all config files) you need to execute the script:

    ~/enableWriteAccess.sh
    
To disable write access, restart the gateway or execute:

    ~/disableWriteAccess.sh


For editing files `vim` is installed.

Open or change configuration of the Lobaro Modbus Server:

    sudo vim /etc/lobaro-modbus-server/lobaro-modbus-server.yml
    

After editing the configuration `lobaro-modbus-server` must be restarted:

     sudo systemctl restart lobaro-modbus-server
     
Check the status with
     
     sudo systemctl status lobaro-modbus-server

Check the logs with

    sudo journalctl --no-pager -e -u lobaro-modbus-server

### Debugging

Beside checking the logs, you can also analyze the files in the `dataDir` (see config).

There are two files that help writing the config and checking the results.

* `device-data.json` - contains raw json received from Chirpstack. Can be used to verify `mapping[].register.value` configuration.  
* `register-map.json` - contains all register values provided via modbus. All values are formatted as `int`.

Example `device-data.json`:
```
{
  "0000000000000000-1": {
    "adr": true,
    "applicationID": "1",
    "applicationName": "default",
    "data": "AAMEAOQN1g==",
    "devEUI": "0000000000000000",
    "deviceName": "0000000000000000",
    "fCnt": 1,
    "fPort": 1,
    "object": {
      "temp": 22.8,
      "vBat": 3.542,
      "version": "v0.3.4"
    },
    "rxInfo": [
      {
        "gatewayID": "0000000000000000",
        "loRaSNR": 8.8,
        "location": {
          "altitude": 0,
          "latitude": 0,
          "longitude": 0
        },
        "name": "default",
        "rssi": -36,
        "uplinkID": "ce2e086a-d747-4813-9428-b7a4a45abcc8"
      }
    ],
    "txInfo": {
      "dr": 0,
      "frequency": 868300000
    }
  }
}
```

Example `register-map.json`:
```
{
  "Register": {
    "100": {
      "Val": 0,
      "Type": 1,
      "UpdatedAt": "2020-02-07T13:44:03.39750918Z"
    }
  }
}
```

### Configuration file

Example config:

```
# The application stores persistent data at this path
dataDir: /mnt/ssd/var/data/lobaro-modbus-server/

# Chipstack configuration. Required to manage configured LoRaWAN devices.
chirpstack:
  server: http://localhost:8080
  broker: localhost
  appId: 1
  username: admin
  password: admin

# Modbus configuration. 
# Serial mode is fixed at: 8 Data bits, Even Parity, 1 Stop bit (8E1)
modbus:
  baud: 19200
  slaveId: 1
  port: /dev/ttyUSB0

# Mapping from LoRaWAN Sensors to Modbus Registers
mapping:
  # LoRaWAN Sensor parameters
  - devEUI: 0000000000000000
    appKey: 00000000000000000000000000000000
    # Chirpstack Device Profile to use. Includes the Payload Parser.
    devProfile: lobaro-environment
    # Register mapping for this device
    # One device can fill any number of registers.
    # The server will check for overlapping definitions on start.
    register:
        # Modbus Address (do NOT prefix with 0, else it's octal)
      - addr: 1
        # The value to be mapped.
        # Usually the value is taken from the Chirpstack Parser result JSON
        # and can be selected via JSON Path as handled by https://github.com/tidwall/gjson
        # There are some special values:
        # @age - age of last update in minutes (for any register of this device)
        # @now - Current time as Unix Timestamp
        value: "@age" # age of last update in minutes (for any register of this device)
        # Data type of the value. Default byte order is LittleEndian
        # Supported types are: int16, uint16 (more will come in future versions)
        type: int16
        # The value is only for messages on the specified port, 0 for "every". Default: 0
        port: 0
        # The register value is multiplied with the given factor, 0 is irgnored. Default: 1
        factor: 1

      - addr: 2
        port: 1 # status packet
        value: "object.vBat"
        type: int16
        factor: 1000
      - addr: 3
        port: 2
        value: "object.temperature"
        type: int16
        factor: 10
      - addr: 4
        port: 2
        value: "object.humidity"
        type: int16
        factor: 10
      - addr: 5
        port: 2
        value: "object.pressure"
        type: int16
        factor: 10
      - addr: 6
        port: 2
        value: "rxInfo.0.rssi"
        type: int16
      - addr: 7
        port: 2
        value: "txInfo.dr"
        type: int16

  # A second device as example
  - devEUI: 0000000000000000
    appKey: 00000000000000000000000000000000
    devProfile: lobaro-one-wire
    register:
      - addr: 100
        value: "@age" # age of last update in minutes (for any register of this device)
        type: int16
      - addr: 101
        port: 1 # status packet
        value: "object.vBat"
        type: int16
        factor: 1000
      - addr: 102
        port: 2
        value: "object.sensors.0.temp"
        type: int16
        factor: 10
      - addr: 103
        port: 2
        value: "rxInfo.0.rssi"
        type: int16
      - addr: 104
        port: 2
        value: "txInfo.dr"
        type: int16
```
## Chirpstack

The gateway uses a local Chirpstack server. Access management interface on `https://<gw-ip>:8080`.

Documentation can be found on [Chripstack.io](https://chirpstack.io){: target="_blank"}.

For each type of device the `lobaro-modbus-server` needs to reference a Device Profile. 
See: [Chirpstack Device Profile Management](https://www.chirpstack.io/application-server/use/device-profiles/){: target="_blank"}.
The Device Profile of each LoRaWAN device must be referenced by its name or UUID in the `lobaro-modbus-server.yml` config file.

## Gateway administration

When ever any file on the SD-Card need to change make sure to execute `~/enableWriteAccess.sh`

### Change password

Login via SSH (see: [Remote access](#remote-access))

    passwd

### Change IP address

    sudo vim /etc/network/interfaces

```
pi@LoRaGateway:~ $ cat /etc/network/interfaces
# interfaces(5) file used by ifup(8) and ifdown(8)

# Please note that this file is written to be used with dhcpcd
# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

# DHCP (Default, comment line to disable DHCP)
iface eth0 inet manual

# Fixed IP (Uncomment to enable or use /etc/dhcpcd.conf)
#auto eth0
#iface eth0 inet static
#    address 10.0.0.42/24
#    gateway 10.0.0.1
```