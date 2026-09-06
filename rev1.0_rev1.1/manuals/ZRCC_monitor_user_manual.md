# ZRCC Monitor V0.3 User Manual
## Introduction
ZRCCMon is the monitor program for ZRCC. Once installed in track 0, sector 0xF8-0xFD of the compact flash disk, it is the program Z80 loads into RAM from 0xB400-0xBFFF immediately after reset. After the load is completed, Z80 jumps into 0xB400 to execute ZRCCMonitor.

## ZRCCMon commands
ZRCCMon is a simple monitor with the following single-key commands. Except when noted, the commands may be entered in upper or lower cases. In the following description, command entered is in bold, the response is in italic

### H
```
help
G <addr> CR
R <track> <sector>
D <start addr> <end addr>
Z CR
F CR
T CR
E <addr>
X <options> CR
B <options> CR
C <options> CR
```
### G
```
go to address: 0x

Enter the 4 hexadecimal address values. Confirm the command execution with a carriage return or abort the command with other keystroke.
```
### R
```
read CFdisk track:0x

Enter the 2 hexadecimal digits for the track number and 2 hex digits for the sector value. The content of the selected track/sector will be displayed as 512-byte data block. Press carriage return for next sector or any other key to return to command prompt.

>read CF disk track:0x01 sector:0x00
+0000 : 00 58 4D 4F 44 45 4D 20 20 43 4F 4D 00 00 00 21 .XMODEM COM…!
+0010 : 04 00 05 00 00 00 00 00 00 00 00 00 00 00 00 00 …………….
+0020 : 00 55 4E 41 52 4A 20 20 20 43 4F 4D 00 00 00 60 .UNARJ COM…`
+0030 : 06 00 07 00 08 00 00 00 00 00 00 00 00 00 00 00 …………….
+0040 : 00 43 50 4D 33 20 20 20 20 41 52 4A 01 00 00 80 .CPM3 ARJ….
+0050 : 09 00 0A 00 0B 00 0C 00 0D 00 0E 00 0F 00 10 00 …………….
+0060 : 00 43 50 4D 33 20 20 20 20 41 52 4A 03 00 00 80 .CPM3 ARJ….
+0070 : 11 00 12 00 13 00 14 00 15 00 16 00 17 00 18 00 …………….
+0080 : 00 43 50 4D 33 20 20 20 20 41 52 4A 05 00 00 80 .CPM3 ARJ….
+0090 : 19 00 1A 00 1B 00 1C 00 1D 00 1E 00 1F 00 20 00 ………….. .
+00A0 : 00 43 50 4D 33 20 20 20 20 41 52 4A 07 00 00 80 .CPM3 ARJ….
+00B0 : 21 00 22 00 23 00 24 00 25 00 26 00 27 00 28 00 !.“.#.$.%.&.'.(.
+00C0 : 00 43 50 4D 33 20 20 20 20 41 52 4A 09 00 00 80 .CPM3 ARJ….
+00D0 : 29 00 2A 00 2B 00 2C 00 2D 00 2E 00 2F 00 30 00 ).*.+.,.-…/.0.
+00E0 : 00 43 50 4D 33 20 20 20 20 41 52 4A 0B 00 00 80 .CPM3 ARJ….
+00F0 : 31 00 32 00 33 00 34 00 35 00 36 00 37 00 38 00 1.2.3.4.5.6.7.8.
+0100 : 00 43 50 4D 33 20 20 20 20 41 52 4A 0D 00 00 80 .CPM3 ARJ….
+0110 : 39 00 3A 00 3B 00 3C 00 3D 00 3E 00 3F 00 40 00 9.:.;.<.=.>.?.@.
+0120 : 00 43 50 4D 33 20 20 20 20 41 52 4A 0E 00 00 11 .CPM3 ARJ….
+0130 : 41 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 A……………
+0140 : 00 43 50 4D 32 20 20 20 20 41 52 4A 01 00 00 80 .CPM2 ARJ….
+0150 : 42 00 43 00 44 00 45 00 46 00 47 00 48 00 49 00 B.C.D.E.F.G.H.I.
+0160 : 00 43 50 4D 32 20 20 20 20 41 52 4A 03 00 00 80 .CPM2 ARJ….
+0170 : 4A 00 4B 00 4C 00 4D 00 4E 00 4F 00 50 00 51 00 J.K.L.M.N.O.P.Q.
+0180 : 00 43 50 4D 32 20 20 20 20 41 52 4A 04 00 00 0D .CPM2 ARJ….
+0190 : 52 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 R……………
+01A0 : 00 48 54 43 20 20 20 20 20 41 52 4A 01 00 00 80 .HTC ARJ….
+01B0 : 53 00 54 00 55 00 56 00 57 00 58 00 59 00 5A 00 S.T.U.V.W.X.Y.Z.
+01C0 : 00 48 54 43 20 20 20 20 20 41 52 4A 03 00 00 80 .HTC ARJ….
+01D0 : 5B 00 5C 00 5D 00 5E 00 5F 00 60 00 61 00 62 00 [.\.].^._.`.a.b.
+01E0 : 00 48 54 43 20 20 20 20 20 41 52 4A 05 00 00 80 .HTC ARJ….
+01F0 : 63 00 64 00 65 00 66 00 67 00 68 00 69 00 6A 00 c.d.e.f.g.h.i.j.
Data NOT same as previous read

carriage return for next sector, any other key for command prompt
```
### D
```display memory from 4 hexadecimal digits start address to 4 hexadecimal end address. If start address is greater than the end address, only 1 line (16 bytes) of data will be displayed.

D 0400 0420

0400 : C3 09 04 88 B0 FB 00 00 00 31 FF 0F 0E 08 2E FF
0410 : ED 6E DB E8 32 03 04 3E B0 D3 E8 DB E8 32 04 04
0420 : 2E 00 ED 6E D3 A0 CD 54 0A 3E E2 D3 10 3E 80 D3
```
### I
```
Read from I/O port and display the value in hexadecimal value

input from port e8
Value=40
```
### O
```
Output hexadecimal value to port in hexadecimal value

output 23 to port e8
```
### L
```
List memory in Intel Hex format. Enter the 4 hexadecimal start address and 4 hexadecimal end address. If start address is greater than the end address, only 1 line (16 bytes) of data will be displayed in the Intel Hex format.

list memory as Intel Hex, start address=1000 end address=1100
:10100000F2CE2290EDFCCF41B837B88BA1304D8F96
:10101000F3FBB33AB73ACFE00D1A08A632E78FFCDC
:10102000B7DEEBBF9ABC0CA6290BBD6DBBBDBCF7F0
:10103000538EC9B06083FEB9EA4641FFEF92CF8B71
:10104000F6FE08FBAEFFC8EAED6AFC8AD90463FA33
:10105000EEBBF06E03B2E828A631F8E0E03BCBD05F
:10106000F3B3BABABC1B2AFF3632BB3CB092883B02
:101070002DF8BA0A2E04FE838BE5FFBE3EDE937A7E
:10108000B8EEC6CAA301A2A8C283AFF3AEFB9E7995
:10109000080AAB79889A0ECABD0CA9A8DEA20F3B3C
:1010A000A44CACE4F07FA08B218EC65DE0FA95AE37
:1010B000B73348ECF4B96C8C8CC2C3CFF2979ADB8F
:1010C000B5DBC8CBB3A872ABEE8F128A0B23995C49
:1010D000E9B0328CE80C82AC977E2A83EBB820B260
:1010E000FF9880322391EC4FA00CE88CF1F824FC9F
:1010F000FDE0F8B2CA8BADBD0BEC3C3CFE03020137
:00000001FF
```
### Z
```zero memory
press Return to execute command

Fill memory from 0xC000 to 0xFFFE and from 0x0 to 0xAFFF with 0x0. Press carriage return to confirm the command execution; press other key to abort the command
```
### F
```fill memory with 0xFF
press Return to execute command

Fill memory from 0xC000 to 0xFFFE and from 0x0 to 0xAFFF with 0xFF. Press carriage return to confirm the command execution; press other key to abort the command
```
T
test memory
press Return to execute command

Test memory from 0xC000 to 0xFFFE and from 0x0 to 0xAFFF. The memory is filled with unique test patterns generated from a seed value. The seed value is changed for each iteration of the test. Each completed iteration will display an 'OK' message. Any keystroke during the test with abort the test and return to command prompt.

E
Edit memory specified with the 4 hexadecimal digits value. Exit the edit session with 'X'

E 0000

0000 : FF 12 12
0001 : FF 23 23
0002 : EF 00 00
0003 : 7F 01 01
0004 : F7 x

X
clear disk directories
A – drive A,
B – drive B:
C – drive C,
D – drive D,

Fill the directories of the selected disk with 0xE5. This effectively erase the entire disk. The disk letter __mustu be in upper case. Confirm the command with a carriage return or abort command with any other key stroke.

B
boot CP/M
1–User Apps,
2–CP/M2.2:
3–CP/M3:
Enter '2' to boot CP/M 2.2 (it is the only option for now). This assumes the appropriate software has been copied to RAM disk as described under the “C” command. Confirm the command with a carriage return or abort command with any other key stroke.

boot CP/M
1–User Apps,
2–CP/M2.2:
3–CP/M3: 2 press Return to execute command
Copyright 1979 © by Digital Research
CP/M 2.2 for Z80SBC64 Rev1 12/16/18

a>

C
copy to CF
0–boot,
1–User Apps,
2–CP/M2.2:
3–CP/M3:

Prior to execution of the C2 command, CP/M2.2 BDOS/CCP/BIOS must be loaded in memory 0xDC00-0xFFFF.

```
