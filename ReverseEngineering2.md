## Reverse Engineering of AC46/BT15 Part 2

A few days ago, I received and assembled the kit which comes with another AC46 chipset.<br>
For review and specification, please click the link below.

[Aitendo K-PLY4606A](Aitendo/index.md)

By connecting the kit to my computer via male to male USB cable, it recognised as audio device and disk drive.
After running JLDFUTool, I got some error, but in the end it successfully dumped the flash.
```
>jldfutool \\.\E: Aitendo
JL dumper! for AC69XX. by kagaimiq // Mizu-DEC
compiled Dec 10 2021 - 23:11:01
-------- Get chip type and load loader if neccessary! ---------
Inquiry-> [BT15    ] [ DEVICE V1.00   ] [1.00]
------------- Get Device Info -------------
failed to do IOCTL_SCSI_PASS_THROUGH_DIRECT ioctl - 121 (The semaphore timeout period has expired. )
[jlUsbIsd_GetDeviceStatus] failed to do the command fc:0a!
failed to get device status!

>jldfutool \\.\E: Aitendo
JL dumper! for AC69XX. by kagaimiq // Mizu-DEC
compiled Dec 10 2021 - 23:11:01
failed to open scsi device `\\.\E:` - 2 (The system cannot find the file specified. )
failed to open deivce `\\.\E:`.

>jldfutool \\.\D: Aitendo
JL dumper! for AC69XX. by kagaimiq // Mizu-DEC
compiled Dec 10 2021 - 23:11:01
-------- Get chip type and load loader if neccessary! ---------
Inquiry-> [BT15    ] [UBOOT2.00       ] [1.00]
------------- Get Device Info -------------
command failed - 2
[jlUsbIsd_GetFlashPageSize] failed to do the command fc:14!
failed to get max flash page size! using default 512 bytes
Device type =3, Device ID =5e4014, Chip key =a42f
The user.app encryption key is 0000 i guess...
-------------- Try This! --------------
CRC16 of header [4b9a] and the calculated one [38e2] doesn't match!
-------------- Let's Dump! --------------
====> Calced flash size = 1048576 (1024 KB)
----------------- Reset chip! ---------------
failed to do IOCTL_SCSI_PASS_THROUGH_DIRECT ioctl - 121 (The semaphore timeout period has expired. )
[jlUsbIsd_Reset] failed to do the command fc:0c!
```

As always, I pushed the dump into hex editor and raw flash/file header appeared.<br>
[FlashHeader_Aitendo]<br>
And as I excepted, there is a overlap between _____.____2 and user.app.
But anyway, like I did before, I extracted some files manually.<br>
At this point, I can say that this strange structure is normal for AC46 firmware.
I've seen different 3 firmware files and all of them have this structure.
Although the file took from SDK is more encrypted, but it still seems to have a same structure.
I guess it's for security against reverse engineering like this, since it can be public for firmware update through BFU file, while flash image usually doesn't be public and cannot be dumped without expertise.

And for now, this is the end of this markdown.
There are more things to be investigated, and it's ongoing.
