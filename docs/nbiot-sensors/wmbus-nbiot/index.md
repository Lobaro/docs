# Wireless M-Bus NB-IoT Gateway

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

### Mobile operator configuration
If you are using a different mobile operator than pre-configured, you should change the mobile operator 
code set in the Config Parameter `Operator`. Operator codes are 5 digit codes that indicate country and 
operator, for example Deutsche Telekom in Germany has `26201` and Tele2 in Sweden has `24007`. You can find 
a [list of mobile operator codes][operator-list] on Wikipedia. 

[operator-list]: https://en.wikipedia.org/wiki/Mobile_Network_Codes_in_ITU_region_2xx_(Europe)

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
    