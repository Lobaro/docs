# Wireless mBUS over LoRaWAN Bridge

The latest firmware version is **v2.3.9** *(since 2020-02-21)*

!!! faq "Which firmware version am I using right now?"
    You can see the firmware version of your device by inspecting the USB log output in our PC maintenance tool. Or use the information
    send in the daily status uplink message send on LoRaWAN port 1.
    
!!! faq "How do I update the firmware?"
    Please use the [Lobaro Maintenance Tool](../../tools/lobaro-tool.md){: target="_blank"} and the [Lobaro PC usb adapter](../../tools/usb-config-adapter.md).

## Firmware Downloads

!!! caution "Select correct Hardware revisions"
    Due to permanent development there exist two main hardware revisions of the Lobaro wMBUS over LoRaWAN bridge hardware.
    **Please use the images below to select the right firmware file for your given hardware.**
    
    The (dedicated) revision 2.x has a simpler production process which helped us to optimize bigger deployments. 

 
### Hardware Revision 2.x (active since 2020)

![wMBUS LoRaWAN bride HW Rev2](files/Lobaro-wMBus-LoRaWAN-v2-Housing-open.jpg){: style="height:150px;display: block; margin: 0 auto;"}

Download: 

* [app-wMbusLoraBridge-TZ0-2.3.9+dedicated.hex](firmware/app-wMbusLoraBridge-TZ0-2.3.9+dedicated.hex) (hardware v2.x) [current release]


### Hardware Revision 1.x (active since 2017)

![wMBUS LoRaWAN bride HW Rev1](files/Lobaro-wMBus-LoRaWAN-v1-Housings-open.jpg){: style="height:150px; display: block; margin: 0 auto;"}

Download:

* [app-wMbusLoraBridge-TZ0-2.3.9+LoRa.hex](firmware/app-wMbusLoraBridge-TZ0-2.3.9+LoRa.hex) (hardware v1.x) [current release]

!!! info "Please note revisions 1.x are no longer the default variant since 02/2020"
    The v1 hardware revisions are still active but only on special sales inquiry! 
    
    Please [contact Lobaro](https://www.lobaro.com/contact/){: target="_blank"} if you are interested in revisions 1.x hardware.

## E-Mail release notifications 

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
    
    Make sure to select the **"Firmware Updates"** checkbox!    
    
## Release changelog

### v2.3.9 (21.01.2020)
- Fix minor memory Leak in wMBus parser 

### v2.3.8 (09.01.2020)
- Fix issue in mbus store which triggered raw system resets

### v2.3.7 (08.01.2020)
- Internal use only

### v2.3.6 (07.01.2020)
- Increase wireless MBUS C1/T1 mode receive bandwidth
- Change detection method for incomming C1/T1 mode telegrams

### v2.3.5 (23.12.2019)
- Improve wireless M-BUS C1/T1 mode receive sensitivity

### v2.3.4 (13.12.2019)
- Improve reset line usage (dedicated only)
- Fix issue with bad measurement timing for status message battery voltage 

### v2.3.3 (10.12.2019)
- Optimize LoRaWAN RX1/RX2 window timing + add more precise downlink logs

### v2.3.2 (18.11.2019)
- Fix crash on receiving certain long non-standard telegrams.

### v2.3.1 (13.11.2019)
- Improve Filtering of non-standard wMBus-telegrams

### v2.3.0 (12.11.2019)
- Migrate to alternative dedicated board
- Fix Deep Sleep settings on dedicated board
- Fix rx/handling of S1-mode CRCs of unencrypted telegrams in wMBUS driver

### v2.2.0 (unreleased)
- Integrated with new LoRaWAN stack, now supporting v1.1, Class C, downlinks, ...

### v2.1.3 (11.03.2019)
- New Parameter: learnedListenSec to define how long to listen when meters was learned

### v2.1.2 (11.03.2019)
- Packet size depending on SF with payloadFormat = 1

### v2.1.1 (20.02.2019)
- New Parameter: timeSync - request time with status packet and upload status packets until we got a valid time
- New Parameter: payloadFormat - 0 = as in older versions, 1 = new split format including receive timestamp (see below)
- The upload is randomly delayed by 1-30 seconds to avoid collisions

New payload Format "1":

LoRaWAN Port: 101
Message: <5 byte Timestamp UTC><raw wMbus Telegramm>

Messages are split into chunks with 1 prefix byte:
Prefix byte bits: <7..2 RESERVED><1 LAST><0 FIRST>
The FIRST bit is set on the first packet.
The LAST bit is set on the last packet.
Together with the LoRaWAN framecounter, a whole message can be reconstructed in the backend.

### v2.0.0 (12.02.2019)
- Leanring mode to learn up to 20 devices with their intervals.
- New Parameter: learningMode - set to true to allow deep sleep based on learned intervals (default: false)
- New Parameter: meterIntervalSec - predefine the sending interval of the meter, so it needs not to be learned (default: 0 = learn intervals)

Learning mode:
Up to 20 devices are learned. When received the first time, the device is added to the list. When received the second time the interval is calculated.
The second step is omitted when "meterIntervalSec" ist set to any value but 0.
When learning was completed during one listening interval the device will only wakeup 10 seconds before and after receiving the learned meters in future.
When missing one device for whatever reason, the bridge will start the learning mode again and stay awake for one full listening period.

While receiving learned sensors the maximum receive interval is doubled but ends as soon as all learned devices are received.

## Older v1.x firmware

!!! warning "Old v1.x firmware version"
    The v1.x firmware branch is no longer recommended for use. 
    
    If you rely on these older version you can find it at the end of the outdated [v1 documentation](1.x/index.md).
    

---

