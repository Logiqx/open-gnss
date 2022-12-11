## VTG - Velocity and Track Made Good

### Summary

This is one of the 7 sentences commonly emitted by GPS / GNSS units.

The VTG sentence conveys the track made good / course over ground (COG) and the speed relative to the ground (SOG).

There are two forms of VTG which can be distinguished by field 2, which will be the fixed text 'T' in the newer form.



### Newer Structure

```
NMEA 2.3 onwards:
         1  2  3  4  5  6  7  8 9 10
         |  |  |  |  |  |  |  | | |
 $--VTG,x.x,T,x.x,M,x.x,N,x.x,K,m*hh<CR><LF>

Prior to NMEA 2.3:
         1  2  3  4  5  6  7  8 9
         |  |  |  |  |  |  |  | |
 $--VTG,x.x,T,x.x,M,x.x,N,x.x,K*hh<CR><LF>
```

| #    | Field          | Format      | Example | Description                                                  |
| ---- | -------------- | ----------- | ------- | ------------------------------------------------------------ |
| 0    | Sentence ID    | string      | $GPVTG  | [Talker ID](../lookups/talker-id.md) (GP) + message ID (VTG) |
| 1    | COG true       | numeric     | 220.86  | Course over ground in degrees (true), typically 2 dp         |
| 2    | COG true unit  | character   | T       | Fixed field. T = degrees true                                |
| 3    | COG mag        | numeric     | -       | Course over ground in degrees (magnetic). Null (empty) if unavailable. |
| 4    | COG mag unit   | character   | M       | Fixed field. M = degrees magnetic                            |
| 5    | SOG knots      | numeric     | 2.550   | Speed over ground in knots, typically 2 or 3 dp              |
| 6    | SOG knots unit | character   | N       | Fixed field. N = Knots                                       |
| 7    | SOG kph        | numeric     | 4.724   | Speed over ground in km/h, typically 2 or 3 dp               |
| 8    | SOG kph unit   | character   | K       | Fixed field. K = Kilometers per hour                         |
| 9    | Pos mode       | character   | A       | [Positioning mode](../lookups/pos-mode.md) indicator (NMEA 2.3 and later) |
| 10   | Checksum       | hexadecimal | \*7B    | Checksum                                                     |

Notes:

- The [positioning mode](../lookups/pos-mode.md) indicator (field 9) was added for the FAA in NMEA 2.3.



### Older Structure

```
The sentence was quite different in some older versions of NMEA:
         1  2  3   4  5
         |  |  |   |  |
 $--VTG,x.x,x,x.x,x.x*hh<CR><LF>
```

| #    | Field        | Format      | Example | Description                                                  |
| ---- | ------------ | ----------- | ------- | ------------------------------------------------------------ |
| 0    | Sentence IDs | string      | $GPVTG  | Talker ID (GP) + message ID (VTG)                            |
| 1    | COG true     | numeric     | 220.86  | Course over ground in degrees (true), typically 2 dp         |
| 2    | COG mag      | numeric     | -       | Course over ground in degrees (magnetic). Null (empty) if unavailable. |
| 3    | SOG knots    | numeric     | 2.550   | Speed over ground in knots, typically 2 or 3 dp              |
| 4    | SOG kph      | numeric     | 4.724   | Speed over ground in km/h, typically 2 or 3 dp               |
| 5    | Checksum     | hexadecimal | \*7B    | Checksum                                                     |



### Examples

#### NMEA Revealed

```
$GPVTG,220.86,T,,M,2.550,N,4.724,K,A*34
```

#### Locosys GT-31

```
$GPVTG,270.04,T,,M,0.04,N,0.1,K,A*09
```

