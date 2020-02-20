# Pressure Sensor LoRaWAN [Lobaro Sensor]

## Firmware

**Downloads**:

* [app-lobaro-pressure-0.1.0.hex](firmware/app-lobaro-pressure-0.1.0+LoRa.hex) [current release]
* [app-lobaro-pressure-0.0.3.hex](firmware/app-lobaro-pressure-0.0.3+LoRa.hex) 

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
     
    Make sure to select the **"Firmware Updates"** checkbox!   

## Changelog

### [0.1.0] (22.10.2019) [current release]

**Added**

 - configuration parameters for sensor limits

### [0.0.3] (30.09.2019)

- initial release

----
#Pressure Sensor LoRaWAN [Keller Sensor]

## Firmware

* [app-keller-pressure-TZ0-0.3.0.hex](firmware/app-keller-pressure-TZ0-0.3.0.hex) [current release]
* [app-keller-pressure-0.2.0.hex](firmware/app-keller-pressure-0.2.0.hex)
* [app-keller-pressure-0.0.3.hex](firmware/app-keller-pressure-0.0.3.hex)

## Changelog

### v0.3.0 (20.02.2020)
- Changed to use new Lobaro LoRaWAN stack with remote config.
- WARNING: default value for AppKey/JoinEUI changed!

### v0.2.0 (22.07.2019)
- Added Battery Voltage to payload

### v0.0.3 (14.02.2019)
- Reduce I2C speed from 10kHz to 1kHz for 10m cable

### v0.0.2
- Update LoRaWAN Stack
- Reduce I2C speed from 100kHz to 10kHz

### v0.0.1 
- inital version
