## GSV - GNSS Satellites in View

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The GSV sentences include the SV ID, elevation, azimuth, and signal strength (C/No) values for up to 4 satellites.

In a multi-GNSS system, sets of the GSV message will be output multiple times, one set for each GNSS.



### Structure

```
NMEA 4.10 onwards:
        1 2 3 4 5 6 7    n-1 n
        | | | | | | |     |  |
 $--GSV,x,x,x,x,x,x,x,...,x*hh<CR><LF>
 
Prior to NMEA 4.10:
        1 2 3 4 5 6 7     n
        | | | | | | |     |
 $--GSV,x,x,x,x,x,x,x,...*hh<CR><LF>
```

| #            | Field            | Format      | Example | Description                                                  |
| ------------ | ---------------- | ----------- | ------- | ------------------------------------------------------------ |
| 0            | Sentence ID      | string      | $GPGSV  | [Talker ID](../lookups/talker-id.md) (GP) + message ID (GSV) |
| 1            | Num messages     | character   | 3       | Total number of GSV messages being output; 1 to 9            |
| 2            | Message num      | digit       | 1       | Message number; 1 to num messages                            |
| 3            | Num SV           | numeric     | 09      | Number of space vehicles (satellites) in view for the talker ID and signal ID |
| -            | # Start of block | -           | -       | Start of repeated block (N times)                            |
| 4 + 4 \* idx | SV ID            | numeric     | 15      | Satellite ID or PRN number (leading zeros sent)              |
| 5 + 4 \* idx | Elevation        | numeric     | 80      | Elevation in degrees (leading zeros sent); 00 to 90          |
| 6 + 4 \* idx | Azimuth          | numeric     | 234     | Azimuth in degrees to true north (leading zeros sent); 000 to 359 |
| 7 + 4 \* idx | C/N<sub>0</sub>  | numeric     | 47      | Carrier-to-noise density ratio in db-Hz; range 00-99 or null (empty) when not tracking |
| -            | # End of block   | -           | -       | End of repeated block (N times)                              |
| n-1          | Signal ID        | hexadecimal | 1       | GNSS [signal ID](../lookups/signal-id.md) (NMEA 4.10 and later) |
| n            | Checksum         | hexadecimal | \*7B    | Checksum                                                     |

Notes:

- Receivers may emit more than 12 quadruples (more than three GSV sentences), even though NMEA-0813 doesn’t allow this.
- The final sentence may include data for less than 4 satellites, if the number of visible satellites is not divisible by 4.
- GSV sentences include carrier-to-noise density ratio (C/N<sub>0</sub>) for each SV in dB-Hz. See related [article](https://insidegnss.com/measuring-gnss-signal-strength/) for further information.
- The GNSS [signal ID](../lookups/signal-id.md) was added in NMEA 4.10.



### Examples

#### NMEA Revealed

```
$GPGSV,3,1,11,03,03,111,00,04,15,270,00,06,01,010,00,13,06,292,00*74 $GPGSV,3,2,11,14,25,170,00,16,57,208,39,18,67,296,40,19,40,246,00*74
$GPGSV,3,3,11,22,42,067,42,24,14,311,43,27,05,244,00,,,,*4D
```

#### Wikipedia

```
$GPGSV,3,1,11,10,63,137,17,07,61,098,15,05,59,290,20,08,54,157,30*70
$GPGSV,3,2,11,02,39,223,16,13,28,070,17,26,23,252,,04,14,186,15*77
$GPGSV,3,3,11,29,09,301,24,16,09,020,,36,,,*76
```

#### Locosys GT-31

```
$GPGSV,3,1,11,15,80,234,47,13,62,116,46,24,40,261,44,14,39,058,44*76
$GPGSV,3,2,11,23,33,303,43,05,19,190,33,17,16,096,,30,13,073,17*76
$GPGSV,3,3,11,10,10,327,,19,03,127,,18,02,268,*47
```

