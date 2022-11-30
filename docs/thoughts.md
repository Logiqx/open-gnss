## Open GPS - Thoughts and Ideas

This is a community-wide initiative but the GitHub project and initial thoughts and ideas were created on GitHub by Michael George (K888).

This is by no means the finished article. It is simply the product of a few hours work and is something of an in-depth brain dump.

All feedback welcome. Feel free to contact me via Seabreeze (or e-mail if you have it) and I will get back to you when time allows.



### Header Frame

A number of items will be required in the file header. These could include the following:

- Frame identifier (0x00 = header) + size = 2 bytes
- Checksum = 2 bytes
- Version number = 4 bytes [semantic versioning](https://semver.org/) to cater for the unlikely event of breaking changes in the future
- Epoch = 8 byte timestamp for first GNSS frame with millisecond precision
- Endian (big or little) so that native binary payloads from SiRF and u-blox chipsets can be used without modification
- Compression type = 0x00 for potential future usage
- GNSS chipset, if known (2 bytes) - e.g. 0x0000 = unknown, 0x0001 = SiRF, 0x02 = Trimble, 0x03 = u-blox, etc.
- Logging device manufacturer, if known (2 bytes) - e.g. 0x0000 = unspecified, 0x0001 = Locosys, etc.
- Logging device name, if known (text) - e.g. "COROS APEX 2 Pro"
- Logging device firmware, if known (text) - e.g. "2.73.0"
- Logging device serial, if known (text) - e.g. "0123456789"
- Logging device identifier, if known (text) - e.g. "K888"

I have no doubt that other fields will be required and they can be added over time in a non-breaking manner.

Results summaries have yet to be tackled - e.g. best 2s, 10s, 500m, etc.



### GNSS Frame Definitions

GNSS frame definitions will detail the content of the GNSS frames:

- Frame identifier (0x01 = GNSS frame summary) + size = 2 bytes
- Checksum = 2 bytes
- Item specifications can use bit-packing to fit the following data into 4 bytes.
  - Field identifier (14 bits) from standard dictionary - e.g. 0x00 = ts, 0x01 = lon, 0x02 = lat, etc.
  - Byte size (10 bits) of the GNSS frame payload. This excludes the frame identifier + size and checksum so it allows up to 1KB payloads.
    - Actual value = payload size - 1
  - Data type (4 bits) - signed integer, unsigned integer, bitfield, signed float, unsigned float, semicircles or text
  - Scaling (4 bits) - e.g. 1 x 10<sup>-7</sup> = 0x07 or 0x00 for no scaling.

It should be noted that optimal byte alignment is retained for these frames because every item specification is exactly 4 bytes.

Multiple frame types can appear in the open format (e.g. corresponding to individual NMEA sentences). The GNDD frame identifiers will be implicit, simply based on the order of frame definitions.



### GNSS Frames - Minimal Content

This section illustrates how minimal SiRF and u-blox GNSS frames might be implemented:

- Simple to implement within the logging devices
- Slightly more complex when reading the files but all possible variations can be handled by common code
- The definition frames enable processing of SiRF and u-blox data in exactly the same way, despite differing endian and / or precisions

It's worth noting that byte offsets / alignments have been chosen to suit the underlying data types (e.g. 4 byte integers aligned with 4-byte boundaries).

Failure to align data types properly can lead to sub-optimal processing, depending on the CPU architecture.



####  Minimal SiRF GNSS Frame

| Offset | Type |       Scaling       | Name  | Units | Description                          |
| ------ | :--: | :-----------------: | :---: | :---: | ------------------------------------ |
| 0      |  u2  |          -          |  id   |   -   | Frame identifier and size            |
| 2      |  u2  |          -          | cksum |   -   | Checksum                             |
| 4      |  u4  | 1 x 10<sup>-3</sup> |  ts   |   s   | Timestamp - ms relative to the epoch |
| 8      |  u4  | 1 x 10<sup>-7</sup> |  lat  |  deg  | Latitude                             |
| 12     |  u4  | 1 x 10<sup>-7</sup> |  lon  |  deg  | Longitude                            |
| 16     |  u2  | 1 x 10<sup>-2</sup> |  cog  |  deg  | Course Over Ground                   |
| 18     |  u2  | 1 x 10<sup>-2</sup> |  sog  |  m/s  | Speed Over Ground                    |
| 20     |  u1  | 1 x 10<sup>-2</sup> | sdop  |  m/s  | Speed Dilution of Precision          |
| 21     |  u1  | 2 x 10<sup>-1</sup> | hdop  |   -   | HDOP                                 |
| 22     |  u1  |          -          |  sat  |   -   | Number of SVs                        |

TOTAL = 23 bytes

Notes:

- The order of the elements has been chosen to ensure optimal byte alignments for the underlying types.



####  Minimal u-blox GNSS Frame

| Offset | Type |       Scaling       | Name  | Units | Description                          |
| ------ | :--: | :-----------------: | :---: | :---: | ------------------------------------ |
| 0      |  u2  |          -          |  id   |   -   | Frame identifier and size            |
| 2      |  u2  |          -          | cksum |   -   | Checksum                             |
| 4      |  u4  | 1 x 10<sup>-3</sup> |  ts   |   s   | Timestamp - ms relative to the epoch |
| 8      |  u4  | 1 x 10<sup>-7</sup> |  lat  |  deg  | Latitude                             |
| 12     |  u4  | 1 x 10<sup>-7</sup> |  lon  |  deg  | Longitude                            |
| 16     |  u4  | 1 x 10<sup>-3</sup> |  cog  |  deg  | Course Over Ground                   |
| 20     |  u4  | 1 x 10<sup>-3</sup> |  sog  |  m/s  | Speed Over Ground                    |
| 24     |  u2  | 1 x 10<sup>-3</sup> | sacc  |  m/s  | Speed accuracy                       |
| 26     |  u2  | 2 x 10<sup>-2</sup> | hdop  |   -   | HDOP                                 |
| 28     |  u1  |          -          |  sat  |   -   | Number of SVs                        |

TOTAL = 29 bytes

Notes:

- The order of the elements has been chosen to ensure optimal byte alignments for the underlying types.
- sAcc is output by u-blox chipsets using 4 bytes but this proposal suggests 2 bytes. This results in a max value of 65.535 m/s or ~130 knots.
  - TBC - This 130 knot limit for sAcc must surely be acceptable. What is the largest sAcc ever seen in a u-blox file?



### GNSS Frames - Full Payloads

Detailed GNSS frames can by direct copies of binary payloads output by SiRF and u-blox chipsets.

The frame identifier and checksum are 4 bytes in total so byte-aligned SiRF / u-blox payloads will remain correctly aligned.

| Offset | Type | Scaling | Name  | Units | Description               |
| ------ | :--: | :-----: | :---: | :---: | ------------------------- |
| 0      |  u2  |    -    |  id   |   -   | Frame identifier and size |
| 2      |  u2  |    -    | cksum |   -   | Checksum                  |
| ...    | ...  |   ...   |  ...  |  ...  | ...                       |



### GNSS Frames - Custom Content

Custom GNSS frames could be utilised by developers, adding additional data to the minimal content.

Example custom data items might include the following fields:

- elevation
- rate of climb (roc)
- NED velocities - north / east / down (down = opposite of roc)
- VDOP, PDOP, TDOP, GDOP
- VSDOP / VSDOS
- hAcc / vAcc / tAcc
- satellite IDs
- fix type
- flags

Since it is just a change to the GNSS frame definition, software that does not recognise such items can simply ignore them whilst loading the file.



### Common Features

#### Frame Identifiers

Frame identifiers can specify the frame type and frame size, packed into 2 bytes.

- Frame identifier = 6 bits which allows up to 64 different frame types, unique to a specific file
- Frame size (excluding the identifier and checksum) = 10 bits which allows for 1KB payloads.
  - Actual value = frame size - 1


It should be noted that any given frame type will always have the same first 2 bytes (identifer + size) throughout the file:

- Header frame identifier (0x00 in upper 6 bits) + size
- GNSS frame definition identifier (0x01 in upper 6 bits) + size
- GNSS frame identifier (0x02 in upper 6 bits) + size

File readers may wish to utilise the frame identifier and frame size to skip past frames that are of no interest / unsupported. This can be even be done without using the detailed field specifications in the GNSS frame definition(s).



#### Checksums - 2 bytes

A simple checksum algorithm is desired with minimal processing overhead, yet effective error detection:

- SiRF use a very weak checksum so it and will be discounted.
- u-blox use [Fletcher's checksum](https://en.wikipedia.org/wiki/Fletcher%27s_checksum) to provide error-detection, approaching a [cyclic redundancy check](https://en.wikipedia.org/wiki/Cyclic_redundancy_check) but with lower computational effort.

[Fletcher's checksum](https://en.wikipedia.org/wiki/Fletcher%27s_checksum) is well suited because of it's simple implementation and low computational overheads.

It has the added benefit that existing C / Java / Python code within UBX and OAO readers can be re-used.



#### Timestamps - 4 bytes

The minimal GNS frame examples take the opportunity to save space by using 4 byte timestamps:

- Store the "epoch" in the file header (or subsequent epoch frames).
- Store milliseconds since the epoch in 4 bytes. Two options to consider:
  - Can use integer artithmetic
    - value % 1000 = milliseconds
    - value // 1000 = seconds since epoch
      - max duration = 49.71 days
  - Bit masks
    - 10 bits = milliseconds
    - 22 bits = seconds since epoch
      - max duration = 48.55 days
- There are two obvious options for handling the 48-49 day rollover:
  1. Just wrap around when generating the file and let the file reader(s) figure it out.
  2. Add “epoch” frames (where necessary) to reset the timer to zero.

The first option isn't particularly viable because existing SBP files can contain vast date ranges and therefore could not be converted to the open format.

The second option is suited to devices that are either likely to record continuously for in excess of 48 days, or software that converts SBP files to the open format. It would be very easy to implement in both the file writers and file readers.



### Additional Notes

#### Speed Over Ground

In theory, consumer GPS units are limited to 1000 knots which is approximately 514.44 m/s.

- SiRF precision is 2 decimal places which means only 2 bytes are required for SOG. Limit is 65,535 = 655.35 m/s which exceeds 1,000 knots.
- u-blox precision is 3 decimal places which means 2 bytes are insufficient for SOG. Either 3 or 4 bytes are therefore required.



#### Degrees vs Semicircles

FIT files store latitude and longitude in semicircles.

This is actually relatively pointless because standard precision GNSS chipsets output latitude and longitude as 4 byte integers with a scaling of 1 x 10<sup>-7</sup>.

Converting the native output of the GNSS chip from degrees to semicircles may use the full 32-bit range but it does not add any precision!

Since semicircles are what are recorded in FIT files they should be perfectly preserved within the open format, using the dedicated semicircles data type.



#### RTK / PPK

High precision logging devices such as the u-blox NEO-M8P and ZED-F9P add the following items to record higher precision:

- lonHp and latHp (1 byte each) which have a scaling of 1e-9.

If such a device wished to log data in the open format they simply need to specify them in the GNSS frame definition.



#### Special Values

The SiRF files use 0xff in SDOP and VSDOP (aka SDOS and VSDOS) to indicate that value is unknown.

Some consideration needs to be given to these special cases so they are not confused with genuine values.

It is also worth mentioning that SDOP and sAcc are similar but subtly different. The header allows them to be distinguished using the GNSS chipset.