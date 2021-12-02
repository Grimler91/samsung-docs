---
layout: default
title: PIT
description: Samsung Partition Table Format
nav_order: 4
---

## Header
`0x12349876` is the magic number that is located in first 4 bytes of a PIT file. \
After that we have 8 32-bit numbers, it's unknown what does they do.

## Entries
Entries begin after first 28 bytes. \
One entry is 132 bytes long.
### Binary Type
Type: 32-bit number
Value: 
* AP = 0
* BL = 1

### Device Type
Type: 32-bit number
Value:
* NAND = 0
* File = 1
* MMC = 2
* All = 3

### Identitifier
Type: 32-bit number
### Attributes
Type: 32-bit number
Flags:
* STL = 0x0001
* Write = 0x0010

### Update Attributes
Type: 32-bit number
Flags:
* FOTA = 0x0001
* Secure = 0x0010

### Block size/offset
Type: 32-bit number
### Block count
Type: 32-bit number
### File Offset (obsolete)
Type: 32-bit number
### File Size (obsolete)
Type: 32-bit number
### Partition name
Type: 32 bytes long string
### File name
Type: 32 bytes long string
### FOTA name
Type: 32 bytes long string
