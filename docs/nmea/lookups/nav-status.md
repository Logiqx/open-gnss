## NMEA - Navigational Status

### Overview

The navigational status indicator was added to several messages in NMEA 4.10.



### Messages

Navigational status was added to the following messages in NMEA 4.10:

| Message ID                            | Description                                |
| ------------------------------------- | ------------------------------------------ |
| [GLL](../messages/gll.md)<sup>1</sup> | Geographic Position - Latitude / Longitude |
| GNS                                   |                                            |
| [RMC](../messages/rmc.md)             | Recommended Minimum GNSS Data              |

Notes:

- GLL<sup>1</sup> - The addition of the navigational status indicator to GLL is still to be confirmed.



### Possible Values

| Navigational Status | Description      |
| ------------------- | ---------------- |
| C                   | Caution          |
| S                   | Safe             |
| U                   | Unsafe           |
| V                   | Void (not valid) |
