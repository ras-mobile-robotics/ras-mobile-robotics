---
layout: default
title: "VM Setup"
sort: 1
---

```danger
## Under Construction
This guide is currently being updated for the **Spring 2026** semester. Please note that some text and terminal commands may change before the final release.
```

This guide covers the configuration of the your **Host Computer**. The  This ensures stable communication in a shared classroom network environment. 

This lab bridges the gap between basic setup and custom robot control. By using the official ROS 2 Jazzy documentation, you learn how to navigate the primary resource for ROS 2.

# LAB 1: VM Setup and ROS Pub/Sub Lab
The Host computer will run a Ubuntu 24.04 VM using VMWare. ROS 2 Jazzy FastDDS (along with a lot of useful tools) is installed in the Ubuntu VM.

## PART 1: VM Setup

### 1. Importing the VM
1.  Open VMware (Workstation, Player, or Fusion).
2.  Select **File > Open** and choose the provided `.ova` or `.ovf` file.
3.  **CRITICAL:** When you first start the VM, VMware will ask: *"This virtual machine might have been moved or copied."*
    * **Select: "I COPIED IT"**
    * *Why?* This generates a unique MAC address and Machine ID so your VM doesn't conflict with others on the network.
    

### 2. Optimizing Hardware Settings
Before clicking "Power On," select **Edit Virtual Machine Settings** and apply these optimizations:


| Setting | Recommended Value | Reason |
| :--- | :--- | :--- |
| **Memory (RAM)** | **4GB - 8GB** | Essential for Ubuntu 24.04 and Gazebo performance. |
| **Processors** | **2 to 4 Cores** | ROS 2 handles many nodes in parallel. |
| **Graphics** | **Accelerate 3D Graphics (ON)** | Critical for Rviz and Gazebo visualization. |
| **Graphics Memory**| **2GB** | Prevents GUI flickering in simulations. |
| **Network Adapter**| **Bridged (Automatic)** | VM needs its own IP on the classroom router. |

---

### 3: First Boot

> **_NOTE:_**  Make sure you change the display settings to match the resolution and scaling required for your monitor.

Once you log in, run this script on your Desktop to configure your VM. It will automatically set your hostname to `ubuntuvm{NUMBER}`.

Open a terminal using the Tilix application on the Ubuntu global menu. 
```bash
./vm_setup/first_boot.sh
```

---

### 4: VM ROS2 Setup: Set Namespace and Discovery Server
These steps allow your personal computer to "see" the robot on the classroom router, when the robot is running.

#### Step 1: Run Discovery Script

Run the configuration script provided in your setup folder:

```bash
. ~/vm_setup/configure_discovery.sh
```

Follow the interactive prompts carefully:

* **ROS_DOMAIN_ID [0]:** `0`
* **Discovery Server ID [0]:** `XXXXXXXXXX` (Matches your robot ID)
* **Discovery Server IP:** `YYYYYYYY` (The Static IP of your TurtleBot)
  * ip addr show wlan0
  * ip -4 addr show wlan0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
* **Discovery Server Port [11811]:** Press **Enter** (Default)
* **Next Action:** Enter `d` for **Done**.

#### Step 2: Finalize Environment

Source your bash configuration and restart the ROS 2 daemon to clear any old discovery caches:

```bash
source ~/.bashrc
ros2 daemon stop; sleep 5; ros2 daemon start
```

# Learn to use VMWare and Ubuntu
Take some time to learn how to use VMWare and the Ubuntu OS.

## Shutdown
In VMware, choosing how to close your VM is the difference between a clean exit and a corrupted lab.
### Power Options Compared

| Option | What happens | Best for... |
| --- | --- | --- |
| **Suspend** | "Freezes" the VM state to your disk. | Taking a quick break. |
| **Power Off** | "Pulls the plug" instantly. | **Emergency only** (if the VM is frozen). |
| **Shut Down** | Signals Ubuntu to save and exit. | **Everyday use.** (Prevents file corruption). |

---

### How to Shut Down Ubuntu

Use these steps to ensure your work is saved and the virtual "disk" stays healthy.

#### **The Method (Same for both)**

* **GUI:** Top-right corner icon → **Power Off / Log Out** → **Power Off**.
* **Terminal:** Open with `Ctrl`+`Alt`+`T` and type: `sudo shutdown now`.

#### **The VMware Interface**

::: tabs

@tab **Windows (VMware Player)**

1. Click the **Player** menu (top left).
2. Go to **Power**.
3. Select **Shut Down Guest**.
* *Note: If you click "Power Off," it's like pulling the plug. Always look for "Guest" in the name.*



@tab **Mac (VMware Fusion)**

1. Click the **Virtual Machine** menu in the Mac Menu Bar.
2. Select **Shut Down**.
* *Tip: Holding the **Option (⌥)** key changes this to "Force Shut Down" (Power Off), which you should avoid unless stuck.*


:::

## Setup SIM
1. Open a terminal and run 
```bash
set-ros-env sim
```
2. Close ALL the open terminals before you continue so that the new changes are in effect.
> This a wrapper script that helps you setup the necessary env variables to work with a robot or with a simulator.

## PART 2: ROS 2 Publisher and Subscriber
This lab bridges the gap between basic setup and custom robot control. By using the official **ROS 2 Jazzy** documentation, students learn how to navigate the primary resource they will use for the rest of their careers.

**Objective:** Follow the industry-standard tutorial to create a basic "Talker" and "Listener."

1. **The Tutorial:** Open the official [ROS 2 Jazzy Tutorials](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html) page.
2. **The Task:** Complete the tutorials:
   1. Using colcon to build packages
   2. Creating a workspace
   3. Creating a package
   4. Writing a simple publisher and subsccriber (Python)
3. **Validation:** Run both nodes in separate terminals. You should see "Hello World" messages streaming across.

> Tip: Use the "Beginner: CLI Tools" to have a more in-depth understanding of the ROS2 Design and ROS2 CLI tools. 
---