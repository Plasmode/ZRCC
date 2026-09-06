# ZRCC Compact Flash Disk Layout
## Introduction
The compact flash serves both as mass storage for CP/M as well as the nonvolatile storage for system software. This document details the location of various software on the CF disk

## System Files
### ZRCC bootstrap code
Immediately after reset or power up, the CPLD ROM waits for serial input or CF disk not busy. When CF disk is detected as not busy, the CPLD ROM loads the CF data which is defaulted to track 0, sector 1 (AKA Master Boot Record) into RAM and executes it. Therefore the MBR contains the CF bootstrap code. A separate utility software is used to write CF bootstrap code into MBR as well as ZRCC Monitor into a different area in CF. The CF bootstrap code in MBR configures the CF disk as 8-bit device and accesses using Logical Block Addressing scheme (LBA); it then loads the ZRCC monitor and executes it.

### ZRCC Monitor
ZRCC monitor is stored in track 0, sector 0xF8 to sector 0xFD of the CF disk. ZRCC Monitor is loaded into its location in CF with a separate utility software to be described later.

### User Application
By default User Application is Steve Cousin's SCMonitor. ZRCC monitor command C1 copies RAM content 0x0 to 0xAFFF to LBA 0x11 to LBA 0x70) and monitor command B1 copies CF data from LBA 0x11 to LBA 0x70 to RAM starting from 0x0 and execute.

### CP/M 2.2
CP/M 2.2 BDOS/CCP/BIOS is located in LBA 0x80 to LBA 0x92. ZRCC monitor command C2 and B2 saves and loads CP/M2.2.

### CP/M 3
CP/M 3 loader is located in LBA 1 to LBA 0xF. ZRCC monitor command C3 and B3 saves and loads CP/M 3.

## CP/M Files
CP/M BIOS defines each track has 256 sectors and each sector has 512 bytes of data. Each drive has 512 directory entries. Drive B, C, D are 8 megabyte in size, Drive A is slightly smaller, 8 megabyte -128K, due to track 0 is allocated to system software.

- CP/M drive A is located from track 1 to track 0x3F.

- Drive B is located from track 0x40 to track 0x7F

- Drive C is located from track 0x80 to track 0xBF

- Drive D is located from track 0xC0 to track 0xFF

