Seat control for Range Rover, Rolls Royce and Bentley
======================================================
![](assets/IMG_0004.png)

Features
========
* Retrofit for irreparably damaged controller PCBs
  - The PCBs and the mounted microcontroller are irreparably damaged by battery acid from a leaking backup battery.
  - Our assembly reproduces the exact function without relying on a backup battery.
* No modifications to the vehicle required.
* The short wiring harness in the control unit can be plugged in directly.
* If the connectors on the wiring harness are heavily corroded, Wago connectors can alternatively be fitted and the wires clamped directly.
* Control of the 4 seat motors and the two exterior mirrors
* Supports both variants of the possible mirror joysticks (the "red" and "black" model) for the mirrors, including automatic detection
* Memory function in the flash of the modern microcontroller with virtually unlimited storage slots
* Extensive diagnostic options via a WLAN web interface
* 2x GPIO outputs and CAN interface for expansions in retromods

Planned future features
======================
* Control also via app using Bluetooth Low Energy
* Control of the sunroof or central locking via app
* Communication with additional peripherals via CAN for retromods

Hardware description
====================
* Based on the [SANS Gateway](SANS-Gateway.md)
* Modern relay matrix to drive up to two motors simultaneously
* Precision ADC for evaluating the potentiometers in the mirrors for storing position

Notes for installation in the Range Rover
=======================================
* The detection of the joystick type used occurs at power-on. Changing the joystick type while in operation can cause damage.
* Wiring toward the vehicle:
  - Due to the original somewhat unfavorable grounding routing, the connector fitted with four 6.3 mm flat contacts must be plugged in first in the vehicle wiring harness
  - This ensures sufficient grounding and avoids stray currents that can lead to damage of the PCB.