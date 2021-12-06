---
layout: default
title: Odin
description: Odin Protocol v3/v4
nav_order: 3
---

## Warning
If you just wanna to flash firmware onto a Samsung phone, click Hreidmar button below, or use leaked Odin. \
It is an implementation of this protocol, tested on Samsung Galaxy A20s.

## Defaults
* Read/Write timeout for file transfer: 30000 (30 seconds)
* Packet size for file transfer: 131072 (128 KiB)
* File transfer max sequence size: 800 (Packets)

## Terminology
* Sequence: Size is *Packet size for file transfer* multiplied by *File transfer max sequence size*
* File Part: Size is *Packet size for file transfer*.

## Other
* `Unknown` should be always 0x00. If it's not, you're in trouble.
* All non-flash packets should be sent with buffer length 1024, else the packet will be rejected.

## Handshake
It is also known as protocol initialization. It can be done once for an USB connection. \
Write: `ODIN` \
Read: `LOKE`
## Rooting
**Warning!** This command was found while looking at the `aboot.mbn` and was not tested yet! \
Also, it's purpose is yet unknown, so be careful! \
Write: `ROOTING` \
Read: `<untested, 88 bytes>`
## SECCMD
**Warning!** This command was found while looking at the `aboot.mbn` and was not tested yet! \
Also, it's purpose is yet unknown, so be careful! \
Write: `SECCMD` \
Read: `<untested, dynamic length>` 
## DVIF
**Warning!** This command was found while looking at the `aboot.mbn` and was not tested yet! \
By looking at it's code, it prints something on the screen and probably does something more. \
Write: `DVIF` \
Read: `<untested, dynamic length>`
## Session (0x64)
### Begin Session
Write: `0x64(Session) 0x00(Begin) 0x03(Odin v3)` -or- `0x64(Session) 0x00(Begin) 0x04(Odin v4)` \
Read: `0x64(Session) 0x00(Unknown)`

If Unknown is not 0:
* Read/Write timeout for file transfer: 120000 (2 minutes)
* Packet size for file transfer: 1048576 (1 MiB)
* File transfer max sequence size: 30 (Packets)
* Send `0x64(Session) 0x05(FilePartSize) 0x100000(Packet Size)`
* Read `0x64(Session) 0x00(Unknown)`
### Device Type
**WARNING!** This no longer returns what do you expect. \
It does something other that giving out the device type. \
It always returns zero according to `aboot.mbn`. \
Write: `0x64(Session) 0x01(Device Type)` \
Read: `0x64(Session) <32bit>(Device Type)`
### Erase userdata
**WARNING!** This is do a factory reset. \
Write: `0x64(Session) 0x07(Erase)` \
Read: `0x64(Session) 0x00(Unknown)`
### Total Bytes
Write: `0x64(Session) 0x02(Total Bytes) <64bit>(Bytes Length)` \
Read: `0x64(Session) 0x00(Unknown)`
### Enable T-Flash
Write: `0x64(Session) 0x05(T-Flash)` \
Read: `0x64(Session) 0x00(Unknown)`
## End Session (0x67)
### End Session
Write: `0x67(End Session) 0x00(End)` \
Read: `0x67(End Session) <32bit>(Untested)`
### Reboot
Write: `0x67(End Session) 0x01(Reboot)` \
Read: `0x67(End Session) <32bit>(Untested)`
### Shutdown
Write: `0x67(End Session) 0x03(Shutdown)` \
Read: `0x67(End Session) <32bit>(Untested)`
## PIT (0x65)
### Flashing
#### Request PIT flash
Write: `0x65(PIT) 0x00(Flash)` \
Read: `0x65(PIT) <32bit>(Untested)`
#### Begin PIT Flash
Write: `0x65(PIT) 0x02(Begin) <32bit>(Byte Length)` \
Read: `0x65(PIT) <32bit>(Untested)`
#### Send PIT data
Write: `<PIT data buffer>` \
Read: `0x65(PIT) <32bit>(Untested)`
#### End PIT Flash
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) <32bit>(Untested)`
### Dumping
#### Request PIT data dump
Write: `0x65(PIT) 0x01(Dump)` \
Read: `0x65(PIT) <32bit>(File Size)`
#### Dump PIT data block
One block is 500 bytes. \
Send an empty packet after last block. \
Write: `0x65(PIT) 0x02(FilePart) <32bit>(Index)` \
Read: `<PIT data block>`
#### End PIT dump
Identical to End PIT flash. \
Write: `0x65(PIT) 0x03(End)` \
Read: `0x65(PIT) <32bit>(Untested)`
## File (0x66)
### Request file flash
Write: `0x66(File) 0x00(Flash)` \
Read: `0x66(File) <32bit>(Untested)`
### Begin file sequence flash
Write: `0x66(File) 0x02(FilePart) <32bit>(Byte Length)` \
Read: `0x66(File) <32bit>(Untested)`
### Flash a file part
Send an empty packet for the first file part. \
Write: `<File part data>` \
Read: `0x00(File Part) <32bit>(File Part Index)`
### End file sequence flash: MODEM
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x01(Modem/CP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Is last Sequence)` \
Read: `0x66(File) <32bit>(Untested)`
### End file sequence flash: PHONE
Send an empty packet before and after. \
Write: `0x66(File) 0x03(End) 0x00(Phone/AP) <32bit>(Sequence Byte Length) 0x00(Unknown) <32bit>(Device Type) <32bit>(Identifier) <32bit>(Is last Sequence)` \
Read: `0x66(File) <32bit>(Untested)`
