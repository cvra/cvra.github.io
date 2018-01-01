---
layout: page
title: CAN Motor Board
---

<div class="row">
<div class="large-6 columns">
    <p>
    The CAN motor board is a 31x16mm board we made to control a single DC motor over CAN bus.
    This is a core board we use in all our robots since 2015.
    We use it to control DC motors, motor pumps, and RC servos over CAN.
    </p>
    <h1>Contents</h1>
    <p>
        <ul>
            <li><a href="motor_board.html#features">Features</a></li>
            <li><a href="motor_board.html#hardware">Hardware</a></li>
            <li><a href="motor_board.html#software">Application software</a></li>
            <li><a href="motor_board.html#uavcan">UAVCAN messages</a></li>
            <li>Quick links:
                <a href="https://github.com/cvra/motor-control-board">Hardware</a> |
                <a href="https://github.com/cvra/robot-software/tree/master/motor-control-firmware">Software</a> |
                <a href="https://github.com/cvra/can-bootloader">Bootloader</a>
            </li>
        </ul>
    </p>
</div>
<div class="large-6 columns">
    <figure>
        <img src="/images/technologies/motor-board.jpg" alt="CAN motor boards on Debra's arm">
        <figcaption>
            <center>
                CAN motor boards used on the SCARA arm of Debra.
            </center>
        </figcaption>
    </figure>
</div>
</div>

<a name="features"></a>

# Features

The CAN motor board is a small board with a dedicated MCU that has the following features:

 - CAN interface with communication over UAVCAN for parameter/gain setting, setpoint sending, and feedback streaming
 - CAN bootloader for easy firmware update over the bus
 - Control a single DC motor with current sensing, allowing cascaded PID control (torque, velocity, and position)
 - H-bridge to drive the motor bidirectionally that supports driving a motor with up to 16.8V @ 6.6A continuously
 - Quadrature encoder interface for motor (5V tolerant)
 - Secondary quadrature encoder interface (5V tolerant) (eg. for separate odometry wheels)
 - Analog input for RC-servo control
 - Digital input for indexing, allows us to determine a reference location on a given axis that can be used for absolute positioning
 - Runs on 3 or 4 cell LiPo batteries
 - SWD connector for flashing and debugging, with UART exposed on the same connector

<a name="hardware"></a>

# Hardware

<div class="row">
<div class="large-6 columns">
    <p>
    The board is made up of a 31x16mm double-sided PCB with 4 copper layers.
    Schematics and layout are made with KiCad and <a href="https://github.com/cvra/motor-control-board">available on Github</a>.
    It fits on the back of a big RC servo and the BOM costs less than 35 USD.
    </p>
</div>
<div class="large-6 columns">
    <p><img src="/images/technologies/3d-motor-side.png" alt="CAN motor board 3D side view" /></p>
</div>
</div>

<div class="row">
<div class="large-6 columns">
    <p><img src="/images/technologies/3d-motor-front.png" alt="CAN motor board 3D front view" /></p>
</div>
<div class="large-6 columns">
    <p><img src="/images/technologies/3d-motor-back.png" alt="CAN motor board 3D back view" /></p>
</div>
</div>

<a name="software"></a>

# Application software

The CAN motor board runs firmware on a STM32F3 MCU (STM32F303CCT6 precisely) that we wrote using [ChibiOS RTOS](chibios.org).
Drivers for the ADC, quadrature encoder, and H-bridge were implemented on top of the ChibiOS HAL.
The [UAVCAN communication protocol](uavcan.org) is supported by the CAN motor boards.
All the code is [available on Github](https://github.com/cvra/robot-software/tree/master/motor-control-firmware).

The control software runs three cascaded PID loops for torque, velocity, and position.
Parameters can be read and set over UAVCAN.
We also implemented a [GUI interface for PID tuning](https://github.com/cvra/robot-software/tree/master/tools/pid-tuner).

<a name="uavcan"></a>

# UAVCAN messages

Each motor board has a unique human-readable name (e.g. right-wheel) that we can use to address it over UAVCAN by maintaining a key-value pair associating UAVCAN IDs with board names, just like DNS does for matching internet URLs with IP adresses.
Communication with the CAN motor boards can be any of the following kinds:

 - Service call to read and set the config,
 - Publish control setpoints, supports voltage, torque, velocity, and position setpoints,
 - Subscribe to feedback messages, supports streaming voltage, torque, velocity, position, external encoder, index position, and PID (setpoint/measured) values

<figure>
  <img src="/images/technologies/motor-board.jpg" alt="CAN motor boards on Debra's arm">
  <figcaption><center>CAN motor boards used on the SCARA arm of Debra.</center></figcaption>
</figure>
