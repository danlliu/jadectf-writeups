# AutoCAD
## DFIR (50 pts)

Writeup by danlliu (WolvSec)

## Challenge

We are given a file, `poster.png`, and asked to retrieve the flag from it.

## Solution

Similar to many forensics/steg approaches, my first step was to see if the image could be opened. However, when I tried to open it, I got an error! Let's check out the file in `exiftool`:

```
ExifTool Version Number         : 12.42
File Name                       : poster_original.png
Directory                       : .
File Size                       : 476 kB
File Modification Date/Time     : 2022:10:23 09:53:08-04:00
File Access Date/Time           : 2022:10:23 09:53:27-04:00
File Inode Change Date/Time     : 2022:10:23 09:53:25-04:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 0
Image Height                    : 0
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
SRGB Rendering                  : Perceptual
Image Size                      : 0x0
Megapixels                      : 0.000000
```

The image size is set to `0x0`, which means that there's likely modification to the PNG header itself. Let's take a look at the header.

```
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 0000 0000 0000 0806 0000 0017 63ad  ..............c.
00000020: c800 0000 0173 5247 4200 aece 1ce9 0000  .....sRGB.......
00000030: 2000 4944 4154 785e 94bd 6993 6559 721c   .IDATx^..i.eYr.
```

In a PNG image, the `IHDR` chunk contains the image size information, which is stored in the 8 bytes immediately following "IHDR". We see here that the bytes are set to 0 (`00000010: 0000 0000 0000 0000`). Our goal is to find the correct width and height. But how?

For this, we can utilize the **CRC checksum** that appears in the IHDR header. In this case, it is `0x1763adc8` (found at bytes `0x1d` through `0x20`). We can utilize the following Python script to find the correct width and height:

```python
import os
import binascii
import struct

misc = open("poster.png","rb").read()

for w in range(1024):
  for h in range(1024):
    data = misc[12:16] + struct.pack('>i',w) + struct.pack('>i', h) + misc[24:29]
    crc32 = binascii.crc32(data) & 0xffffffff
    if crc32 == 0x1763adc8:
        print(w, h)
```

The script prints out `800 450`, meaning that our image is `800x450` pixels. From here, we can use a hex editor to edit the PNG file directly:

```
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 0320 0000 01c2 0806 0000 0017 63ad  ... ..........c.
00000020: c800 0000 0173 5247 4200 aece 1ce9 0000  .....sRGB.......
00000030: 2000 4944 4154 785e 94bd 6993 6559 721c   .IDATx^..i.eYr.
```

Notice that the width and heigh have been updated to `0x320 = 800` and `0x1c2 = 450` respectively. From here, we can open the image:

![The flag](autocad_assets/poster.png)

In the top left, in faint green, is the flag!

`jctf{st3g_1s_easy_as_f**k}`
