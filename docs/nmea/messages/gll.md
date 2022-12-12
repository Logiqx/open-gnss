## GLL - Geographic Position - Latitude / Longitude

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The GLL sentence includes latitude and longitude, with time of position fix and status.



### Structure

```
NMEA 2.3 onwards:
        1       2 3        4 5         6 7 8
        |       | |        | |         | | |
 $--GLL,ddmm.mm,a,dddmm.mm,a,hhmmss.ss,a,m*hh<CR><LF>

Prior to NMEA 2.3:
        1       2 3        4 5         6 7
        |       | |        | |         | |
 $--GLL,ddmm.mm,a,dddmm.mm,a,hhmmss.ss,a*hh<CR><LF>
```

| #    | Field       | Format      | Example    | Description                                                  |
| ---- | ----------- | ----------- | ---------- | ------------------------------------------------------------ |
| 0    | Sentence ID | string      | $GPGLL     | [Talker ID](../lookups/talker-id.md) (GP) + message ID (GLL) |
| 1    | Latitude    | ddmm.mmmm   | 5321.6802  | Latitude, typically 4 or 5 dp. Leading zeros are always included |
| 2    | NS          | character   | N          | Hemispherical orientation N or S (north or south)            |
| 3    | Longitude   | dddmm.mmmm  | 00630.3371 | Longitude, typically 4 or 5 dp. Leading zeros are always included |
| 4    | EW          | character   | W          | Hemispherical orientation E or W (east or west)              |
| 5    | Time        | hhmmss.sss  | 092751.000 | UTC time, typically 2 or 3 dp. Leading zeros are always included |
| 6    | Status      | character   | A          | Data status; A = active (data valid), V = void (data invalid) |
| 7    | Pos Mode    | character   | A          | [Positioning mode](../lookups/pos-mode.md) indicator (NMEA 2.3 and later) |
| 8    | Checksum    | hexadecimal | \*7B       | Checksum                                                     |

Notes:

- The [positioning mode](../lookups/pos-mode.md) indicator (field 7) was added for the FAA in NMEA 2.3.
- The [navigational status](../lookups/nav-status.md) indicator (not listed above) may have been added in NMEA 4.10. TBC




### Examples

#### NMEA Revealed

```
$GNGLL,4404.14012,N,12118.85993,W,001037.00,A,A*67
```

#### Locosys GT-31

```
$GPGLL,5637.8345,N,01638.4927,W,125901.000,A,A*48
```
