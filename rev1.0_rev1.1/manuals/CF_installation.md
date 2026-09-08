# Installing a New CF Disk for ZRCC
### Introduction
On ZRCC the CF disk effectively serves as the system EPROM. When a new CF disk is introduced, it has no useful information. This installation procedure programs the CF disk to serve as the system EPROM. Here is the step-by-step description.

Recall there is a tiny ROM (64bytes) in CPLD that can load from either CF disk or serial port. Since the CF disk is blank, we need to start from the serial port:
1. Enable serial bootstrap (this step is tricky. Refer to section below for additional information for enabling serial bootstrap) and load serial bootstrap loader, which is a 256-byte hex loader.
2. Use the 256-byte hex loader to load ZRCC monitor
3. Use ZRCC monitor to load CF boot loader that has a bootstrap loader for CF disk and the associated installation software
4. Run CF boot loader to save CF bootstrap code into Master Boot Record of the CF disk
5. Run 2nd part of the CF boot loader to save the ZRCC monitor into specific sectors in track 0 of CF disk
6. Enable CF bootstrap and recycle power. The tiny ROM code in ZRCC will now load the CF bootstrap from Master Boot Record, which in turn, load ZRCC Monitor and run.
7. At ZRCC monitor, load CPM22 BIOS/BDOS/CCP into memory and type 'c2' to copy it into certain sectors in track 0 of the CF disk
8. In ZRCC monitor, load XMODEM.HEX and type 'b2' to boot into CPM22 with XMODEM image in memory 0x100
9. type 'save 17 XMODEM.com' to create the first CP/M22 file in the new CF disk
10. Type 'xmodem unarj.com /r/z1' to copy a decompression program to CF disk
11. Type 'xmodem cpm2.arj /r/z1' to copy compressed CP/M22 distribution files to CF disk
12. Type 'unarj e cpm2' to decompress CP/M22 distribution files to CF disk
13. With CF bootstrap in place and CP/M22 installed, now I can just turn on power and type 'b2' and I'm in CP/M.

[~~TeraTerm macro for installing CF~~](https://github.com/Plasmode/ZRCC/blob/main/rev1.0_rev1.1/manuals/zrcc_newcf_teraterm_macro.zip).<- replaced with updated macro, see below. The above procedures can be automated with a macro in TeraTerm, a terminal emulator running in Windows environment. To run the macro, unzip the files into c:\teraterm\zrcc file folder and run the macro file 'newCF.ttl' in TeraTerm.

[Video](https://youtu.be/eARz73N0KL0) of installing a new CF disk using TeraTerm macro, newCF.ttl.

[Updated TeraTerm macro](https://github.com/Plasmode/ZRCC/blob/main/rev1.0_rev1.1/software/zrcc_install_all.zip) that installs CP/M 2.2, CP/M 3, HiTech C, Zork, and Startrek edition of SCMonitor. To run the macro, unzip the files into c:\teraterm\zrcc folder and run the macro file “newCF.ttl” in TeraTerm

### Additional instruction for enabling serial bootstrap mode
Enabling the serial bootstrap mode is a somewhat tricky process. After reset button is released, the CF disk needs about half second to initialize itself to be ready. If the ROM software detected a serial character during the wait for CF to be ready, it will switch to the serial bootstrap mode. To enable the serial bootstrap mode:

- Set up the serial port to 115200 N-8-1 and be ready to send a character to ZRCC,
- Press and release the RESET button. As soon as the button is released, press the keyboard once to send a character to ZRCC,
- ZRCC is now in serial bootstrap mode, but there are no visual indication at all,
- Send the [serial loader](../software/zrcc_rev1_software_serialloader_rev01.zip) as binary file (In TeraTerm, check the “Binary” box), the console will respond with
```
ZRCC Loader v0.1
Auto start at 0xB400
```
ZRCC is now ready to accept an Intel Hex file and start execution from 0xB400

