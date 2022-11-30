## Open GPS - Existing Formats

This document contains some details about existing file formats and their contents.

It is copy / pasted from the [gps-wizard](https://logiqx.github.io/gps-wizard/) documentation and as such it defines the formats in the standardised types of the Python project.

No changes have been made to the text so bear that in mind when reading through the list of fields.




### FIT

| FIT Name                       | Description         | Name |  Type   | Units |     Resolution      |
| ------------------------------ | ------------------- | :--: | :-----: | :---: | :-----------------: |
| timestamp                      | Date + Time         |  ts  | float64 |   s   |          -          |
| position_lat <sup>1</sup>      | Latitude            | lat  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| position_long <sup>1</sup>     | Longitude           | lon  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| distance                       | Cumulative Distance | dist | float64 |   m   | 1 x 10<sup>-3</sup> |
| altitude <sup>2 3</sup>        | Altitude            |  -   |    -    |   -   |          -          |
| enhanced_altitude <sup>3</sup> | Altitude            | ele  | float64 |   m   | 2 x 10<sup>-1</sup> |
| speed <sup>2 4</sup>           | Speed               |  -   |    -    |   -   |          -          |
| enhanced_speed <sup>4</sup>    | Speed               | sog  | float32 |  m/s  | 1 x 10<sup>-3</sup> |
| vertical_speed <sup>5</sup>    |                     | roc  | float32 |  m/s  | 1 x 10<sup>-3</sup> |
| cog <sup>6 7</sup>             | Course Over Ground  | cog  | float32 |  deg  | 1 x 10<sup>-3</sup> |
| Sat <sup>6</sup>               | Satellites          | sat  |  uint8  |   -   |          -          |
| hdop <sup>6 8</sup>            | HDOP                | hdop | float32 |   -   | 1 x 10<sup>-1</sup> |
| heart_rate                     | Heart Rate          |  hr  | uint16  |  bpm  |          -          |

Notes:

1. Latitude + longitude are converted to degrees and rounded to 7 decimal places; see section below.
2. Altitude and speed are unused because enhanced altitude and speed are better; see section below.
3. Altitude and enhanced altitude are recorded to the nearest 0.2m.
4. Speed and enhanced speed are recorded in mm/s, providing up to 3 decimal places for m/s.
5. Vertical speed is only present in Suunto FIT files.
6. COG, satellites and HDOP are only available in COROS FIT files.
7. COG is currently an integer in COROS FIT files. Decimal places have been requested.
8. HDOP provides exactly 1 decimal place in COROS FIT files.



### GPX

| GPX Name                      | Description        | Name |  Type   | Units |     Resolution      |
| ----------------------------- | ------------------ | :--: | :-----: | :---: | :-----------------: |
| lat <sup>1</sup>              | Latitude           | lat  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| lon <sup>1</sup>              | Longitude          | lon  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| ele <sup>2</sup>              | Elevation          | ele  | float64 |   m   | 1 x 10<sup>-3</sup> |
| time                          | Date + Time        |  ts  | float64 |   s   | 1 x 10<sup>-3</sup> |
| course / cog <sup>3 4 5</sup> | Course Over Ground | cog  | float32 |  deg  | 1 x 10<sup>-3</sup> |
| speed <sup>3 6</sup>          | Speed Over Ground  | sog  | float32 |  m/s  | 1 x 10<sup>-3</sup> |
| sat                           | Satellites         | sat  |  uint8  |   -   |          -          |
| hdop <sup>7</sup>             | HDOP               | hdop | float32 |   -   | 1 x 10<sup>-2</sup> |
| hr / heart_rate <sup>8</sup>  | Heart Rate         |  hr  | uint16  |  bpm  |          -          |

Notes:

1. Latitude and longitude are rounded to 7 decimal places by the GPX reader; see comments below.
2. Elevation does not have a fixed precision but will rarely be more than 3 decimal places.
3. Course over ground and speed over ground are only supported natively by GPX 1.0; see comments below.
4. Course over ground is automatically rounded to 3 decimal places by the GPX reader.
5. Course over ground is incorrectly named in COROS files and GPX exports from GPSResults; "cog" instead of "course".
6. Speed over ground does not have a fixed precision but will rarely be more than 3 decimal places.
7. HDOP does not have a fixed precision but will rarely be more than 2 decimal places.
8. Heart rate is supported by the Garmin TrackPointExtension schema and ClueTrust GPXData schema.



### NMEA

#### RMC - Recommended Minimum Navigation Information

The RMC message contains the core data; latitude, longitude, speed and course.

| NMEA Name                           | Raw Name  | Type | Name |  Type   | Units |     Resolution      |
| ----------------------------------- | --------- | :--: | :--: | :-----: | :---: | :-----------------: |
| UTC                                 | hhmmss    |  f8  |  ts  | float64 |   s   | 1 x 10<sup>-3</sup> |
| Status                              | status    |  U1  |  -   |    -    |   -   |          -          |
| Latitude <sup>1</sup>               | lat       |  f8  | lat  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| N or S                              | ns        |  U1  |  -   |    -    |   -   |          -          |
| Longitude <sup>1</sup>              | lon       |  f8  | lon  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| E or W                              | ew        |  U1  |  -   |    -    |   -   |          -          |
| Speed over Ground <sup>2</sup>      | sog       |  f4  | sog  | float32 |  m/s  | 1 x 10<sup>-3</sup> |
| Track Made Good (True) <sup>3</sup> | cog       |  f4  | cog  | float32 |  deg  | 1 x 10<sup>-3</sup> |
| Date <sup>4</sup>                   | ddmmyy    |  u4  |  ts  | float64 |   s   | 1 x 10<sup>-3</sup> |
| Magnetic Variation                  | magVar    |  f4  |  -   |    -    |   -   |          -          |
| E or W                              | magVarEw  |  U1  |  -   |    -    |   -   |          -          |
| FAA Mode Indicator <sup>5</sup>     | faaMode   |  U1  |  -   |    -    |   -   |          -          |
| Nav Status <sup>6</sup>             | navStatus |  U1  |  -   |    -    |   -   |          -          |

Notes:

1. Latitude and longitude is provided in degrees and minutes in the NMEA format. The internal conversion to degrees is rounded to 7 decimal places.
2. Speed over ground is provided in knots in the NMEA format. The internal conversion to m/s is rounded to 3 decimal places.
3. Course over ground / track made good is automatically rounded to 3 decimal places.
4. Two digit years are since 1 Jan 1980.
5. FAA mode indicator is NMEA 2.3 and later.
6. Nav status is NMEA 4.1 and later.



#### GGA - Global Positioning System Fix Data

The first few fields are unused because they are also present in the corresponding RMC message.

| NMEA Name                                     | Raw Name   | Type | Name |  Type   | Units |     Resolution      |
| --------------------------------------------- | ---------- | :--: | :--: | :-----: | :---: | :-----------------: |
| UTC                                           | -          |  -   |  -   |    -    |   -   |          -          |
| Latitude                                      | -          |  -   |  -   |    -    |   -   |          -          |
| N or S                                        | -          |  -   |  -   |    -    |   -   |          -          |
| Longitude                                     | -          |  -   |  -   |    -    |   -   |          -          |
| E or W                                        | -          |  -   |  -   |    -    |   -   |          -          |
| GPS Quality Indicator                         | quality    |  u1  |  -   |    -    |   -   |          -          |
| Number of Satellites                          | numSv      |  u1  | sat  |  uint8  |   -   |          -          |
| Horizontal Dilution of Precision <sup>1</sup> | hdop       |  f4  | hdop | float32 |   -   | 1 x 10<sup>-2</sup> |
| Altitude from MSL <sup>2</sup>                | alt        |  f8  | ele  | float64 |   m   | 1 x 10<sup>-3</sup> |
| Units of Altitude                             | altUnit    |  U1  |  -   |    -    |   -   |          -          |
| Geoidal Separation                            | geoSep     |  f4  |  -   |    -    |   -   |          -          |
| Units of Geoidal Separation                   | geoSepUnit |  U1  |  -   |    -    |   -   |          -          |
| Age of differential GPS data                  | dgpsAge    |  u2  |  -   |    -    |   -   |          -          |
| Differential reference station ID             | dgpsId     |  u2  |  -   |    -    |   -   |          -          |

Notes:

1. HDOP does not have a fixed precision but will rarely be more than 2 decimal places.
2. Altitude does not have a fixed precision but will rarely be more than 3 decimal places.



#### Latitude and Longitude in NMEA

GPS / GNSS chips outputting NMEA data typically provide a maximum precision of ddmm.mmmmm for latitude and dddmm.mmmmm for longitude. Since the mm before the decimal point is whole minutes (00 - 60), overall resolution is only 60% of the SiRF and ublox binary formats which provide 7 decimal digits.



### SBP

| SiRF Name                            | Raw Name            | Type | Name  |  Type   | Units |     Resolution      |
| ------------------------------------ | ------------------- | :--: | :---: | :-----: | :---: | :-----------------: |
| HDOP                                 | hdop                |  u1  | hdop  | float32 |   -   | 2 x 10<sup>-1</sup> |
| Number of SVs in Fix                 | sv_count            |  u1  |  sat  |  uint8  |   -   |          -          |
| UTC Milliseconds                     | utc_millisecs       | <u2  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Packed Datetime                  | utc_packed_datetime | <u4  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| Satellite ID List                    | sv_ids              | <u4  | svids | uint32  |   -   |          -          |
| Latitude                             | latitude            | <i4  |  lat  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| Longitude                            | longitude           | <i4  |  lon  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| Altitude from MSL                    | altitude_msl        | <i4  |  ele  | float64 |   m   | 1 x 10<sup>-2</sup> |
| Speed Over Ground (SOG)              | sog                 | <u2  |  sog  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Course Over Ground (COG, True)       | cog                 | <u2  |  cog  | float32 |  deg  | 1 x 10<sup>-2</sup> |
| Climb Rate                           | climb_rate          | <i2  |  roc  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Speed Dilution of Precision          | sdop                |  u1  | sdop  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Vertical Speed Dilution of Precision | vsdop               |  u1  | vsdop | float32 |  m/s  | 1 x 10<sup>-2</sup> |



### SBN

The SiRF binary format consists of 91 to 97 byte messages which mainly originate from the Geodetic Navigation Data.

The first 91 bytes are standard SiRF and remaining bytes are specific to Locosys; unfiltered SOG + COG, SDOP and VSDOP.

SiRFDrive fields are always set to zero on the GT-11 and GT-31, so they can safely be ignored.

| SiRF Name                                         | Raw Name             | Type | Name  |  Type   | Units |     Resolution      |
| ------------------------------------------------- | -------------------- | :--: | :---: | :-----: | :---: | :-----------------: |
| Message ID <sup>1</sup>                           | message_id           |  u1  |   -   |    -    |   -   |          -          |
| Nav Valid <sup>2</sup>                            | nav_valid            | >u2  |   -   |    -    |   -   |          -          |
| Nav Type <sup>3</sup>                             | nav_type             | >u2  |   -   |    -    |   -   |          -          |
| Extended Week Number                              | extended_week_no     | >u2  |   -   |    -    |   -   |          -          |
| TOW                                               | time_of_week         | >u4  |   -   |    -    |   -   |          -          |
| UTC Year                                          | utc_year             | >u2  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Month                                         | utc_month            |  u1  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Day                                           | utc_day              |  u1  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Hour                                          | utc_hour             |  u1  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Minute                                        | utc_minute           |  u1  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| UTC Second                                        | utc_millisecs        | >u2  |  ts   | float64 |   s   | 1 x 10<sup>-3</sup> |
| Satellite ID List                                 | sv_ids               | >u4  | svids | uint32  |   -   |          -          |
| Latitude                                          | latitude             | >i4  |  lat  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| Longitude                                         | longitude            | >i4  |  lon  | float64 |  deg  | 1 x 10<sup>-7</sup> |
| Altitude from Ellipsoid                           | altitude_ellipsoid   | >i4  |   -   |    -    |   -   |          -          |
| Altitude from MSL                                 | altitude_msl         | >i4  |  ele  | float64 |   m   | 1 x 10<sup>-2</sup> |
| Map Datum <sup>4</sup>                            | map_datum            |  u1  |   -   |    -    |   -   |          -          |
| Speed Over Ground (SOG)                           | sog                  | >u2  |  sog  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Course Over Ground (COG, True)                    | cog                  | >u2  |  cog  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Magnetic Variation <sup>5</sup>                   | magnetic_variation   | >i2  |   -   |    -    |   -   |          -          |
| Climb Rate                                        | climb_rate           | >i2  |  roc  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Heading Rate - SiRFDrive                          | heading_rate         | >i2  |   -   |    -    |   -   |          -          |
| Estimated Horizontal Position Error               | ehpe                 | >u4  | ehpe  | float32 |   m   | 1 x 10<sup>-2</sup> |
| Estimated Vertical Position Error                 | evpe                 | >u4  | evpe  | float32 |   m   | 1 x 10<sup>-2</sup> |
| Estimated Time Error - SiRFDrive                  | ete                  | >u4  |   -   |    -    |   -   |          -          |
| Estimated Horizontal Velocity Error - SiRFDrive   | ehve                 | >u2  |   -   |    -    |   -   |          -          |
| Clock Bias <sup>6</sup>                           | clock_bias           | >i4  |   -   |    -    |   -   |          -          |
| Clock Bias Error - SiRFDrive                      | clock_bias_error     | >u4  |   -   |    -    |   -   |          -          |
| Clock Drift <sup>6</sup>                          | clock_drift          | >i4  |   -   |    -    |   -   |          -          |
| Clock Drift Error - SiRFDrive                     | clock_drift_error    | >u4  |   -   |    -    |   -   |          -          |
| Distance - SiRFDrive                              | distance             | >u4  |   -   |    -    |   -   |          -          |
| Distance Error - SiRFDrive                        | distance_error       | >u2  |   -   |    -    |   -   |          -          |
| Heading Error - SiRFDrive                         | heading_error        | >u2  |   -   |    -    |   -   |          -          |
| Number of SVs in Fix                              | sv_count             |  u1  |  sat  |  uint8  |   -   |          -          |
| HDOP                                              | hdop                 |  u1  | hdop  | float32 |   -   | 2 x 10<sup>-1</sup> |
| Additional Mode Info <sup>7</sup>                 | additional_mode_info |  u1  |   -   |    -    |   -   |          -          |
| Unfiltered Speed Over Ground <sup>8</sup>         | unfiltered_sog       | >u2  | sogu  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Unfiltered Course Over Ground <sup>8</sup>        | unfiltered_cog       | >u2  | cogu  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Speed Dilution of Precision <sup>9</sup>          | sdop                 |  u1  | sdop  | float32 |  m/s  | 1 x 10<sup>-2</sup> |
| Vertical Speed Dilution of Precision <sup>9</sup> | vsdop                |  u1  | vsdop | float32 |  m/s  | 1 x 10<sup>-2</sup> |

Notes:

1. Message ID is always 41 (geodetic navigation data), so it is ignored.

2. Navigation valid is always zero or one on the GT-11 and GT-31 and is currently ignored.

3. Navigation type is described in the SiRF binary protocol.

4. Map datum is typically 21 (WGS-84), so it is ignored.

5. Magnetic variation is always zero on the GT-11 and GT-31, so it is ignored.

6. Clock bias and clock drift are currently ignored.

7. Additional mode information is always zero on the GT-31 and always zero or one on the GT-11, so it is ignored.

8. Unfiltered SOG + COG were added by SiRF and Locosys for the GT-11.

9. SDOP and VSDOP were added by SiRF and Locosys for the GT-31, GW-62 and GW-60.



### UBX

| UBX Name                        | Raw Name | Type | Name | Type    | Units | Resolution          |
| ------------------------------- | -------- | ---- | ---- | ------- | ----- | ------------------- |
| GPS time of week                | iTOW     | <u4  | -    | -       | -     | -                   |
| Year (UTC)                      | year     | <u2  | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Month (UTC)                     | month    | u1   | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Day (UTC)                       | day      | u1   | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Hour (UTC)                      | hour     | u1   | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Minute (UTC)                    | min      | u1   | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Second (UTC)                    | sec      | u1   | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| Validity flags                  | valid    | u1   | -    | -       | -     | -                   |
| Time accuracy estimate          | tAcc     | <u4  | -    | -       | -     | -                   |
| Fraction of second (ns)         | nano     | <i4  | ts   | float64 | s     | 1 x 10<sup>-3</sup> |
| GNSS fix type (includes DR)     | fixType  | u1   | fix  | uint8   | -     | -                   |
| Fix status flags                | flags    | u1   | -    | -       | -     | -                   |
| Additional flags                | flags2   | u1   | -    | -       | -     | -                   |
| Number of satellites used       | numSV    | u1   | sat  | uint8   | -     | -                   |
| Longitude                       | lon      | <i4  | lat  | float64 | deg   | 1 x 10<sup>-7</sup> |
| Latitude                        | lat      | <i4  | lon  | float64 | deg   | 1 x 10<sup>-7</sup> |
| Height above ellipsoid          | height   | <i4  | -    | -       | -     | -                   |
| Height above mean sea level     | hMSL     | <i4  | ele  | float64 | m     | 1 x 10<sup>-3</sup> |
| Horizontal accuracy estimate    | hAcc     | <u4  | hacc | float32 | m     | 1 x 10<sup>-3</sup> |
| Vertical accuracy estimate      | vAcc     | <u4  | vacc | float32 | m     | 1 x 10<sup>-3</sup> |
| NED north velocity <sup>1</sup> | velN     | <i4  | -    | -       | -     | -                   |
| NED east velocity <sup>1</sup>  | velE     | <i4  | -    | -       | -     | -                   |
| NED down velocity <sup>2</sup>  | velD     | <i4  | roc  | float32 | deg   | 1 x 10<sup>-3</sup> |
| Ground Speed (2-D)              | gSpeed   | <i4  | sog  | float32 | m/s   | 1 x 10<sup>-3</sup> |
| Heading of motion (2-D)         | headMot  | <i4  | cog  | float32 | deg   | 1 x 10<sup>-3</sup> |
| Speed accuracy estimate         | sAcc     | <u4  | sacc | float32 | m/s   | 1 x 10<sup>-3</sup> |
| Heading accuracy estimate       | headAcc  | <u4  | cacc | float32 | deg   | 1 x 10<sup>-3</sup> |
| Position DOP                    | pDOP     | <u2  | pdop | float32 | -     | 1 x 10<sup>-2</sup> |
| Additional flags                | flags3   | <u2  | -    | -       | -     | -                   |
| Reserved                        | reserved | <u4  | -    | -       | -     | -                   |
| Heading of vehicle              | headVeh  | <i4  | -    | -       | -     | -                   |
| Magnetic declination            | magDec   | <i2  | -    | -       | -     | -                   |
| Magnetic declination accuracy   | magAcc   | <u2  | -    | -       | -     | -                   |

Notes:

1. NED north + east velocity are recorded by the [FlySight](https://www.flysight.ca/) GPS.
1. NED down velocity is the opposite of rate of climb (ROC).



