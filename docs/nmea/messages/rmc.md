## RMC - Recommended Minimum GNSS Data

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The RMC sentence contains the time, date, position, course and speed data provided by the GPS / GNSS receiver.



### Structure

```
NMEA 4.10 onwards:
        1         2 3       4 5        6  7   8   9  10 11 12 13 14
        |         | |       | |        |  |   |   |   |  |  | |  |
 $--RMC,hhmmss.ss,A,ddmm.mm,a,dddmm.mm,a,x.x,x.x,xxxx,x.x,a,m,s*hh<CR><LF>

NMEA 2.3 onwards, prior to NMEA 4.10:
        1         2 3       4 5        6  7   8   9   10 11 12 13
        |         | |       | |        |  |   |   |    |  | |  |
 $--RMC,hhmmss.ss,A,ddmm.mm,a,dddmm.mm,a,x.x,x.x,xxxx,x.x,a,m*hh<CR><LF>
 
Prior to NMEA 2.3:
        1         2 3       4 5        6  7   8   9    10 11 12
        |         | |       | |        |  |   |   |    |  |  |
 $--RMC,hhmmss.ss,A,ddmm.mm,a,dddmm.mm,a,x.x,x.x,xxxx,x.x,a*hh<CR><LF>

```

| #    | Field       | Format      | Example    | Description                                                  |
| ---- | ----------- | ----------- | ---------- | ------------------------------------------------------------ |
| 0    | Sentence ID | string      | $GPRMC     | [Talker ID](../lookups/talker-id.md) (GP) + message ID (RMC) |
| 1    | Time        | hhmmss.sss  | 092751.000 | UTC time, typically 2 or 3 dp. Leading zeros are always included |
| 2    | Status      | character   | A          | Data status; A = active (data valid), V = void (receiver warning) |
| 3    | Latitude    | ddmm.mmmm   | 5321.6802  | Latitude, typically 4 or 5 dp. Leading zeros are always included |
| 4    | NS          | character   | N          | Hemispherical orientation N or S (north or south)            |
| 5    | Longitude   | dddmm.mmmm  | 00630.3371 | Longitude, typically 4 or 5 dp. Leading zeros are always included |
| 6    | EW          | character   | W          | Hemispherical orientation E or W (east or west)              |
| 7    | SOG         | numeric     | 0.146      | Speed over ground in knots, typically 2 or 3 dp              |
| 8    | COG         | numeric     | 77.52      | Course over ground in degrees true, typically 2 dp              |
| 9    | Date        | ddmmyy      | 100117     | Date, ddmmyy                                                 |
| 10   | Mag Var     | numeric     | -          | Magnetic Variation in degrees. Null (empty) if unavailable.  |
| 11   | Mag Var E/W | character   | -          | Magnetic Variation, E or W (East or West). Null (empty) if unavailable. |
| 12   | Pos Mode    | character   | A          | [Positioning mode](../lookups/pos-mode.md) indicator (NMEA 2.3 and later) |
| 13   | Nav Status  | character   | V          | [Navigational status](../lookups/nav-status.md) indicator (NMEA 4.10 and later) |
| 14   | Checksum    | hexadecimal | \*7B       | Checksum                                                     |

Notes:

- The [positioning mode](../lookups/pos-mode.md) indicator (field 12) was added for the FAA in NMEA 2.3.
- The [navigational status](../lookups/nav-status.md) indicator (field 13) was added in NMEA 4.10.



### Examples

#### NMEA Revealed

```
$GNRMC,001031.00,A,4404.13993,N,12118.86023,W,0.146,,100117,,,A*7B
```

#### Wikipedia

```
$GPRMC,092750.000,A,5321.6802,N,00630.3372,W,0.02,31.66,280511,,,A*43
```

#### Locosys GT-31

```
$GPRMC,125900.000,A,5637.8345,N,01638.4927,W,0.04,270.04,101222,,,A*7F
```

