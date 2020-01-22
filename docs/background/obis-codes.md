#OBIS-Codes

Object identification system (OBIS) Codes are used to identify the different readings of a smart meter transferred in 
Smart Message Language (SML). 
They are described in the international standard IEC 62056-61.



## Structure
Every code consists of 6 separated group sub-identifiers. In general some of these identifiers may be omitted but our products
require the full code.
The basic pattern is:
  
__A-B:C.D.E*F__

| group   | description | examples |
| ------- |---------- | ----------- |
| __A__ |  medium   | 1 = electricity, 8 = water |
| __B__ |  channel | 0 = no channel available |
| __C__ |  physical unit, depends on __A__ | power, current, voltage...|
| __D__ |  measurement type, depends on __A__ and __C__ | maximum, current value, energy...|
| __E__ |  tariff | 0 = total, 1 = tariff 1, 2 = tariff 2 ...|
| __F__ |  separate values defined by __A__-__E__ | billing periods, __255__ if not used |


## Examples 
|    |   |
| ------- |---------- |
| `1-0:1.8.0*255` |  Positive active energy (A+) total [kWh]  |
| `1-0:3.8.1*255` |  Positive reactive energy (Q+) in tariff T1 [kvarh] |
