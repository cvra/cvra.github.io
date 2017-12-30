---
layout: page
title: CAN IO Board
---

<h1>Contents</h1>
<p>
    <ul>
        <li><a href="io_board.html#features">Features</a></li>
        <li><a href="io_board.html#hardware">Hardware</a></li>
        <li><a href="io_board.html#software">Application software</a></li>
        <li>Quick links:
            <a href="https://github.com/cvra/can-io-board">Hardware</a> |
            <a href="https://github.com/cvra/robot-software/tree/master/can-io-firmware">Software</a> |
            <a href="https://github.com/cvra/can-bootloader">Bootloader</a>
        </li>
    </ul>
</p>

<a name="features"></a>

# Features

The CAN IO board is a small board with a dedicated MCU that has the following features:

 - CAN interface with communication over UAVCAN for IO control (read / write)
 - CAN bootloader for easy firmware update over the bus
 - 11 GPIOs exposing
     * Digital outputs, including timer channels for PWM
     * Digital inputs, including timer channels for pulse counting
     * Analog inputs (ADC)
 - Communication busses such as I²C and SPI
 - SWD connector for flashing and debugging, with UART exposed on the same connector

<a name="hardware"></a>

# Hardware

The board is made up of a 23x15mm double-sided PCB with 2 copper layers.
Schematics and layout are made with KiCad and <a href="https://github.com/cvra/can-io-board">available on Github</a>.
Two Molex picoblade connectors are used for CAN bus wiring in daisy chain.
GPIOs are exposed on the sides of the board by through-hole pins.

![CAN IO board](/images/technologies/3d-io-board.png)

<a name="software"></a>

# Application software

The software is very specific to the applications we might use this board for.
But we wrote a general purpose firmware to get basic IO control over UAVCAN, such as PWM output and digital inputs, [available on Github](https://github.com/cvra/robot-software/tree/master/can-io-firmware).
