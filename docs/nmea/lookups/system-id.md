## NMEA - System Identifier

### Overview

The GNSS system identifier (aka "system ID") was added to several messages in NMEA 4.10.



### Messages

System ID was added to the following messages in NMEA 4.10:

| Message ID                | Description                    |
| ------------------------- | ------------------------------ |
| GBS                       |                                |
| GRS                       |                                |
| [GSA](../messages/gsa.md) | GNSS DOP and Active Satellites |



### Values

GNSS system IDs reported in NMEA 4.10 (and newer):

| System ID | GNSS                  | Owner                      | Coverage                |
| --------- | --------------------- | -------------------------- | ----------------------- |
| 1         | GPS                   | U.S. Government            | Global                  |
| 2         | GLONASS               | Russian Federation         | Global                  |
| 3         | Galileo               | European Union             | Global                  |
| 4         | BeiDou                | People's Republic of China | Global                  |
| 5         | QZSS                  | Government of Japan        | Regional - Asia-Pacific |
| 6         | NavIC, formerly IRNSS | Government of India        | Regional - India        |
