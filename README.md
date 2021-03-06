# DSO1511e+ Unbrick


To unbrick the DSO1511e+, after trying to update using the wrong file, it wasn't unzipped,
- (Special thanks to **pcprogrammer** from the eevblog.com forum, thanks for your patience in guiding step by step!)
- (Sorry, but I don't know this hardware or the software, any questions please contact the developers)

(Note: If anyone can share the original firmware dump I would be grateful)

Maybe you can try these steps:

- 1 - Create an sd card with bootloader that activates the usb port to load the binary file in flash.
- 2 - Insert the SD card (Contacts facing the DSO1511e+ board).
- 3 - Keep the DSO151e+ power button pressed (After writing the firmware it manages to keep it powered up).
- 4 - Test if "ID 1f3a:efe8 Allwinner Technology sunxi SoC OTG connector in FEL/flashing mode" (or something similar) appears using the "lsusb" command. (The USB cable must be connected)
- 5 - Back up flash memory.
- 6 - Load the binary file into flash (Bootloader + Firmware Update).
- 7 - Turn off the DSO151e+.
- 8 - Remove the SD card.
- 9 - Restart the DSO1511e+.
- 10 - Enter update mode (commands described in the steps provided by the supplier).
- 11 - Update the DSO1511e+ following all supplier steps
- 12 - Perform the post-update commands described in the steps provided by the supplier (calibraton etc)

Note The boot image does not appear after performing these procedures, due to the improvised bootloader.

Sources:
All rights reserved to the respective authors.
This library is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

- Firmware supplier: https://www.youtube.com/watch?v=3QZC1mhwGlI

- Bootloader supplier: https://github.com/pecostm32

Step details:

Step 1 - Create an sd card with bootloader that activates the usb port to load the binary file in flash:
- Write the /sunxi_stuff/fel-sdboot.sunxi file to an SD card using the command:

Make SD card:

- sudo dd if=fel-sdboot.sunxi of=/dev/sdc bs=1024 seek=8


(Make sure your card is at /dev/sdc, or modify the command with the correct letter, you can "mount" this device and use the "df -h" command to see the partition name, for example sdc1 ==> sdc, sdf1 ==> sdf)


Step 5 - Back up flash memory.
- It is recommended to backup the Flash before trying to update, try this command below:
(But if it doesn't work, it may be necessary to remove the W25Q32 memory from the PCB and use an EEPROM programmer such as CH341 for example)

Read from flash

- sudo ./sunxi-fel -p spiflash-read 0 4194304 w25q32_backup.bin

- - sunxi-fel Usage: https://linux-sunxi.org/FEL

Step 6 - Load the binary file into flash (Bootloader + Firmware Update).

- To write the file to W25Q32 memory, use this command:
Write to flash

- sudo ./sunxi-fel -p spiflash-write 0 DSO1511e+_v1.2.7_and_bootloader_for_W25Q32.bin

Note 1:
Used the program xgpro (for the TL866II Plus programmer) to combine (merge) the binary files.

- Place the bootloader binary file at address 0x00.
- Place the firmware file at address 0x10000.

- Export (save as) as a binary file (*.bin)


Note 2:
How to compile your own bootloader:
  
(Tested on Xubuntu 22.04)

Make the modifications to the fnirsi_1013d_bootloader.c file.

- cd fnirsi_1013d_bootloader
- make

The binary file is in the folder: 
/fnirsi_1013d_bootloader/dist/Debug/GNU_ARM-Linux/fnirsi_1013d_bootloader.bin
(Folder will be created during compilation)

Note 3:

To compile you must have arm-none-eabi-gcc installed:

- sudo apt-get update
- sudo apt install arm-none-eabi-gcc
