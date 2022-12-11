## ZDA - Time and Date

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The ZDA sentence contains the UTC day, month, year and local time zone.



### Structure

```
        1         2  3  4    5  6  7
        |         |  |  |    |  |  |
 $--ZDA,hhmmss.ss,xx,xx,xxxx,xx,xx*hh<CR><LF>
```



| #    | Field       | Format      | Example    | Description                                                  |
| ---- | ----------- | ----------- | ---------- | ------------------------------------------------------------ |
| 0    | Sentence ID | string      | $GPZDA     | [Talker ID](../lookups/talker-id.md) (GP) + message ID (ZDA) |
| 1    | Time        | hhmmss.sss  | 092751.000 | UTC time, typically 2 or 3 dp. Leading zeros are always included |
| 2    | Day         | dd          | 11         | UTC day of the month; 01-31. Leading zeros are always included |
| 3    | Month       | mm          | 03         | UTC month of the year; 01-12. Leading zeros are always included |
| 4    | Year        | yyyy        | 2004       | UTC year                                                     |
| 5    | Zone hours  | [-]HH       | -1         | Local time zone (hours); -13 to +13 hours                    |
| 6    | Zone mins   | MM          | 00         | Local time zone (minutes); 00 to 59. Apply same sign as hours |
| 7    | Checksum    | hexadecimal | \*7B       | Checksum                                                     |

Notes:

- The zone hours and mins is often fixed at 00 since GPS / GNSS receivers do not typically know the local time zone.




### Examples

#### NMEA Revealed

```
$GPZDA,160012.71,11,03,2004,-1,00*7D
```

#### Locosys GT-31

```
$GPZDA,125900.000,10,12,2022,,*59
```

