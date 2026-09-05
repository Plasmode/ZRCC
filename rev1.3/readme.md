ZRCC rev1.3 replaced the 55nS 128K RAM with 25 nS RAM and change the IDE44 interface to accept disk-on-module. Rev1.3 explores overclocking Z80 to 33MHz.




### Design Files
- Schematic

- Gerber photoplots

- CPLD design file for 33MHz operation

- CPLD design file for 29.5MHz clock

### Software for 29.5MHz clock
- ZRCC Serial bootstrap loader. Enable serial bootstrap and load this file first (note: enable binary file load for this file only). This is a 256-byte hex file loader that loads a hex file to memory at 0xB400 and jump into 0xB400 when loading is completed. Note, this first step can be tricky. Here is additional instruction on how to enable serial bootstrap mode.

- ZRCC monitor, rev0.4. Load this file with the serial bootstrap loader

- CFBootLoader. This boot loader is stored in master boot record of a CF disk. When in CF bootstrap mode, ZRCC reads in this program located in master boot record to location 0xB000 and jump into it. The CF bootloader, in turn, loads 3K bytes worth of data located in track 0, sector 0xF0 to 0xF6 of a CF disk into 0xB400 and jump into 0xB400. Beside the CFBootLoader program itself, this file contains two installation programs. Located at 0xB200 is the installation program for CFBootloader. At ZRCC monitor prompt, type 'gb200' will install CFBootLoader in the master boot record. Located at 0xB300 is the installation program for ZRCC Monitor. Type 'gb300' will install the current monitor into track 0, sector 0xF8-0xFD of the CF disk.

- SCMonitor+StarTrek. This is Steve Cousin's SCMonitor ported to ZRCC. This is Steve Cousin's homepage. As an extra bonus, it includes the StarTrek program in BASIC. To install SCMonitor+StarTrek, send scmonitor_startrek.hex to ZRCC and type 'c1' to install it in track 0 of CF disk. Once it is installed, type 'b1' to load and run SCMonitor. To run Startrek in BASIC, type 'wbasic', then 'run'. Have fun! Youtube video of running Startrek in ZRCC using a TeraTerm macro file. This is the TeraTerm macro program

- CP/M2.2 BIOS/CCP/BDOS. This is CP/M2.2 BIOS/CCP/BDOS all in a file. To install it, send cpm22all.hex to ZRCC and type 'c2' to install it in track 0 of CF disk. Once it is installed, type 'b2' to load and run CP/M2.2

- XMODEM.HEX. This is the very first program in a new ZRCC CF disk. Install CP/M2.2 above first, then while in ZRCC Monitor, send xmodem.hex to ZRCC; then type 'b2' to enter CP/M2.2; then type 'save 17 xmodem.com' at CP/M prompt. This will save the RAM image into a file called xmodem.com.

- unarj.com. This is file decompression program running in CP/M. Use it to decompress .arj files

- cpm22dri. This is the CP/M2.2 distribution files from Digital Research. Use unarj.com to decompress it.

- CP/M3 Loader. This is loader for CP/M3. It expects CPM3.SYS in drive A of the CF disk. To install it, send CPM3LDR.HEX to ZRCC and type 'c3' to install it in track 0 of CF disk. Once it is installed, type 'b3' to boot CP/M3.

- CPM3ALL. This is the CP/M 3 distribution files. Use unarj.com to decompress it.

- CPM3 banked BIOS source code, assembled with zmac

- Installation macro for ZRCC CF disk. This zipped file contains all the files installed in a released ZRCC disk. The installation macro runs in TeraTerm and expects all files in directory c:\teraterm\zrcc_29m. Run the macro once ZRCC is in serial bootstrap mode.

### Procedure for checking out overclocked Z80
Sweep supply voltage between 4.75V and 5.25V while running the built-in memory test
Initialize and upload all software to a new DOM disk using a TeraTerm installation macro.
Set voltage to 5.0V, boot into CP/M2.2, run “pip c:=b:*.*[v]”, where drive C is an empty drive and drive B contains CP/M2.2 distribution
Use XMODEM to transfer large (megabyte) file.
Set voltage to 5.0V, run zexall.com. It should pass with no error. For 33MHz Z80, it took about 27 minutes to complete zexall test.
Run ASCII mandelbrot benchmark, 'mbasic80 asciiart.txt'. It should complete in 40 seconds
Boot into SCMonitor and play a game of Startrek using the TeraTerm macro.
Compile and run a “Hello World” C program using Hitech C v3.09
builderpages/plasmo/z
