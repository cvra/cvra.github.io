---
layout: page
title: CAN Bootloader
---

<div class="row">
<div class="large-6 columns">
    <p>
    The CAN bootloader is a general purpose bootloader, that we use on all our CAN boards, it's <a href="https://github.com/cvra/can-bootloader">available on Github</a>.
    </p>
    <p>
    It allows us to flash them with application firmwares over CAN, thus removing the need to intervene directly on the boards (via debugger).
    And it stores a small config in the microcontroller's flash that makes it easily trackable (ID, board type).
    </p>
</div>
<div class="large-6 columns">
    <figure>
        <img src="/images/technologies/bootloader.png" alt="CAN Bootloader">
        <figcaption>
            <center>
                Using the CAN Bootloader to read the config of a <a href="/technologies/io_board.html">CAN IO board</a>, and flash it over CAN (using the <a href="/technologies/can_dongle.html">CAN USB dongle</a>).
            </center>
        </figcaption>
    </figure>
</div>
</div>

# Features

 - Firmware flashing over CAN, without intervention on the board directly.
 - Firmware flashing is fast, a typical firmware (few hundred kilobytes) is written in a few seconds over a 1MBbs CAN interface.
 - Parallel firmware flashing (using multicast) when several boards are flashed with the same firmware.
 - Board tracking through small config containing ID, name, board type, and number of times the flash was erased.
 - On power up, it waits for a few seconds for the user to input commands before jumping to the application.
 - Application code is checked by CRC at boot, invalid applications are not loaded.
