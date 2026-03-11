# DrawBot CoreXY Controller & Plotter

## Overview

DrawBot is a custom, ESP32-based controller board and large-format CoreXY pen plotter.

This project was built to scale up the traditional desktop plotter experience. By moving to a custom ESP32 PCB running FluidNC, this iteration eliminates the need for multiple microcontrollers and messy wiring, wrapping WiFi control, SD card hosting, and heavy-duty motor driving into one clean package.

## Project Planning

A "living document" used to keep track of the ongoing planning, component sourcing, and build progress for this project can be found here:

* **[DrawBot Planning Spreadsheet](https://docs.google.com/spreadsheets/d/1DZCe1eKOiRGR80yqyA4uuKFQJ7K6CPcNbJUIT71Rvy4/edit)**

## Inspiration

The first iteration of this build was heavily inspired by the amazing [Plottermap project by Volzo](https://volzo.de/posts/plottermap/).

Volzo's original electronics used a dual-Arduino Nano setup running [GRBL](https://github.com/gnea/grbl/wiki), with a co-processor to handle Stallguard endstops. Since the exact frame specs weren't provided, I reverse-engineered my best guess for the physical construction. For this next-generation build, I am keeping that same physical frame but completely overhauling the electronics to a single-board ESP32 solution.

## The Machine (Hardware)

The physical plotter is designed for large-scale artwork:

* **Frame:** 1m x 1.5m V-Slot extruded aluminum with 3D printed joints.
* **Workspace:** Comfortably accommodates canvases and paper at least 24" x 40".
* **Kinematics:** CoreXY motion system driven by standard belts, pulleys, and idlers.
* **Motors:** 2x NEMA 23 stepper motors for slinging the heavy X/Y gantry, and 1x NEMA 17 for the Z-axis pen lift.
* **External Peripherals:** Support for physical limit switches, a dedicated 24V laser attachment port, and a UART expansion port for future upgrades.

## The Electronics (Controller)

The custom KiCad PCB is designed to be a plug-and-play brain for the machine:

* **Microcontroller:** ESP32 (DevKitC-32).
* **Firmware:** Runs [FluidNC](http://wiki.fluidnc.com/en/home) (a next-gen port of GRBL), which easily handles the CoreXY kinematics and adds native support for TMC SPI configuration.
* **Connectivity:** The ESP32 provides WiFi for remote monitoring and control via a FluidNC web UI.
* **Storage:** An onboard Micro SD card reader hosts the G-code project files locally so your computer doesn't have to stay plugged in for a 10-hour plot.
* **Stepper Drivers:** 3x TMC5160 StepStick modules. These easily provide the juice needed for the NEMA 23 motors.
* **Clean Wiring:** The stepper motor phases are routed out through standard RJ45 jacks, allowing you to use cheap, shielded Cat6 Ethernet cables to wire up the gantry.
* **Power:** Runs on a single 24V power supply (split into motor, laser, and logic domains), stepped down to 5V/3.3V using an MP1584 buck converter.

## Repository Contents

* `/drawbot.kicad_pro` - Main KiCad project file.
* `/*.kicad_sch` - Hierarchical schematic files (Power Delivery, Microcontroller, Stepper Drivers, IO Connectors).
* `/drawbot.kicad_pcb` - The physical PCB layout.
* `/libs/` - Custom footprint and symbol libraries used in the design.
* `/pdf/` - Exported PDF versions of the schematic and PCB layout for quick viewing.
