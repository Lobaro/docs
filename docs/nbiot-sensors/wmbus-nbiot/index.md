#LOB-GW-WMBUS-NB-2 
The Lobaro Wireless M-Bus NB-IoT Gateway.

## Overview

The *Lobaro Wireless M-Bus Gateway* (wMBus) is a simple to use, cost and energy efficient device that can 
receive, cache, and forward meter information from 100 Wireless M-Bus devices, like water meters, 
electricity meters, heat meters, etc. Wireless M-Bus activated meters use FSK radio modulation to 
broadcast their information for up to about 50m. The *Lobaro Wireless M-Bus Gateway* collects this 
information and uploads it using mobile technology (NB-IoT or LTE-M), that was designed for use with 
IoT devices. The meter information is sent to the Lobaro Platform, were it is parsed and displayed 
for each individual meter. Because most Wireless M-Bus telegrams are encrypted, the Platform allows 
adding decryption keys for individual meters, so that the data is decrypted in the Platform.
It is also possible to use the *Lobaro Wireless M-Bus Gateway* without the Platform and connect it 
to your own backend, if it is capable of parsing and decrypting wMBus telegrams.


## Quick start guide

* Make sure SIM card and battery are correctly connected.
* Go to [The Lobaro Platform](https://platform.lobaro.com) and log into your account.
* Go to "Devices" and select your "Lobaro NB-IoT wMBus Gateway".
* If you have several Gateways: the "Address" is printed on the device's case.
* You should see all wMBus Telegrams the Gateway collected so far.
* If the data is encrypted (closed lock symbol 🔒), you can add keys for your devices under "Organisation".
* Push the reset button inside the device, if you want to trigger data collection (will take several minutes).


## State of this document

This manual is currently a stub. It gives a short description of how to start the device and how to 
access collected data in the Lobaro Platform. A more thorough manual will be supplied soon.


## Setting up the device

### SIM card
A SIM card is needed by the Gateway to connect to the mobile network. If you purchased a device together 
with a SIM card, it should already be inserted. If you provide your own SIM card, you will need to insert 
it into the socket. A drawing shows you, how the card must be inserted. The contacts should be facing down.

### Power supply
The Gateway is powered by a 3.6V D-Cell battery, that is connected via a XH connector (a white 2 pin 
socket on the board, labeled "VBat 3V6"). If the battery is initially not connected to the board, you 
will need to plug in the XH connector. When the device powers up, the on-board LED will blink 
green once.

### Resetting the device
Inside the device on the board is a button labeled "RESET". If you push this button while the device is 
running, it will stop and reboot. You should see a green flash of the LED when the device starts again.
Disconnecting the battery from the device will not be enough to reboot the device! The Gateway buffers 
enough power to run for several minutes without a power supply connected (the actutal time is depending on 
what the Gateway does during that time).

### Configuring the device
If you purchased your *Lobaro wMBus Gateway* with a SIM card included and you are using the Lobaro Platform, 
you will not need to change any configuration for the device to work. Instructions on how to change the 
device's configuration using the Lobaro Config Adapter can be found on the 
[Device Configuration](../../tools/lobaro-tool.md#device-configuration) on the manual page for our 
Configuration Tool.

### Mobile operator and LTE band configuration
If you are using a different mobile operator than pre-configured, you should change the mobile operator 
code set in the Config Parameters `Operator` and (LTE) `Band` Operator codes are 5 digit codes that indicate country and 
operator.

Common examples :

| Network    | Country       | Operator      | Band
| :-------------  |:----------------|:----------------|:----------------|
| Deutsche Telekom  | Germany           | 26201  | 8 |
| 1nce.com      | Germany  | 26201  | 8 |
| Vodafone Deutschland    | Germany  | 26202  | 20 |
| TDC A/S | Denmark | 23801  |  20 (?)|
| Telenor Denmark | Denmark | 23802  | 20 (?) |
| Telia DK | Denmark | 23820  | 20 (?) |
| Tele2 | Sweden | 24007  | 20  (?)|
 
Other provider codes you can find in a [list of mobile operator codes][operator-list] on Wikipedia. 
Additional information on operators and bands can be found [here](https://halberdbastion.com/intelligence/mobile-networks).

[operator-list]: https://en.wikipedia.org/wiki/Mobile_Network_Codes_in_ITU_region_2xx_(Europe)

The NB-IoT Bands selected for Europe are B3 (1800 MHz), B8 (900 MHz) and B20 (800 MHz). B8 and B20 are supported by the Lobaro Gateway, B3 is available on special sales request.

## The Lobaro Platform
The easiest way to work with the *Lobaro wMBus NB-IoT Gateway* is the *Lobaro Platform*. You can find it 
under https://platform.lobaro.com &ndash; Log in with the credentials provided by Lobaro.

Your Gateways should be listed under "Devices". If you have multiple devices in your account, you can 
distinguish them by the field "Address". The Address is printed on the box of the Gateway (the Address 
is the IMEI of the modem used by the device; that is the unique hardware address used for mobile 
communication).

### Displaying wMBus data
Open the tab "Device Data" to see a list of all Wireless M-Bus telegrams that your Gateway uploaded.

### Changing configuration
You can see and edit the configuration of the Gateway without physical access to the device from 
the Lobaro Platform. Open the tab "Config" for your device. The current configuration will be shown. 
You can edit individual config entries by clicking on the pencil. After you entered all the values 
you want to change, click the "Update config" button. The new configuration will be sent to the 
device the next time it uploads data to the platform. After changing the configuration, it will reboot 
and start working with the new config. 

For an explanation of the wMBus specific configuration values, please refer to the explanation of 
[wMbus parameters](/lorawan-sensors/wmbus-lorawan/#wmbus-bridge-parameters) in the LoRaWAN wMBus Bridge.

The remaining configuration parameters (Host, Port, APN, Band, ...) are used to configure the way 
the device connects to the mobile provider and to the Lobaro Platform. There is no need to change 
these values when using the Gateway with the Lobaro Platform.

### wMBus encryption Keys
Many meters are sending encrypted data. In order to get the values out of that encrypted telegrams, 
you will need to provide the decryption key to the Platform. Go to "Organisation" / "wMBus" to add 
encryption keys. You will need to set a key for a specific meter (identified by its ID).

Once a key is entered for a device, any telegram received after that will be decrypted and listed 
in clear text under "Device Data".


## Forwarding data to your own system
If you want the received data inside your own system, you can add an Integration inside the Lobaro 
Platform that forwards all data to your system. We currently supply a REST API that allows you 
to query data from our platform actively, as well as a HTTP(S) integration, that pushes incoming data 
to your system when it is received.


## Configuration with the Config Adapter
Instead of using the Lobaro Platform, you can use the Lobaro Config Adapter and the Config Tool to 
change the Configuration directly in your hardware. This can be useful when you want to change 
configuration while the mobile connection does not work (or if you do not want to use the Lobaro 
Platform). See [Lobaro USB configuration adapter](/tools/usb-config-adapter.html) for more information.


## Using Raw UDP
In default configuration, the Gateway communicates using 
[CoAP](https://en.wikipedia.org/wiki/Constrained_Application_Protocol) with messages that are designed 
to work with our Lobaro Platform as a backend. If you want to connect the Gateway directly to your own 
backend, it can be hard to implement an endpoint.

### CBOR messages over UDP
Starting with Firmware version 0.5.0, the Gateway supports a second format, where wMBus telegrams are uploaded without CoAP over UDP. When the 
configuration Parameters `UdpHost` and `UdpPort` are set to a destination (IP address and port number), 
wMBus telegrams will be sent to that destination instead of the Lobaro Platform. It will be sent as 
a [CBOR](https://en.wikipedia.org/wiki/CBOR) object using raw UDP packets without CoAP. CBOR can easily be 
parsed in most programming languages using existing libraries. It is similar to JSON but uses a binary 
representation.

!!!warning "Limitations of UDP"

    Because UDP has no validation mechanism, there will be no retransmission in case of packet loss.
    You will be able to spot missing packets by gaps in the frame number. When implementing this, please 
    keep in mind, that UDP packets are not guaranteed to arrive in the order they are sent.

### Controlling the device
When `UdpHost` and `UdpPort` are set while `Host` and `Port` are referring to the Lobaro Platform, the 
Gateway will upload the wMBus telegrams to the UDP destination but will also sent diagnostic messages to 
the Platform. In this configuration you can still use the features of the Lobaro Platform to control the 
device for configuration changes or firmware updates, while receiving your wMBus data directly to your 
own backend.

### Format

#### Schema
The CBOR object contains the following information
````json
{
    "d": {
      "rssi": <int: RSSI>,
      "vbat": <int: Supply voltage in mV>,
      "monitor": <string: human readable diagnostic information>,
      "telegram": <bytes: wmbus telegram>,
      "timestamp": <int: unix timestamp, time of reception>,
      "temperature": <int: device temperature in 1/10°C>
    },
    "i": <string: device's IMEI>,
    "n": <int: frame number>
}
````

| Name | Explanation |
|------|-------------|
| `rssi` | [Received signal strength indication](https://en.wikipedia.org/wiki/Received_signal_strength_indication) indicating the quality of the received signal. |
| `vbat` | Supply voltage to the Gateway, measured in millivolts (mV). |
| `monitor` | Human readable diagnostic string. The format of this information subject to change and should not be relied on. |
| `telegram` | Bytes of the received wMBus telegram as a byte string. |
| `timestamp` | Time of reception of the telegram in the gateway, given as a [Unix Timestamp](https://en.wikipedia.org/wiki/Unix_time). |
| `temperature` | Temperature inside the Gateway, measured in tenth of Degree Centigrade (d°C). |
| `i` | IMEI of the Gateway's Modem, uniquely identifying the Device. |
| `n` | Frame number. Starts at `1` for the first UDP-upload after boot and is increased for every upload. |

#### Example
The following shows an example message if you display it as JSON. 
In the CBOR object, the `telegram` is stored as a byte string. Because JSON does not support binary data, in this
example it is encoded using base64.
````json
{
    "d": {
      "rssi": -99,
      "vbat": 3688,
      "monitor": "connected:1, conMode:1, reg:5, tac:D71E, ci:019C1307, psm:11100000, tau:00001100, RSRP:56(2/4), RSRQ:24(3/4), SNR:37(3/4), conTime:3, conFails:0",
      "telegram": "SUSTRHkFAYg0CHgN/181AIJnADXIv1WtPFse1mYcZZQLiPR/aujF9e46meEB6CIkxJmHUEd6xPdAmop3uqIt4yWMgbwEbToKiCc=",
      "timestamp": 1594201536,
      "temperature": 240
    },
    "i": "123456101550542",
    "n": 7
}
````

Explanation:
````
UDP-Uplink #7 from Gateway with IMEI 123456101550542
Status of Gateway during upload:
  internal Temperature: 24.0°C
  supply Voltage: 3.688V
  Mobile provider, Cell-ID: 019C1307
Received wMBus Telegram:
  time of recept: 2020-07-08T09:45:36 (UTC)
  telegram (as hex): 49449344790501883408780dff5f350082670035c8bf55ad3c5b1ed6661c65940b88f47f6ae8c5f5ee3a99e101e82224c4998750477ac4f7409a8a77baa22de3258c81bc046d3a0a8827
  rssi: -99
````

## Troubleshooting

!!! faq "I did not get a username/password for the *Lobaro Platform*."

    Please contact [contact Lobaro](https://www.lobaro.com/contact/) to get your account information.

!!! faq "I do not see my Gateway listed unter "Devices"."

    It is possible that the purchased device has not been added to your account. Please check if you 
    got an *Activation Code* with your hardware. If so, you can enter it under "Tools" / "Hardware Activation" 
    in order to claim the device for your account.
 
!!! faq "I cannot find data for my specific meter."

    Make sure your Gateway collected data since you brought it close to the meter (check timestamps on 
    data). With standard configuration, it only collects data every 8 hours. You can press the "RESET" 
    Button inside the device to make it reboot and start collecting data.
    
    Also check the specifications of your wMBus meter. How often does it send data? What mode does it 
    use? Using standard configuration, the wMBus Gateway only collects C-Mode and T-Mode telegrams. 
    If your meter is sending S-Mode, you will need to change the Gateway's configuration.

!!! faq "Data for my meter only shows "Payload encrypted"."

    Most meters encrypt the data they are sending out (information about water/energy usage is 
    personal data). In order to see values from encrypted wMBus telegrams, your will need to supply 
    the decryption key for your meter (you should be able to get the key where you got the meter).
    You can add the key in the Lobaro Platform under "Organisation" / "wMBus". You will have to add 
    a key for a specific meter (identified by the meter's ID). 
    
    After you supplied the key, new telegrams that are received should be decrypted so that you 
    can see the values inside the telegrams.
    