
The connection to the LoRaWAN network is defined by multiple configuration parameters.
This need to be set according to your LoRaWAN network and the way your device is 
supposed to be attached to it, or the device will not be able to send any data.

There are two different methods to attach a device to a LoRaWAN network: 
Over-the-air-activation (OTAA) and Activation-by-personalisation (ABP). 
Depending on which method you are using you will have to set different values.

Several values are a number of bytes, that need to be entered as hexstrings (without 
`0x`-prefix). So e.g. the DevEUI is a value of 8 bytes encoded in hex will be 
16 hexdigits long. A sample value would be `0123456789abcdef`.

| name | used | type | description |
|------|------|------|-------------|
| `OTAA` | both | bool | `true` &equiv; use OTAA, `false` &equiv; use ABP |
| `DevEUI` | OTAA | hexbyte[8] | the 8 byte DevEUI identifies the hardware on the join operation. The default value is a world wide unique value predefined in the hardware. Should not be changed unless required by the network provider.
| `AppEUI` | OTAA | hexbyte[8] | ID defining the application used in the LoRaWAN network. |
| `AppKey` | OTAA | hexbyte[16] | Key used to encrypt communication with the LoRaWAN network. |
| `AppSKey` | ABP | hexbyte[16] | Application Session Key to be synced with the LoRaWAN network. |  
| `NetSKey` | ABP | hexbyte[16] | Network Session Key to be synced with the LoRaWAN network. |  
| `DevAdr` | ABP | hexbyte[4] | Device Address used to identify device in the LoRaWAN network. |  
| `SF` | both | int | Initial LoRa spreading factor used for transmissions. Valid range is 7-12. The actual spreading factor used by change during operation if ADR is used. |
| `ADR` | both | bool | Should adaptive data rate be used? `true` &equiv; use ADR, `false` &equiv; don't | 
