SANS Gateway
============

![](assets/IMG_9896.png)

Overview
========

Features
--------
* Adapting CAN messages in an existing system. Typical use cases include
  - Adjusting the displayed speed on the speedometer with a configurable factor
  - Controlling additional accessories such as headlights or light bars depending on CAN bus state
  - Controlling external components via GPIO depending on speed or engine RPM
  - Adapting instrument cluster displays for modified vehicles, e.g. in motorsports
* Forwarding CAN messages over other interfaces such as Wi‑Fi, Bluetooth Low Energy or USB
* Gateway configuration via Wi‑Fi in the web browser
* High-quality, compact plastic enclosure (118mm x 78mm x 23mm) with crimp-friendly Molex Mini-Fit Jr. connectors
* Internally expandable via 2.54mm pin headers, with enough space for a second PCB inside the enclosure: [picture of the opened unit](assets/gateway-open.jpg)

Specifications
--------------
* ESP32‑C6 Mini H4 with 4MB flash and extended temperature range up to 105°C
* Two CAN buses, each up to 1 Mbit/s
* 9x digital outputs, switched to ground up to 500mA, available on 2.54mm pin headers and Molex connector (one pin shared with digital input via solder jumper)
* 4x digital inputs, up to +32V, on 2.54mm pin headers and Molex connector
* Internal I2C bus for displays and extensions on pin header and JST‑XH connector
* Nano‑ESP32 compatible IO header (2.54mm pitch)
* USB for programming, debugging and communication with another system, e.g. in tester mode.
  - USB is available either via a fitted Mini‑USB jack (population option) or as contacts for a TC‑2050 USB cable.
  - The device is an ACM (serial) device that allows high speeds (up to 5 Mbit according to the datasheet) but behaves software-wise like RS‑232 and works with standard OS drivers everywhere.
* Power supply 8–32V

SANS Tester
-----------
The SANS Tester is a variant of the SANS Gateway with additional features:
* Mini‑USB jack populated
* DIP switch for configuring CAN termination
* 128x32 pixel OLED display for status information, etc.

Software
--------
* ESP32RET: Powerful open source project for CAN testers [Download](https://github.com/sans-ltd/ESP32RET/tree/sans_tester)

Hardware Documentation
======================

Hardware Features
-----------------
* Hardened power supply
  - Input voltage range 8V to 32V (VBAT)
  - Pads for a filter inductor for special requirements
  - Fuse, reverse‑polarity protection and overvoltage protection
  - High-efficiency DCDC converters for 5V and 3.3V
* 2x automotive‑grade CAN transceivers
  - SN65HDV230
  - Standby and wakeup capable
  - With additional ESD protection
* 9x GPIO OUT (OUT1–OUT9)
  - ULN2804, loadable up to 500mA per pin, voltage tolerant to 32V
  - Switch inverted as open collector to GND (HIGH -> OUTx pulled to GND, LOW -> floating)
  - Flyback diode to the supply
  - WARNING: OUT9 is active during the boot process because of a pull‑up resistor used for boot configuration
* 4x GPIO IN (IN3, IN4, IN6, IN7)
  - ULN2804 as voltage protection up to 32V
  - Switch inverted as open collector to GND
  - Therefore the ESP internal pull‑ups must be enabled on the corresponding GPIOs
  - INx >= 3.3V -> GPIO reads LOW
  - INx open or GND -> GPIO reads HIGH
* 2x ULN2804 switching stages for extensions
  - Available on the expansion headers but not connected directly to the ESP32
  - IN1 (ULN input) is connected to GPIO11 (ULN output)
  - IN2 (ULN input) is connected to GPIO10 (ULN output)
  - Because an ESP32‑C6 Mini is used, GPIO10 and GPIO11 are not usable on the MCU
* USB interface
  - Serial ACM communication up to 5 Mbit/s for firmware upload, logging or data
  - JTAG endpoint enables debugging via OpenOCD
  - GPIO16 and GPIO17 are used for CAN1, therefore there is no serial UART console
  - By setting the fuse bit `UART_PRINT_CONTROL` to `1`, even the rarely used bootloader output is disabled

Solder Jumper Assignments
-------------------------

| Jumper | Function |
| ------ | -------- | 
| JP1    | Termination of CAN 0, only on the Gateway |
| JP2    | Termination of CAN 1, only on the Gateway |
| JP3    | Enables pull‑up resistor to 5V for OUT3, e.g. for RS‑232 communication |
| JP4    | Enables pull‑up resistor to 5V for OUT2, e.g. for RS‑232 communication |
| JP5    | J1 pin 3 is an output (OUT4) |
| JP6    | J1 pin 3 is an input (IN4) |
| JP7    | Enables pull‑up resistor to 5V for OUT8, e.g. for RS‑232 communication |
| JP8    | To bridge the EMI suppression choke FL1 when it is not populated |
| JP9    | Set together with JP8 |
| JP10   | Connects IN2 to GPIO14. Provides an additional input, but disables I2C when set |
| JP11   | Connects USB to the NanoESP header. Populate only if USB is actually used there, otherwise EMC issues may occur |
| JP12   | Set together with JP12 |
| R22    | To break Tx on CAN 0 |
| R23    | To break Rx on CAN 0 |
| R24    | To break Tx on CAN 1 |
| R28    | To break Rx on CAN 1 |
| R291   | To break the standby control of both CAN transceivers |
| R292   | Populate to keep the CAN transceivers permanently in standby |


Connector Pinout
---------------

### Top view of the Gateway
![](assets/gateway_connectors.svg)

### Connector J1 (12‑pin Molex Mini‑Fit Jr.)
| Pin | Function |
| --- | -------- |
| 1   | Supply voltage VBAT 8–32V |
| 2   | Ground |
| 3   | IN4 or OUT4, depending on JP5 and JP6 |
| 4   | OUT2 |
| 5   | CAN1 High |
| 6   | CAN1 Low |
| 7   | +5V output |
| 8   | IN3 |
| 9   | OUT3 |
| 10  | OUT1 |
| 11  | CAN0 High |
| 12  | CAN0 Low |

### Connector J2 (6‑pin Molex Mini‑Fit Jr.)
| Pin | Function |
| --- | -------- |
| 1   | Supply voltage VBAT 8–32V |
| 2   | Ground |
| 3   | OUT5 |
| 4   | OUT7 |
| 5   | OUT6 |
| 6   | IN7 |

### Connector J3 (4‑pin Molex Mini‑Fit Jr.)
| Pin | Function |
| --- | -------- |
| 1   | OUT8 |
| 2   | IN8 |
| 3   | OUT9 |
| 4   | +5V output |

Internal Pin‑Header Assignments
--------------------------------

### PCB positions
![](assets/SANS-Gateway-pcb-bottom-V04.png)

### J20 and J21 nanoESP32‑C6 connector
| Connector | Pin | MCU function | Function in the SANS Gateway |
| --------- | --- | ------------ | --------------------------- | 
| J20       |   1 |              | GND                         | 
| J20       |   2 |              | 3.3V output                 |
| J20       |   3 | RESET        |                             |
| J20       |   4 | GPIO4        | IN8                         | 
| J20       |   5 | GPIO5        | IN7                         | 
| J20       |   6 | GPIO6        | Standby control of CAN transceivers, HIGH = standby, LOW = active |
| J20       |   7 | GPIO7        | OUT1                        | 
| J20       |   8 | GPIO0        | IN4                         | 
| J20       |   9 | GPIO1        | IN3                         | 
| J20       |  10 | GPIO8        | I2C Clock                   |
| J20       |  11 |              | ULN2804 output for IN2 ( = GPIO10 on the nanoESP) | 
| J20       |  12 |              | ULN2804 output for IN1 ( = GPIO11 on the nanoESP) |  
| J20       |  13 | GPIO12       | USB D-                      | 
| J20       |  14 | GPIO13       | USB D+                      |  
| J20       |  15 |              | 5V output                   |
|           |     |              |                             | 
| J21       |   1 |              | GND                         | 
| J21       |   2 | GPIO2        | CAN0 Tx                     |
| J21       |   3 | GPIO3        | CAN0 Rx                     |
| J21       |   4 | GPIO16       | CAN1 Tx                     | 
| J21       |   5 | GPIO17       | CAN1 Rx                     | 
| J21       |   6 | GPIO15       | OUT8                        |
| J21       |   7 | GPIO23       | OUT7                        |
| J21       |   8 | GPIO22       | OUT6                        |
| J21       |   9 | GPIO21       | OUT5                        |
| J21       |  10 | GPIO20       | OUT4                        |
| J21       |  11 | GPIO19       | OUT3                        |
| J21       |  12 | GPIO18       | OUT2                        |
| J21       |  13 | GPIO9        | OUT9, WARNING: active during boot |
| J21       |  14 |              | GND output                  | 
| J21       |  15 |              | 5V output                   |  

### J22 I2C and CAN buses
| Pin    | MCU function | Function in the SANS Gateway |
| ------ | ------------- | --------------------------- | 
|      1 | GPIO14        | I2C Data                    |
|      2 | GPIO8         | I2C Clock                   |
|      3 |               | CAN0 Low                    |
|      4 |               | CAN0 High                   |
|      5 |               | CAN1 Low                    |
|      6 |               | CAN1 High                   |
|      7 |               | GND                         |
|      8 |               | 3.3V output                 |

### J23 Inputs via ULN2804
| Pin    | MCU function | Function in the SANS Gateway |
| ------ | ------------- | --------------------------- | 
|      1 |               | IN1 -> J20 12               |
|      2 |               | IN2 -> J20 11               |
|      4 | GPIO0         | IN4                         |
|      3 | GPIO1         | IN3                         |
|      5 | GPIO5         | IN7                         |
|      6 | GPIO4         | IN8                         |
|      7 |               | GND                         |
|      8 |               | 5V output                   |
|      9 |               | 3.3V output                 |
|     10 |               | VBAT output after reverse‑polarity protection |

### J24 Outputs via ULN2804
| Pin    | MCU function | Function in the SANS Gateway |
| ------ | ------------- | --------------------------- | 
|      1 |               | GND                         |
|      2 | GPIO7         | OUT1                        |
|      3 | GPIO18        | OUT2                        |
|      4 | GPIO19        | OUT3                        |
|      5 | GPIO20        | OUT4                        |
|      6 | GPIO21        | OUT5                        |
|      7 | GPIO22        | OUT6                        |
|      8 | GPIO23        | OUT7                        |
|      9 | GPIO15        | OUT8                        |
|     10 | GPIO9         | OUT9                        |
|     11 |               | Ground input                |
|     12 |               | 5V output                   |
|     13 |               | VBAT output after fuse, before reverse‑polarity protection, e.g. for flyback diodes |
|     14 |               | VBAT input                  |
|     15 |               | 3.3V output                 |

### OLED Display
| Pin    | MCU function | Function in the SANS Gateway |
| ------ | ------------- | --------------------------- | 
| J4   1 | GPIO14        | I2C Data                    |
| J4   2 | GPIO8         | I2C Clock                   |
| J6     |               | 3.3V output                 |
| J7     |               | GND                         |
