# USonic LoRaWAN

## Firmware
**Downloads:**

* [app-lorawan-usonic-0.1.1.hex](firmware/app-lorawan-usonic-0.1.1.hex)

!!! hint "Firmware Release Notifications"
    We normally send e-mail notifications upon release of new firmware versions. To receive this mails you can sign up
    to the Lobaro newsletter here.
    
    [**Subscribe to our email newsletter here**](http://eepurl.com/gQYRbH){: target="_blank"} 
    
    Make sure to select the **"Firmware Updates"** checkbox!    

## Changelog 

**Firmware:** app-lorawan-usonic

v0.1.1
--------
- Tune for new sensor, still compatible with last one (V2)
- Allow to configure ADR
- Fix bug where TxPower config value was not applied
- Add usonicTest mode to permanently measure usonic values
- Usonic sensor measurement extended to 3.5m
- Implement more LoRaWAN Mac commands
- Fix bug where Firmware reset when receiving unknown MAC command