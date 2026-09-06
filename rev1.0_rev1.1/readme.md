# ZRCC Rev1.0 and Rev1.1
Rev 1.1 and rev1.0 are the same except rev1.0 requires an engineering change.
![rev1.1](ZRCC_rev1_1_topview.jpg)

I like the idea of a low-cost Z80 running CP/M with minimum component count, but I don't like Z80 having to share the spot light with a processor many times its performance. So instead of using a powerful modern microcontroller as the I/O processor, I used a CPLD instead. The CPLD is Altera EPM7064S which is compatible with Atmel ATF1504. The CPLD's logic fabric can emulate a small ROM, 64 bytes, a simple serial port, and glue logic functions. The small ROM is enough to load program from the compact flash disk into RAM and execute. In case the compact flash is new and un-initialized, the ROM can also read in program from the serial port to initialize the new CF disk and load it with boot program. Beside the internal ROM, the CPLD also implemented a simple serial receiver and bit-bang serial transmitter. The serial receiver is just glorified shift register with fixed baud rate and protocol, 115200-N-8-1. The remaining CPLD is glue logic.
![rev1_1_annotated](zrcc_rev1_1_topview_annotated.jpg)
### Features
- Z80 running at 22MHz
- 128K banked RAM
- EPM7064S CPLD
- Compact flash drive
- CP/M-ready
- 26-pin I/O expansion (RC2014-lite)
- 84mm x 51mm 2-layer pc board
### Theory of Operation
The heart of ZRCC is the EPM7064S CPLD. It contains a 64-byte boot ROM, a buffered shift register that serves as simple serial receiver, and decoding logic. At powerup, the Z80 executes the small ROM code which continuously polls the compact flash READY signal and the serial port receive ready flag. If serial receive ready flag is set, then it jumps into the serial bootstrap routine that loads 256 serial data into memory starting from 0xB000 and jump into 0xB000 after the 256th serial data is received.

If CF READY is detected, it jumps into the CF bootstrap routine that reads the 256 words of data from CF's Master Boot Record (track 0, sector 1) into memory starting from 0xB000 and jump into 0xB000 after the 256th words are read.

Because it takes a moment for the CF disk to be ready after reset, the user can wait a second for the CF disk to be ready and boot or press a key immediately after reset button is released and select the serial bootstrap mode. The serial bootstrap is primarily for loading CF initialization software to set up a new CF disk.

### Design Information
- [~~ZRCC Rev1.0 schematic~~](zrcc1_bom.pdf) <- this is rev1.0 schematic that requires an engineering change
- [ZRCC Rev1.1 schematic](zrcc_rev1_1_scm.pdf) <- this is the current schematic
- [~~Gerber photoplots, rev1.0~~](zrcc_singlepcb_rev1.zip) <- this Gerber requires an engineering change.
- [Gerber photoplots, rev1.1](zrcc_rev1.1_gerber_single.zip) <- this is the current Gerber photoplots
- [Bill of Materials](zrcc1_bom.pdf)
- [CPLD design](zrcc_rev1_cpld_design_files.zip) files
- Memory map
### Software
- ZRCC [Serial bootstrap loader](software/zrcc_rev1_software_serialloader_rev01.zip). Enable serial bootstrap and load this file first (note: enable binary file load for this file only). This is a 256-byte hex file loader that loads a hex file to memory at 0xB400 and jump into 0xB400 when loading is completed. Note, this first step can be tricky. Here is additional instruction on how to enable serial bootstrap mode.
- [ZRCC monitor, rev0.3](software/zrcc_rev1_software_monitor_rev03.zip). Load this file with the serial bootstrap loader
- [CFBootLoader](software/zrcc_rev1_software_cfbootloader.zip). This boot loader is stored in master boot record of a CF disk. When in CF bootstrap mode, ZRCC reads in this program located in master boot record to location 0xB000 and jump into it. The CF bootloader, in turn, loads 3K bytes worth of data located in track 0, sector 0xF0 to 0xF6 of a CF disk into 0xB400 and jump into 0xB400. Beside the CFBootLoader program itself, this file contains two installation programs. Located at 0xB200 is the installation program for CFBootloader. At ZRCC monitor prompt, type 'gb200' will install CFBootLoader in the master boot record. Located at 0xB300 is the installation program for ZRCC Monitor. Type 'gb300' will install the current monitor into track 0, sector 0xF8-0xFD of the CF disk.
- [SCMonitor+StarTrek](software/zrcc_rev1_software_scmonitor_startrek.hex). This is Steve Cousin's SCMonitor ported to ZRCC. This is Steve Cousin's homepage. As an extra bonus, it includes the StarTrek program in BASIC. To install SCMonitor+StarTrek, send scmonitor_startrek.hex to ZRCC and type 'c1' to install it in track 0 of CF disk. Once it is installed, type 'b1' to load and run SCMonitor. To run Startrek in BASIC, type 'wbasic', then 'run'. Have fun! Youtube video of running Startrek in ZRCC using a TeraTerm macro file. This is the TeraTerm macro program
- [CP/M2.2 BIOS/CCP/BDOS](software/zrcc_rev1_software_cpm22all.hex). This is CP/M2.2 BIOS/CCP/BDOS all in a file. To install it, send cpm22all.hex to ZRCC and type 'c2' to install it in track 0 of CF disk. Once it is installed, type 'b2' to load and run CP/M2.2
- [XMODEM.HEX](software/cpm_software_xmodem.hex). This is the very first program in a new ZRCC CF disk. Install CP/M2.2 above first, then while in ZRCC Monitor, send xmodem.hex to ZRCC; then type 'b2' to enter CP/M2.2; then type 'save 17 xmodem.com' at CP/M prompt. This will save the RAM image into a file called xmodem.com.
- [unarj.com](software/unarj.zip). This is file decompression program running in CP/M. Use it to decompress .arj files
- [cpm22dri](software/cpm22dri.zip). This is the CP/M2.2 distribution files from Digital Research. Use unarj.com to decompress it.
- [CP/M3 Loader](software/cpm3ldr.hex). This is loader for CP/M3. It expects CPM3.SYS in drive A of the CF disk. To install it, send CPM3LDR.HEX to ZRCC and type 'c3' to install it in track 0 of CF disk. Once it is installed, type 'b3' to boot CP/M3.
- [CPM3ALL](software/cpm3all.zip). This is the CP/M 3 distribution files. Use unarj.com to decompress it.
- CPM3 [banked BIOS](software/z80sbc64_cpm3.zip) source code, assembled with zmac
- Installation macro for ZRCC CF disk. [This zipped file](software/zrcc_install_all.zip) contains all the files installed in a released ZRCC disk. The installation macro runs in TeraTerm and expects all files in directory c:\teraterm\zrcc Run the macro once ZRCC is in serial bootstrap mode.
- [Image of CF](software/zrcc_cf_installed.zip) installed using the above TeraTerm macro file. Unzip the file and use disk copy tool like Win32DiskImager to copy the image to CF disk 64MB or larger

### Manuals
- [Getting started with ZRCC](manuals/Getting_Started_with_ZRCC.md)
- ZRCC Monitor [User manual](manuals/ZRCC_monitor_user_manual.md)
- [Pictorial Assembly Guide](manuals/Pictorial_assembly_guide_zrcc_rev1_1.md) for ZRCC, rev 1
- Installing a new CF disk. Only need to do this once. The CF disk effectively serve as the system EPROM. This installation process loads the CF disk.
- CF Layout map. This document shows where various software are stored on the compact flash disk.
- Video of installing a new CF disk using a TeraTerm macro.
- Fun with ZRCC, projects you can do with ZRCC

