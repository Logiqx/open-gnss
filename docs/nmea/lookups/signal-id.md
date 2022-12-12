## NMEA - Signal Identifier

### Overview

NMEA 4.10 added the signal identifier (aka "signal ID") to distinguish between different signals from GNSS.

Signal identifiers are only valid when combined with a GNSS [system identifier](system-id.md).



### Messages

Signal ID was added to the following messages in NMEA 4.10:

| Message ID                | Description             |
| ------------------------- | ----------------------- |
| GBS                       |                         |
| GRS                       |                         |
| [GSV](../messages/gsv.md) | GNSS Satellites in View |



### Possible Values

A decent list of signal identifiers can be found in NMEA Revealed - [NMEA 4.11 System ID and Signal ID](https://gpsd.gitlab.io/gpsd/NMEA.html#_nmea_4_11_system_id_and_signal_id).

