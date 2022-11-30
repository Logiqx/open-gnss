## Open GNSS Format

### Introduction

The GPS speedsurfing community have used a number of different GPS devices over the years, several of which have had their own proprietary file formats. Common file formats include:

- Flexible and Interoperable Data Transfer (FIT)
- GPS Exchange Format (GPX)
- National Marine Electronics Association (NMEA)
- OnAndOn (OAO) binary from the Motion GPS
- SiRF Binary (SBN) from Locosys
- SiRF Binary Packed (SBP) from Locosys
- u-blox (UBX) binary

The above formats all have their pros / cons and their suitability is very much dependent on the specific use-case, device, GNSS chipset, etc, 



### Opportunity

There is a clear opportunity to develop an open format for the GPS speedsurfing community (and other sports) which is suitable for GNSS data, regardless of the logging device or underlying GNSS chipset.

In the longer term, adoption of this format will make development and support simpler for devices and analytical software. It will enable many different types of data to be logged, enabling compact files containing minimal data for GPS speedsurfing or large files containing extensive GNSS data.

The goal of this project is to develop an open format which is simple to implement and with an emphasis on compatibility (forward and backwards) so that files are able to store data specific to the underlying GNSS chipset and tailored to each specific individual use-case.

Read the [documentation](https://logiqx.github.io/open-gnss/) for further details about project goals, etc.



### In a Hurry?

If you are already familiar with the project goals, you can jump straight to implementation "thoughts and ideas" document:

[Open GNSS - Thoughts and Ideas](thoughts.md)
