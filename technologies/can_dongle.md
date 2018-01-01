---
layout: page
title: CAN USB Dongle
---

<div class="row">
<div class="large-6 columns">
    <p>
    The CAN USB dongle is a small USB device that allows us to communicate over the CAN bus from our dev computers.
    It supports the SLCAN protocol, which allows it to be interfaced as a UNIX socket via SocketCAN (on Linux / Mac OS X).
    </p>
    <p>
    The CAN USB dongle can also power the CAN bus, which is useful when debugging single boards outside our robots.
    </p>
    <h1>Contents</h1>
    <p>
        <ul>
            <li><a href="can_dongle.html#features">Features</a></li>
            <li><a href="can_dongle.html#hardware">Hardware</a></li>
            <li><a href="can_dongle.html#software">Software</a></li>
            <li>Quick links:
                <a href="https://github.com/cvra/can-usb-dongle">Hardware</a> |
                <a href="https://github.com/cvra/can-usb-dongle-fw">Software</a>
            </li>
        </ul>
    </p>
</div>
<div class="large-6 columns">
    <figure>
        <img src="/images/technologies/can-dongle.jpg" alt="CAN USB Dongle">
        <figcaption>
            <center>
                The CAN USB dongle with micro USB wired (left).
            </center>
        </figcaption>
    </figure>
</div>
</div>

<a name="features"></a>

# Features

 - CAN interface to communicate with nodes on the CAN bus, with 2 Molex picoblade connectors for daisy chain wiring.
 - Dedicated MCU to translate CAN to SLCAN (Serial-Line CAN) over the USB interface.
 - Power regulator to power a few boards over the CAN bus, useful for debugging single boards outside our robots. Activated by pressing the button for more than 1 second
 - UART interface that adds FTDI-like functionality to the dongle.
 - micro-USB interface used for interfacing the computer, but also used for DFU flashing, activated by pressing the button while wiring the dongle over USB.

<a name="hardware"></a>

# Hardware

The board is made up of a small double-sided PCB with 2 copper layers.
Schematics and layout are made with KiCad and [available on Github](https://github.com/cvra/can-usb-dongle).
It is powered by a STM32F3 controller, and has a few LEDs indicating status and if the bus is powered by the dongle.

<a name="software"></a>

# Software

The firmware of the CAN USB dongle is written using [ChibiOS RTOS and HAL](http://www.chibios.org/) and is [available on Github](https://github.com/cvra/can-usb-dongle-fw).
It implements CAN to USB translation using the Serial-Line CAN protocol (SLCAN).
This allows us to expose the CAN interface as a UNIX socket (with SocketCAN), and the standard `candump` / `cansend` can be used.

One major use case of the CAN USB dongle, is flashing our boards over CAN, leveraging the [CAN bootloader](/technologies/bootloader.html).

Using the SocketCAN interface, we can also monitor our boards during application execution using [UAVCAN](http://uavcan.org/) tools such as the [UAVCAN node monitor](https://github.com/UAVCAN/libuavcan/blob/master/libuavcan_drivers/linux/apps/uavcan_monitor.cpp) or the [GUI tool](https://github.com/UAVCAN/gui_tool).
