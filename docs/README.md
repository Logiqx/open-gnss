## Open GPS Format

### Introduction

The GPS speedsurfing community have used a number of different GPS devices over the years, several of which have had their own proprietary file formats. Common file formats include:

- Flexible and Interoperable Data Transfer (FIT).
- GPS Exchange Format (GPX).
- National Marine Electronics Association (NMEA).
- OnAndOn (OAO) binary from the Motion GPS.
- SiRF Binary (SBN) from Locosys.
- SiRF Binary Packed (SBP) from Locosys.
- u-blox (UBX) binary.

The above formats all have their pros / cons and their suitability is very much dependent on the specific use-case, device, GNSS chipset, etc, 



### Opportunity

There is a clear opportunity to develop an open format for the GPS speedsurfing community (and other sports) which is suitable for GNSS data, regardless of the logging device or underlying GNSS chipset.

In the longer term, adoption of this format will make development and support simpler for devices and analytical software. It will enable many different types of data to be logged, enabling compact files containing minimal data for GPS speedsurfing or large files containing extensive GNSS data.

The goal of this project is to develop an open format which is simple to implement and with an emphasis on compatibility (forward and backwards) so that files are able to store data specific to the underlying GNSS chipset and tailored to each specific individual use-case.



### Goals

The following list is a summary of the goals / requirements:

- Open Source
  - Thorough documentation with examples in C, Java and Python and a full range of test files.
- Easy to implement within GNSS logging devices.
  - A simple but flexible file format which is easy for software developers to understand.
- Flexible format with simple definitions describing the various frame types within the file.
  - This will enable data content specific to the use-case; however minimal or extensive.
- Ability to include data from all commonly used GNSS chipsets and existing file formats in a lossless manner.
  - Conversions of existing files to the open format and back again should be completely lossless.
  - Support the full native GNSS chipset precision of all data items.
  - Support for all common data types; big and little endian.
    - integers (signed and unsigned), bitfields, floats (signed and unsigned), semicicles and text.
  - Support for differing scaling factors and differing precisions
    - e.g. u-blox data such as cog and sog are higher precision than the SiRF equivalents.
  - Support for similar but subtly different data items.
    - e.g. SDOP / SDOS (SiRF) and sAcc (u-blox) are similar but subtly different.
- Low processing overhead for the the logging device.
  - Ideally allow native data types from the GNSS chip to be written straight into the output frames.
- Checksums
  - Simple checksum algorithm with minimal processing overhead, yet effective error detection.




Possible future requirements:

- Native compression (e.g. [zlib](https://www.zlib.net/)) has been deemed out of scope at this time.
  - The proposed header frame does however include a flag to indicate native compression so that it can be added in the future, if required.




#### Data Types

- Integers
  - Signed Integer - 1, 2, 4 or 8 bytes
  - Unsigned Integer - 1, 2, 4 or 8 bytes
- Bitfields - 1, 2, 4 or 8 bytes
- Floats
  - Signed Floats (IEEE 764) - 4 or 8 bytes 
  - Unsigned Floats (u-blox) - 1 or 2 bytes
- Semicircles
  - The native 4-byte representation of longitude and latitude in FIT files
- Text - UTF-8 encoded



#### Native Frame Support

The file will need to contain specifications of the frame types and the associated data items.

The data items within say a GNSS frame may be a minimal set of data items or an extensive list.

The frame specifications make it possible to copy entire payloads or specific data items from the binary payloads of the GNSS chip; SiRF or u-blox.

The file reader will not need to treat different logging devices or GNSS chipsets any differently since the frame specifications will be standard.



#### Formats

It should be (theoretically) possible to convert any existing files to the open format.

Converting to the open format then back to the original format needn't be implemented but it should (theoretically) be lossless.

The open format should therefore have the ability to store precise data from the following existing formats and associated GNSS chipsets:

- FIT
  - Standard GNSS data, plus the COROS developer fields
- GPX
  - GPX 1.0 standard data
  - GPX 1.1 standard data, plus sog and cog of TrackpointExtension 2.0
- NMEA
  - The 5 sentences implemented by most / all GNSS chipsets - GGA, GSA, GSV, RMC, VTG
  - The 3 sentences implemented by many GNSS chipsets - GLL, GNS, ZDA
  - The 3 sentences containing error / accuracy information - EPE, GBS, GST
  - Manufacturer specific sentences - e.g. PSRFEPE which contains SiRF error estimates
- OAO
  - OAO header content - 0x0ad0
  - Hybrid of UBX-NAV-PVT and UBX-NAV-DOP (HDOP) -  0x0ad4 and 0x0ad5
- SBN
  - Locosys header content
  - Geodetic Navigation Data - Message ID 41
  - GT-11 extensions to Geodetic Navigation Data (unfiltered speeds)
  - GT-31 extensions to Geodetic Navigation Data (SDOP and VSDOP)
- SBP
  - Locosys header content
  - Packed Geodetic Navigation Data with support for GT-11 and GT-31 variants
- UBX
  - The 2 most commonly used navigation frames - UBX-NAV-PVT, UBX-NAV-DOP



Further details about these existing formats is provided on a separate [page](formats.md).



### Next Steps

This is a community-wide initiative but the GitHub project and initial thoughts and ideas were created on GitHub by Michael George (K888).

The open format needs to be devised, reviewed, discussed, tweaked and so forth. Early thoughts and ideas have been captured in separate document:

- [Open GPS - Thoughts and Ideas](thoughts.md)

The above is by no means the finished article. It is simply the product of a few hours work and something of an in-depth brain dump.

All feedback welcome. Feel free to contact me via Seabreeze (or e-mail if you have it) and I will get back to you when time allows.
