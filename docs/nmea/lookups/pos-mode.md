## NMEA - Positioning Mode

### Overview

The positioning mode indicator was added to several of the messages in NMEA 2.3 for Federal Aviation Administration (FAA).

It is usually a single character but some messages will contain multiple characters, one for each GNSS.

The order of the characters indicates the systems; GPS, GLONASS, Galileo, BeiDou, QZSS.



### Messages

The positioning mode indicator is used by the following messages:

| Message ID                | Description                                |
| ------------------------- | ------------------------------------------ |
| [GLL](../messages/gll.md) | Geographic Position - Latitude / Longitude |
| GNS                       |                                            |
| [RMC](../messages/rmc.md) | Recommended Minimum GNSS Data              |
| [VTG](../messages/vtg.md) | Velocity and Track Made Good               |



### Values

Positioning modes supported by NMEA:

| Positioning Mode | Description |
| ---- | ---- |
| A | Autonomous (non-differential) mode |
| C | Caution (Quectel Querk) |
| D | Differential (DGPS) mode |
| E | Estimated (dead-reckoning) mode |
| F | RTK float mode |
| M | Manual input mode |
| N | Data not valid. Either constellation not in use, or no valid fix |
| P | Precise (NMEA 4.10 and later).<br />No deliberate degradation (e.g. SA), and using hi-res fix (e.g. P-code).<br />Also signifies multi-frequency, SBAS or Precise Point Positioning (PPP). |
| R | RTK integer mode, or using "course position" from almanac (SiRF) <sup>1</sup> |
| S |  Simulator mode |
| U | Unsafe (Quectel Querk) |

Notes:

<sup>1</sup> - SiRF - position was calculated based on one or more of the SVs having their states derived from almanac parameters, as opposed to ephemerides.
