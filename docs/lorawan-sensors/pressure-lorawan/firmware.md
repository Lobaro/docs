# Pressure Sensor LoRaWAN 
There exist different Variants of our Hardware with Pressure Probes, because there exist very 
different Pressure Probes for different situations. Please make sure to install the correct 
firmware for your Hardware. You can see which firmware your device is running, by checking the 
log with our config adapter, while the device is booting.

## Pressure Sensor [Lobaro Sensor]

### Firmware

**Downloads**:

* [app-lorawan-pressure-0.2.2.hex](firmware/app-lorawan-pressure-0.2.2.hex) [current release]

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
     
    Make sure to select the **"Firmware Updates"** checkbox!   

### Changelog

#### 0.2.2 - 2020-08-06
- Completely rewrite app structure (taken form `app-lorawan-environment`), to enable use 
  of LoRaWAN-features, like Downlink-Configs, Class-C

#### 0.1.0 - 2019-10-22
- configuration parameters for sensor limits

#### 0.0.3 - 2019-09-30
- initial release

----
## Pressure Sensor LoRaWAN [Keller Sensor]

### Firmware

* [app-keller-pressure-0.3.0.hex](firmware/app-keller-pressure-0.3.0.hex) [current release]
* [app-keller-pressure-0.2.0.hex](firmware/app-keller-pressure-0.2.0.hex)
* [app-keller-pressure-0.0.3.hex](firmware/app-keller-pressure-0.0.3.hex)

### Changelog

#### 0.3.0 - 2020-02-20
- Changed to use new Lobaro LoRaWAN stack with remote config.
- WARNING: default value for AppKey/JoinEUI changed!

#### 0.2.0 - 2019-07-22
- Added Battery Voltage to payload

#### 0.0.3 - 2019-02-14
- Reduce I2C speed from 10kHz to 1kHz for 10m cable

#### 0.0.2
- Update LoRaWAN Stack
- Reduce I2C speed from 100kHz to 10kHz

#### 0.0.1 
- initial version
