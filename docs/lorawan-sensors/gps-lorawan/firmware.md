# LoRaWAN GPS Tracker

## Firmware Downloads


### Hardware Revision 2.x

![Product GPS Tracker HW2](files/gpslorawan.png){: style="height:250px;display: block; margin: 0 auto;"}

**Downloads**: 

* [app-lorawan-gps-button-6.1.0.hex](firmware/app-lorawan-gps-button-6.1.0.hex) (hardware v2.x) [current release]

* [app-lorawan-gps-button-6.0.3.hex](firmware/app-lorawan-gps-button-6.0.3.hex) (hardware v2.x)

### Hardware Revision 1.x 

![GPS Tracker HW1](files/config.png){: style="height:150px; display: block; margin: 0 auto;"}

**Downloads**:

* [app-lorawan-gps-button-5.0.5.hex](firmware/app-lorawan-gps-button-5.0.5.hex) (hardware v1.x)
* [app-lorawan-gps-button-4.0.9.hex](firmware/app-lorawan-gps-button-4.0.9.hex) (hardware v1.x)

!!! info 
    The LoRaWAN GPS data uplink data encoding has been changed between firmware 4.x ("legacy format") and 5.x. See the manual for details.
    
!!! warning
    This hardware revision is no longer available for sale! Consider using the improved HW Rev 2.x.

## Changelog
--------------------
### v6.1.0 - 09.12.2019
* added new parameters to increase accuracy: maxHDOP & maxDataAfterFix

### v6.0.3 - 02.08.2019
* fixed temperature sensor readout

### v6.0.2 - 22.07.2019
- inverted powerpin for telit module (hardware v2.1)
- added nmea prefix GN ( Glonass+GPS ) to the parser

### v5.0.5 - 09.01.2019
- Update LoRaWAN Stack
- Enable stepUp if needed by battery condition

### v5.0.4 - 15.11.2018
- update board driver

### v5.0.3 - 15.11.2018
- fix signed issue with longitude

### v5.0.0 - 26.10.2018
- Add option for Cayenne LLP Payload format
- Adjust payload format to support neagtive values
- Add Altitude
- Send GPS coordinates in Lat/Lon Format

- Update LoRaWAN Stack

### v4.0.9 - 05.10.2018
- Update LoRaWAN Stack

### v4.0.7 - 15.08.2018
- Fixed bugs with some LoRaWAN Network providers

### v4.0.6 - 28.05.2018
- Clear pending mems IRQ before sleep
- Updated internal state handling

### v4.0.4 - 20.03.2018
- Disable mems in active mode


### v4.0.3 - 20.03.2018
- compile against new board revision (lower power in sleep)
- disable external rtc 
- default tx power to 14dBm

### v4.0.2 - 23.02.2018
- Fix lost GPS messages due to broken CRC checks

### v4.0.1 - 29.01.2018
- Port to LoRa v3.2 Board

### v3.2
- Measure vBat when 3.3V Step Up is off
- Add config parameter gps timeout
- Add config parameter LoRaWAN ADR

### v3.1
- Enable buttons for sending (Reed contact connectors)
