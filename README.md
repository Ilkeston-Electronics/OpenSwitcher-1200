# OpenSwitcher 1200
Amiga Microprocessor-controlled drive switcher for Amiga 600 and 1200 (possibly all Amigas actually, if correct resistor is removed) with DF1 internal drive support. Optional SMD small version[Gerbers not uploaded yet for SMD version]. If you want the A500 / A500 Plus version, see [OpenSwitcher HERE](https://github.com/Ilkeston-Electronics/OpenSwitcher)

# Disclaimer
Consider this beta. I have checked and rechecked the schematics and DRC rules-checked it. However, I have only built it on veroboard, and I am not perfect. I'm waiting for boards to come.

# [Latest firmware in releases HERE.](https://github.com/Ilkeston-Electronics/OpenSwitcher/releases)

![openswitcher1200_pcb](https://user-images.githubusercontent.com/89555920/133881179-3998639f-3dd8-40bd-beae-fb34b562b518.png)

# Description

There are lots of (very simple) Amiga drive switcher projects out there. This one intends to be a little different. Driven by logic and very simple code, designed to be user buildable and programmable, with no fancy programmers required. If you have USB, you can program this with a single file! Tested with A500 plus and kickstarts v2.04, v2.05, v3.1 and v3.1.4. Compatible with Pistorm (though pistorm has its own swapping system).

# Project Aims

To build a quality Amiga drive swapper out of "off the shelf" parts, easily available and easily programmable. Also have the internal drive functioning as DF1 via correct drive ID timing during switching.

# But why?
Our Amigas deserve the best. All other drive swapping projects I have seen have the following caveats... Drilling holes, fitting switches, programming PIC chips, fitting surface-mount components, internal drive not working... This project aims to address all those.
Amigas are very fussy about which drive we run from. DF1 is a bootable drive, but it isn't DF0. Which is sadly hard-coded into the software, leading to problems.

# Working Principle
How does this work?
OK... There are 2 lines we need to swap, which are SEL0 and SEL1. Simple enough...

This project uses an Adafruit Trinket M0 microcontroller.

This uses a relay so any signal is "pure", ie it is just like swapping the SEL points with solder or a switch. The SEL signal is passed through the relay just as it left the CIA.
This switching is controlled by the Trinket, which has levels corrected by the transistors, which then goes to the relays.

* Q2 and Q3 are sel0 / sel1 sensor lines. Only sel0 sensor line is used at the time of writing.
* Q1 base (and Q5 if fitted) is connected to relay coil Upon a base positive voltage, coil - GND goes low resistance, switching on the relay.
* J3 contains J_RST_LINE. If high, returns low to microcontroller. If using / testing without keyboard reset line connected, jumper to pin 1 to stop automatic rebooting and eeprom writing of microcontroller.
* JP1 is used as part of a latching relay system. Link points marked on board if using a latching relay

# We must disconnect one resistor on the Amiga.
Disconnect E595 as shown below... Then use jumper wire or pin or whatever to make your connection to JP2 on OpenSwitcher.
![openswitcher1200_1200connection](https://user-images.githubusercontent.com/89555920/133881800-5d6d2870-812e-49cd-81e0-e90c9588fb28.png)

# Construction Notes
On all headers and jumpers, the square pad is pin 1.
All connectors and descriptions: \


J1 - Power Supply. 5V and GND. On some boards (example Amiga 600) there are resistors directly underneath these pads you could take power from. Or even use Pogo pins if you are feeling adventurous. Or connect to floppy 5v. See J3 below if looking for alternative power.
* J1 - Pin 1=5v
* J1 - Pin 2=GND

SEL Lines
* J2 - Pin 1= SEL1 Point E595 on Amiga board. On some boards, this will just say 95. Remember the the resistor we disconnected? We connect a wire to the now free LOWER pad where the resistor used to be. Use a pogo pin? Little bit of wire? A header pin?
* J2 - Pin 2= SEL0 (Not required as it is taken from the drive port). Added "just in case". Leave disconnected.

JP3 - Latching relay jumper.
* 1--2 Not Used
* 2--3 Latching Relay fitted
* Unconnected - Non-latching relay fitted(default)

J3 - KB_Reset and power. This is designed to mimic the unused keyboard header on a Amiga 1200 rev 2B, which is CN17.
* J3 - Pin 3 - KB_Reset line. Fits to J3 pin 3 on Amiga 1200 rev 2B. Other Amiga 1200, connect to TP2 pin 9 (bottom far left of motherboard), Amiga 600, connect to U5 pin 63 (Gayle). There are other points, take a look at [AmigaPCB] for guidance.
* J3 - Pin 6 - Alternative 5V voltage input (take from floppy drive)
* J3 - Pin 7 - Alternative GND voltage input (take from floppy drive)

# Parts List - Large Through-hole version
* 1 x 34 pin (or 2x17) male pins, 2.54mm pitch.
* 1 x 34 pin (or 2x17) female header, 2.54mm pitch.
* Relay (see below)
* 4 (or 5) x KSP2222 or similar NPN BJT transistor
* 4 (or 5) 1/8 watt resistors (if you can find them. 1/4 watt if not).
* 1 (or 2) x 1n4148 or similar diode
* 1 x Adafruit Trinket M0 (recommended), though classic Trinket (3v and 5v are / will be supported)
* 2 x 2 pin header 2.54mm pitch
* 1 x 7 pin header 2.54mm pitch (only 3 pins are used. Soldering a single pin is never fun).
* 4 x DuPont cables female-soldered end (or female to male, to insert into keyboard connector pin 3, 5V, GND and maybe E595 too.
* 2 x 5 male pin header (2.54mm pitch supplied with Trinket)
* 2 x 5 female pin header (2.54mm pitch to plug Trinket in to, or solder Trinket into board)
* 1 x 68R resistor (Optional) 1/8 or 1/4 watt.

# Component Locations - Large Through-hole version
* DRIVE_CON - 1 x 34 pin (or 2x17) male pins, 2.54mm pitch.
* BOARD_CON - 1 x 34 pin (or 2x17) female header, 2.54mm pitch.
* RL1 - Relay (see below)
* D1, D2 - 1N4148 diode
* R1, R2, R3, R4, R5 - 1K resistor
* Q1, Q2, Q3, Q4, Q5 - KSP2222 npn BJT transistor(600mA, 40v)
* P1 - Adafruit Trinket M0
* J1 - 1 x 2 header pin
* J2 - J1 - 1 x 2 header pin
* JP1 - 1 x 3 header pin (optional)
* R6 - Optional - This replaces the disconnected resistor we removed connecting E595. 68R is the value. In practice, this can just be bridged. Up to you!

# Parts List - Slim SMD version
* 1 x 34 pin (or 2x17) male pins, 2.54mm pitch.
* 1 x 34 pin (or 2x17) female header, 2.54mm pitch.
* Relay (see below)
* 4 (or 5) x BC817-40 npn BJT transistor
* 4 (or 5) 1Kohm 0402 0.06w resistor
* 2 x 1n4148 or similar diode
* 1 x Adafruit Trinket M0 (recommended), though classic Trinket (3v and 5v are / will be supported)
* 1 x 2 pin header 2.54mm pitch
* 1 x DuPont cable female-soldered end (or female to male, to insert into keyboard connector pin 3
* 2 x 5 male pin header (2.54mm pitch supplied with Trinket)
* 2 x 5 female pin header (2.54mm pitch to plug Trinket in to, or solder Trinket into board)
* 1 x 68R resistor (Optional) 1/8 or 1/4 watt.

# Component Locations - Slim SMD version
* DRIVE_CON - 1 x 34 pin (or 2x17) male pins, 2.54mm pitch.
* BOARD_CON - 1 x 34 pin (or 2x17) female header, 2.54mm pitch.
* K1 - Relay (see below)
* D1, D2 - 1N4148 diode
* R1, R2, R3, R4, R5 - 1K 0402 0.06w resistor
* Q1, Q2, Q3, Q4, Q5 - BC817-40 npn BJT transistor(500mA, 45v)
* P1 - Adafruit Trinket M0
* J1 - 1 x 2 header pin
* JP1 - 1 x 3 header pin (optional)
* R6 - Optional - This replaces the disconnected resistor we removed connecting E595. 68R is the value. In practice, this can just be bridged. Up to you!

# R6 - 68R Resistor - This replaces the 68R resistor we removed from the Amiga motherboard. You can just link this and it will work. But I'm not sure I would advise it. For the cost of a resistor, fit one.

# Supported relays
* Any 5v relay that will fit into the footprint
# Latching relays (need D2, R5 and Q5 fitting)
* Panasonic TX2-L-5V / TX2-L2-5V
* Hongfa HFD3/005-L2
* Kemet EE2-5T
# Non-Latching relays
* TE Connectivity IM03TS
* TE Connectivity 1-1462038-3
* Omron G6K2P5DC
* Omron G6K-2P-Y 5DC
* Hongfa HFD4/005

# Notes about relays
* You can either use a latching or non-latching relay. Please pay attention to the following points.
* If using a non-latching relay, no need to fit D2, R5 or Q5. JP1 should remain open and unlinked.
* If using latching relay, D2, R5 and Q5 is required to set / reset relay. JP1 should be linked where indicated on the board.

# User Guide
* Remove floppy ribbon cable (34 way IDC). Fit floppy cable into DRIVE_CON on OpenSwitcher 1200
* Fit KB_RST wire in your preferred way, as described earlier in this document.
* Connect power. Either from floppy drive (5v) or elsewhere.
* Ensure Trinket is NOT PLUGGED IN to OpenSwitcher or CIA is removed from OpenSwitcher (I haven't had any failed CIA chips during flashing, but better to be safe)
* Download software and plug in Trinket. A drive will open
* Drag and drop downloaded UF2 file to this drive. Wait for lights to go out on device then disconnect.
* Fit Trinket onto OpenSwitcher as the image on the OpenSwitcher board
* Complete

# Usage
* Operating Guide

* START
* Upon power on, unconnected (ie plugged into PC), red light will be flashing. This means successfully flashed... Please plug into OpenSwitcher.
* This will ONLY HAPPEN ONCE on FIRST INSTALL INTO OpenSwitcher! Upon Amiga power-on, white LED should flash 3 times. Identify relay, program virtual Eeprom with default values. When white LED flash 3 times This indicates setup is correct.
* Upon Green LED, drive ID successful, booting in a stock unswapped condition
* Upon Blue LED, drive ID successful, booting in swapped condition.
* LED off after 8 seconds approx = OpenSwitcher is asleep.
* Upon Amiga RESET, pink LED, reboot successful, go back to start.
* If Amiga RESET is HELD for approx 3 seconds, White LED will blink 3 times. If in a swapped condition, will become unswapped. If unswapped, will be swapped. Stored to Eeprom.
* That is all

# DISCLAIMER 
* You only have yourself to blame. You decide to make and fit this. I will not take responsibility for any damage, however caused. Though I will try to help.

