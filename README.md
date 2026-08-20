# 6-Axis Robotic Arm (Work in Progress)

An open-source, 6-DOF robotic arm featuring a custom mixed-architecture control board. The system offloads inverse kinematics (ROS 2 / Python) from a host laptop to an onboard Raspberry Pi Pico W analyzing encoder data and generating step signals.

---

## Hardware Overview

### Mechanical
The physical structure consists of a custom 3D printed robotic arm with one meter span. Joints are driven with Nema stepper motors through gear ratios and pulleys, controlling rotation and movement of arm. Motors are to be guided through sensor inputs (limit switches, MT6701) and controlled via Nema Stepper Motors and precise gear ratios.
<p align="center">
  <img src="docs/robotic-arm.jpg" alt="Physical Arm Structure" width="600"/>
</p>

### Electrical
Electronics consist of a power supply unit (connected to a wall outlet) sent to a PCB that actively controls the robotic arm through signals to the stepper motors and data from encoders and limit switches mounted near the stepper motors on the physical arm.
<p align="center">
  <img src="docs/arm-pcb.jpg" alt="Assembled PCB" width="600"/>
</p>

The printed circuit board handles logic shifting via 74HCT245, limit switch & encoder monitoring, and I2C multiplexing. It houses pin headers for reusability of all boards in case of PCB failure. The board has thermal reliefs on 24V power pours, decoupling, and pull-up resistors for limit switch activation.
<p align="center">
  <img src="docs/pcb-layout.png" alt="PCB Layout" width="600"/>
</p>

---

## System Architecture

### Hardware Stack
*   **Microcontroller:** Raspberry Pi Pico W (RP2040)
*   **Power:** Mean Well LRS-200-24 (24V, 8.8A), DFR0831 Buck Converter (24V to 5V logic)
*   **Actuators:** 
    *   3x STEPPERONLINE Short Body NEMA 17 (Driven via A4988)
    *   1x Leadshine 42CME06 Closed-Loop (Driven via TMC2209)
    *   2x Leadshine iCL57-23 (Integrated Drivers)
*   **Feedback:** MT6701 Magnetic Encoders (I2C) & Leadshine Quadrature Encoders

### Software Stack (In Progress)
*   **Host (Laptop):** ROS 2, Python, MoveIt 2 (Inverse Kinematics, Trajectory Planning, URDF/Xacro modeling).
*   **Firmware (Pico W):** C++ (Pico SDK). 
    *   **Core 0:** Serial communication (micro-ROS/Custom protocol) & Emergency Stop hardware interrupts.
    *   **Core 1:** PID closed-loop feedback processing.
    *   **PIO:** Mathematically precise step/direction pulse generation for 6 axes.

---

## Development Roadmap

- [x] Mechanical Design and Manufacturing
- [x] PCB Design and Manufacturing
- [x] Finalize BOM and Sourcing
- [x] **[Current]** ROS 2 URDF, RViz2, and Gazebo simulation
- [ ] C++ Firmware: Step generation, and analyzing encoder data
- [ ] Communication between laptop and Pico (micro-ROS)
- [ ] Full integration of mechanical, electrical, and software components

---
