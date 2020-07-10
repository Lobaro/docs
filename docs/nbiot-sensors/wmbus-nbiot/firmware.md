## Firmware
Firmware can be installed on the wMBus NB-IoT Gateway using the Lobaro Tool and Config Adapter. Please 
refer to [Updating Firmware](/tools/lobaro-tool.html#updating-firmware) for instructions.

!!! caution "Select correct Hardware revisions"
    Due to continuous development there exist two main hardware revisions of the Lobaro wMBUS over LoRaWAN bridge hardware.
    **Please use the images below to select the correct firmware file for your given hardware.**
    
### Hardware Revision 2
* [app-nrf9160-wmbus-TZ2-0.5.4+hw2-mcuboot-slot0-boot.hex](firmware/app-nrf9160-wmbus-TZ2-0.5.4+hw2-mcuboot-slot0-boot.hex) [current release]

### Hardware Revision 1
* [app-nrf9160-wmbus-TZ2-0.5.4+hw1-mcuboot-slot0-boot.hex](firmware/app-nrf9160-wmbus-TZ2-0.5.4+hw1-mcuboot-slot0-boot.hex) [current release]
* [app-nrf9160-wmbus-TZ2-0.4.0-mcuboot-slot0-boot.hex](firmware/app-nrf9160-wmbus-TZ2-0.4.0-mcuboot-slot0-boot.hex)



## Changelog

### 0.5.4 - 2020-07-08
#### Added
- Support new dedicated hardware HW2
- Reboot when losing connection to network (in case modem is stuck)
- Add optional upload per raw UDP (without coap) to alternate address
#### Changed
- Support only LTE-M/NB-IoT in firmware as supported by current hardware
- Increase config size, up to 175 device IDs
- Disable modem on initial wMBus collection to enhance reception quality
- Start frame counter at 1, not at 0

### 0.4.0 - 2020-05-15
#### Added
- Start changelog.
#### Fixed
- Make Firmware work with nrf-Board 1.4 (fix power-on logic for module).
#### Changed
- Removed learning mode.
