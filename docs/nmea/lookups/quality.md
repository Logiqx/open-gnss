## NMEA - Quality

### Overview

The quality indicator has always been present in [GGA](../messages/gga.md) messages.

The [positioning mode](pos-mode.md) indicator is somewhat similar, but was only added in NMEA 2.3 for the Federal Aviation Administration (FAA).



### Values

Quality indicators above 2 are features of NMEA  2.3, or newer:

| Quality | Description |
| ---- | ---- |
| 0 | Fix not available |
| 1 | GPS fix |
| 2 | Differential GPS fix |
| 3 | PPS fix |
| 4 | Real-time Kinematics (RTK) |
| 5 | RTK float |
| 6 | Estimated (dead-reckoning) |
| 7 | Manual input |
| 8 | Simulation |