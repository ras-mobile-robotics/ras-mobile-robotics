---
layout: default
title: TurtleBot 4 
parent: Tutorials
sort: 4
---
# TurtleBot 4 Hardware & Software Reference

The **TurtleBot 4** is the next-generation robotics platform for learning and development, built on the **iRobot® Create® 3** educational robot and powered by a **Raspberry Pi 4B**. This reference covers both the Standard and Lite models using the **ROS 2 Jazzy Jalisco** software stack.

---

## 1. Hardware Overview

The TurtleBot 4 is available in two configurations. While both share the same mobile base and compute, they differ in sensor payload and user interface capabilities.



### Core Technical Specifications
| Feature | TurtleBot 4 Lite | TurtleBot 4 Standard |
| :--- | :--- | :--- |
| **Dimensions** | 342 x 339 x 192 mm | 342 x 339 x 351 mm |
| **Weight** | 3.3 kg | 3.9 kg |
| **Max Payload** | 9 kg (Default) / 15 kg (Custom) | 9 kg (Default) / 15 kg (Custom) |
| **Max Speed** | 0.31 m/s (Safe) / 0.46 m/s (Raw) | 0.31 m/s (Safe) / 0.46 m/s (Raw) |
| **LIDAR** | RPLIDAR A1 | RPLIDAR A1 (Standard) / A2 (Optional) |
| **Camera** | OAK-D-Lite | OAK-D-Pro (IR/Active Stereo) |
| **User Interface** | 2 LEDs | OLED Display, 4 Buttons, 4 LEDs |
| **Battery** | 26 Wh Li-Ion (14.4V) | 26 Wh Li-Ion (14.4V) |

### User Power Output
* **Standard Model:** VBAT @ 1000mA, 12V @ 1000mA, 5V @ 2000mA, 3.3V @ 500mA.
* **Lite Model:** VBAT @ 1.9A (14.4V nominal). 5V and 3.3V available via Raspberry Pi GPIO.

## 2. Software Stack (ROS 2 Jazzy)

The TurtleBot 4 software environment is built for **Ubuntu 24.04 LTS** and **ROS 2 Jazzy Jalisco**.

### Core Requirements
* **OS:** Ubuntu 24.04.5 LTS (Noble Numbat).
* **Firmware:** iRobot® Create® 3 Firmware version **I.0.0** is required for Jazzy compatibility.
* **RMW Implementation:** FastRTPS (standard) or CycloneDDS.

### Key ROS 2 Packages
* **`turtlebot4_base`:** Manages the hardware interface (OLED, buttons, and Raspberry Pi/Create 3 communication).
* **`turtlebot4_bringup`:** Launch files for all sensors (LIDAR, OAK-D) and robot nodes.
* **`turtlebot4_navigation`:** Optimized Nav2 parameters using the **MPPI Controller** and SLAM Toolbox configurations.
* **`turtlebot4_description`:** URDF and mesh files for the physical robot model.
* **`turtlebot4_simulator`:** Simulation support for **Gazebo Harmonic**.

---

## 3. LED Ring Status (Create® 3 Base)

The LED ring on the mobile base provides the primary visual feedback for the robot's system state.

| Color & Pattern | Meaning | Action Required |
| :--- | :--- | :--- |
| **Solid White** | **Ready.** System is idle and connected. | None. You can now run ROS 2 nodes. |
| **Circling White** | **Booting.** Application is starting up. | **Wait.** This can take 2–5 minutes. |
| **Spinning Blue** | **AP Mode.** Robot is a Wi-Fi hotspot. | Connect your laptop to `Create-XXXX`. |
| **Solid Cyan** | **AP Connected.** Device is linked to Robot AP. | Complete the web setup at `192.168.10.1`. |
| **Pulsing Green** | **Charging.** Battery is filling. | None. Leave on dock until solid green. |
| **Solid Red** | **System Error.** Motor stall or cliff sensor. | Clear obstacles or debris from the wheels. |
| **Pulsing Red** | **Battery Low.** Needs charging. | Manually place the robot on the dock. |
| **Spinning Red** | **Critical Error.** Initialization failed. | **Hard Reboot.** Hold Power for 20 seconds. |

---

## 4. Power Management

The TurtleBot 4 uses two systems: the **Raspberry Pi 4** and the **Create® 3 Base**. Correct shutdown is essential to prevent SD card corruption.

### Powering ON
* **Docking (Best):** Place the robot on the powered Charging Dock; it will auto-boot.
* **Manual:** Press the **Power Button** on the Create® 3 base once.

### Powering OFF
1.  **Shutdown the Pi:** Use the terminal to run `sudo shutdown now`.
2.  **Wait:** Watch the Raspberry Pi's green LED; wait until it stops flickering.
3.  **Shutdown the Base:** Press and hold the **Power Button** on the Create® 3 base for **12 seconds** until you hear the descending chime.

---

## 5. Network Configuration (AP Mode)

If you need to update Wi-Fi credentials or change the network without a router, use Access Point (AP) mode:

1.  **Enter Configuration:** Press and hold **Buttons 1 and 2** simultaneously until the LED ring turns **Solid Yellow**.
2.  **Activate AP Mode:** Press and hold **Buttons 1 and 2** again until the ring begins **Spinning Blue**.
3.  **Connect:** On your PC, connect to the Wi-Fi network `Create-XXXX`.
4.  **Web UI:** Navigate to `192.168.10.1` in your browser to configure settings.

---

## 6. User Interface (Standard Model Only)

The Standard model features a dedicated UI board on the top plate.

* **OLED Display:** Provides real-time data including Robot Name, IP Address, Battery Percentage, and ROS Domain ID.
* **Button 1:** Default mapped to the E-Stop.
* **Button 2:** Default mapped to "Return to Dock".
* **Buttons 3 & 4:** User-programmable via the `/joy` topic.

---

## 7. Official Documentation & Links

* **[TurtleBot 4 User Manual (Official)](https://turtlebot.github.io/turtlebot4-user-manual/)**
* **[Jazzy Software & Navigation Guide](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html)**
* **[iRobot® Create® 3 Education Portal](https://edu.irobot.com/what-we-offer/create3)**
* **[Luxonis OAK-D-Pro Tech Specs](https://docs.luxonis.com/projects/hardware/en/latest/pages/oak-d-pro.html)**
* **[TurtleBot GitHub Repositories](https://github.com/turtlebot)**