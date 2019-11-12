# Wireless mBUS over LoRaWAN Bridge

## Firmware

**Downloads:** 

!!! info "Stable Release V1.6"
    Current stable release for production environments:
    
    Download: [app-wMbusLoraBridge-1.6.0.hex](firmware/app-wMbusLoraBridge-1.6.0.hex)
    
---------------
    
!!! warning "Experimental Release V2.3.x"
    Experimental firmware which adds new features like
    
    * Optional LoRaWAN v1.1 support 
    * LoRaWAN time synchronisation
    * LoRaWAN Downlink configuration
    * wMBUS learning mode for meter tx intervals
    * LoRaWAN Class C support
    * Optional alternative payload format with better LoRaWAN SF adoption
    
    The usage of most new features (beside the learning mode) is described in our [Introduction to LoRaWAN article](https://docs.lobaro.com/background/lorawan.html#lorawan-configuration).
    An updated manual for the wireless MBUS bridge will follow soon. The current wireless MBUS bridge documentation is only valid for firmware up to v1.6!
    
    **For production environments consider using the stable & well documented release V1.6 above instead!**
    
    Download: [app-wMbusLoraBridge-TZ0-2.3.0+LoRa.hex](firmware/app-wMbusLoraBridge-TZ0-2.3.0+LoRa.hex)
    
## Changelogs

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

---

### v1.6.0 (20.05.2019)
- Add additional config parameter "cmodeCompatibility" to allow wideband receive as fallback
- fix issue with crc validation of unencrypted meters

### v1.5.8 (18.02.2019)
- Reduce chance to miss wMbus packet when there is a lot of traffic

### v1.5.7 (07.01.2019)
- Fix issue with very large T1 mode telegrams

### v1.5.6 (20.11.2018)
- Support new FRAM memory type
- Improve random generator behaviour

### v1.5.5 (14.11.2018)
- internal use only

### v1.5.4 (25.10.2018)
- Support LoRaWAN NBTrans > 1
- Don't allow to set unsupported FSK DR during ADR

### v1.5.3 (18.10.2018)
- Fix bug with not working deduplication of same wMBUS messages during listen intervals

### v1.5.2
- Support LoRaWAN Rx1 DataRate Offset

### v1.5.1
- New parameter "resetHours" (default = 0): Hours after which the firmware will reset and rejoin the network (0 = never)

### v1.5.0
- Changed duration parameters to seconds: cmodeDur -> cmodeDurSec and smodeDur -> smodeDurSec
- Status packet is 2 byte shorter, version is encoded with 3 bytes now
- Upload correct battery status in DevStatusReq

LoRaWAN Changes:
- Support ADR ChMask to disable Channels
- Restore default channels when loosing uplink connectivity
- Support LoRaWAN NewChannelReq MAC command
- Support LoRaWAN DlChannelReq MAC command
- Support LoRaWAN RxParamSetupReq MAC command
- No LoraWAN MAC Commands are dropped when unknown MAC command is received
- Fix bug with endless loop when unknown LoRaWAN MAC command was received
- Improve debug logs of LoRaWAN stack

### v1.4.1
- Bugfix: Allow big wMBUS raw messages > 160 Bytes in T1 mode
- Improved cfg parameter explanation texts
- Improved wmbus telegram terminal output

### v1.4.0
- New Parameters for ADR (OTAA = false):  AppSKey, NetSKey, DevAdr
- New Parameter: OTAADelay to configure the delay between OTAA joins on fail + [0% ... 30%]
- Bugfix: Support LoRaWAN Status MAC command
- Bugfix: TxPower was not considered
- Requires Lobaro Tool > v1.3.1 for configuration

### v1.3.1
- Fix ADR Bugs

### v1.3.0
- Allow to enable ADR (default: enabled)

### v1.2.0
- Increase config version (Config will be reset)
- Introduce LoRaWAN default parameter

### v1.1.0
- New Filter: device id (devFilter), device type (typFilter), manufacturer (mFilter)
