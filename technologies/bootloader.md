---
layout: page
title: CAN Bootloader
---

The CAN bootloader is a general purpose bootloader, that we use on all our CAN boards, it's [available on Github](https://github.com/cvra/can-bootloader).
It allows us to flash them with application firmwares over CAN, thus removing the need to intervene directly on the boards (via debugger).
It also stores a small config in the microcontroller's flash that makes it easily trackable (ID, board type).

# Features

 - Firmware flashing over CAN, without intervention on the board directly
 - Firmware flashing is fast, less than 1 minute for a 1MB firmware over 1MBbs CAN interface
 - Parallel firmware flashing (using multicast) when several boards are flashed with the same firmware
 - Board tracking through small config containing ID, name, board type, and number of times the flash was erased
 - On power up, it waits for a few seconds for the user to input commands before jumping to the application
 - Application code is checked by CRC at boot, invalid applications are not loaded
