# Micromouse — Autonomous Maze-Solving Robot

**🥈 2nd Place — IEEE ZSB RAS Chapter PCB Design Track, Final Project**

An autonomous maze-solving micromouse robot, designed end-to-end in Altium — from schematic
and PCB layout through calculations and documentation — as the capstone project for the IEEE
Zagazig Student Branch, Robotics & Automation Society chapter's PCB Design track.

---

## Overview

The robot must detect obstacles, plan its path, and adapt to a maze autonomously, while staying
within a strict **11cm × 11cm** PCB footprint. The design is split across **two stacked PCB
floors** to fit the size constraint while physically separating noise-sensitive sensor circuitry
from higher-current motor/power circuitry.

## Key Components

| Component | Role |
|---|---|
| ESP32-C3 Super Mini | Microcontroller — 13 GPIOs, exact fit for this design |
| 3× VL53L0X | Time-of-Flight distance sensors (front/left/right) |
| MPU6050 | 6-axis IMU |
| DRV8833 | Dual motor driver (bare IC, HTSSOP-16) |
| 2× N20 micro gear motors | With quadrature encoders |
| 0.96" OLED (SSD1306) | Status display + battery percentage |
| 2× 3.7V LiPo cells (series) | 7.4V, 1000mAh power source |

## Architecture

- **Bottom floor:** Power input, fuse + reverse-polarity protection, 5V regulation, MCU, motor
  driver, motors
- **Top floor:** All I2C sensors (3× ToF, IMU, OLED) — connected to the bottom floor via a
  6-pin interconnect
- **I2C bus:** Shared by 5 devices; ToF sensors individually addressed via XSHUT sequencing at
  boot (one sensor hardwired to 3.3V keeps the default address, the other two are reassigned in
  firmware)
- **Noise mitigation:** Physical floor separation, motor suppression capacitors, local
  decoupling at every IC, and a solid ground plane on both floors

## Repository Structure

```
hardware/   → Altium project (schematics, PCB layout, 3D)
docs/       → Bill of Materials, full calculations (trace widths, power budget,
              MCU justification, battery lifetime), presentation
media/      → Schematic/PCB/3D screenshots and project photos and videos
```

## Documentation

- 📐 [Bill of Materials](docs/Micromouse_BOM.pdf)
- 🧮 [Calculations — Trace Width, Power, MCU Justification, Battery Lifetime](docs/Micromouse_Calculations.pdf)
- 🎞️ [Presentation](docs/Team_A.pdf)

## Design Highlights

- Every PCB trace sized against the IPC-2221 standard with calculated safety margins
- Full component-by-component power budget, datasheet-sourced wherever possible
- Zero ERC / zero DRC errors across both floors
- Space-optimized: bare motor-driver IC instead of breakout module, edge-notch motor mounting
  to reduce stack height, compact MCU module chosen for an exact GPIO-count fit

## Team

- Abdallah Ahmed
- Mohamed Hany
- Malak Mahdi

## Acknowledgments

Thank you to the IEEE ZSB RAS chapter leadership for the guidance and structure throughout this
track, and to everyone who made this competition possible.