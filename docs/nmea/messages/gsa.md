## GSA - GNSS DOP and Active Satellites

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The GSA sentence includes GPS / GNSS receiver operating mode, satellites used for navigation, and DOP values.

In a multi-GNSS system the GSA message will be output multiple times, once for each GNSS.



### Structure

```
NMEA 4.10 onwards:
        1 2 3                        14 15  16  17 18 19
        | | |                         |  |   |   |  |  |
 $--GSA,a,a,x,x,x,x,x,x,x,x,x,x,x,x,x,x,x.x,x.x,x.x,x*hh<CR><LF>

Prior to NMEA 4.10:
        1 2 3                        14 15  16  17  18
        | | |                         |  |   |   |   |
 $--GSA,a,a,x,x,x,x,x,x,x,x,x,x,x,x,x,x,x.x,x.x,x.x*hh<CR><LF>
```

| #       | Field            | Format      | Example | Description                                                  |
| ------- | ---------------- | ----------- | ------- | ------------------------------------------------------------ |
| 0       | Sentence ID      | string      | $GPGSA  | [Talker ID](../lookups/talker-id.md) (GP) + message ID (GSA) |
| 1       | Op mode          | character   | A       | 2D / 3D operation mode; M = manual, A = automatic            |
| 2       | Fix mode         | digit       | 3       | [Fix mode](../lookups/fix-mode.md); 1 = no fix, 2 = 2D, 3 = 3D |
| -       | # Start of block | -           | -       | Start of repeated block (12 times)                           |
| 3 + idx | SV ID            | numeric     | 12      | Satellite ID or PRN number (leading zeros sent)              |
| -       | # End of block   | -           | -       | End of repeated block (12 times)                             |
| 15      | PDOP             | numeric     | 1.83    | Position dilution of precision (PDOP), typically 1 or 2 dp   |
| 16      | HDOP             | numeric     | 1.09    | Horizontal dilution of precision (HDOP), typically 1 or 2 dp |
| 17      | VDOP             | numeric     | 1.47    | Vertical dilution of precision (VDOP), typically 1 or 2 dp   |
| 18      | System ID        | hexadecimal | 1       | GNSS [system ID](../lookups/system-id.md) (NMEA 4.10 and later) |
| 19      | Checksum         | hexadecimal | \*7B    | Checksum                                                     |

Notes:

- If less than 12 SVs are used for navigation, the remaining fields are left empty.
- If more than 12 SVs are used for navigation, only the IDs of the first 12 are output.
- The GNSS [system ID](../lookups/system-id.md) (field 18) was added in NMEA 4.10.



### Examples

#### NMEA Revealed

```
$GNGSA,A,3,80,71,73,79,69,,,,,,,,1.83,1.09,1.47*17
```

#### Wikipedia

```
$GPGSA,A,3,10,07,05,02,29,04,08,13,,,,,1.72,1.03,1.38*0A
```

#### Locosys GT-31

```
$GPGSA,M,3,15,13,14,05,23,24,17,10,,,,,1.7,0.9,1.4*32
```

