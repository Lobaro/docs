# EDL21 Electricity meter LoRaWAN Bridge

## Firmware

**Downloads:** 

!!! warning "WARNING"
    When upgrading from Versions <0.4.1 to 0.4.1 or higher: 
    Due to changes in the generation of the LoRaWAN parameters the default values for `JoinEUI` and `AppKey` will change if you 
    use the `Restore Default` function in the `Lobaro Maintenance Tool` in order to reset the configuration.


* [app-edl21-opto-0.4.1+LoRa.hex](firmware/app-edl21-opto-0.4.1+LoRa.hex) New Opto-Head Version (round) [current release] 

* [app-edl21-opto-0.2.0+LoRa.hex](firmware/app-edl21-opto-0.2.0+LoRa.hex) Old Opto-Head Version (square) 

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
    
    Make sure to select the **"Firmware Updates"** checkbox!    


## Changelog

**Firmware:** app-edl21-opto

### v0.4.1 [current release]
* Fixed remote append
* WARNING: Due to changes in the generation of the LoRa paramters the values for JoinEUI and AppKey will change if you 
use the `Restore Default` function in the Lobaro Maintenance Tool in order to reset the configuration.

### v0.4.0
* Added remote config via downlink

### v0.3.2

* Added initial readout before OTAA join to check optical connection

### v0.3.1

* Initial release for new round Opto-Head


**Legacy Downloads:**

* [app-edl21-opto-0.3.2+LoRa.hex](firmware/app-edl21-opto-0.3.2+LoRa.hex) New Opto-Head Version (round) 

* [app-edl21-opto-0.3.1+LoRa.hex](firmware/app-edl21-opto-0.3.1+LoRa.hex) New Opto-Head Version (round)  