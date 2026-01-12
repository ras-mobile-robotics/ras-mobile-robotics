---
layout: default
title: "Virtual Machine"
parent: Tutorials
sort: 2
---
# Clean colcon 
ROS2 logs might take a large amount of disk space over time.
Clean the workspace log folder by running: `colcon clean log`

# Expand VM Hard Disk
Expanding your disk on an Ubuntu 24.04 VM involves two main stages: increasing the "hardware" limit in VMware and then using the built-in GUI to claim that space for `/dev/sda2` (the disk that the Ubuntu VM uses).
---

## Phase 1: Increase Disk Size in VMware Player

You must perform these steps while the Virtual Machine is completely powered off.

1. **Shut down** your Ubuntu VM.
2. In the VMware Player library, select your VM and click **Edit virtual machine settings**.
3. Select **Hard Disk (NVMe)** or **Hard Disk (SCSI)** from the device list.
4. On the right-hand side, look for the "Capacity" section and click the **Expand...** button.
5. Enter the **new maximum disk size** (e.g., if you want 100GB total, enter 100).
6. Click **Expand**. Once you see the "Disk expansion successful" message, click **OK** and power on your VM.

---

## Phase 2: Resize `/dev/sda2` using GParted

Once Ubuntu boots up, it will see the extra space as "Unallocated," meaning it isn't assigned to any partition yet.

1. **Open GParted:** Search for "GParted" in your Ubuntu applications menu and launch it. (You will be prompted for your password).
2. **Locate the Partition:** Find **`/dev/sda2`** in the list. You should see a gray box labeled **Unallocated** immediately to the right of it.
3. **Resize the Partition:**
* Right-click on the **`/dev/sda2`** row.
* Select **Resize/Move**.
4. **Extend to Fill:** In the window that pops up, use your mouse to grab the **right edge** of the partition box and drag it all the way to the right until there is no gray space left.
5. **Confirm Resize:** Click the **Resize/Move** button at the bottom of that window.
6. **Apply Changes:** At this point, the change is only "pending." Click the **Checkmark icon** (✔) in the top toolbar to apply the operations.
7. **Wait for Completion:** Once the process finishes, click **Close**.

---

## Phase 3: Verification
To ensure your ROS 2 environment and system tools now have access to the full space, you can quickly check the disk status:

* Open the **"Disk Usage Analyzer"** (standard on Ubuntu) to see the new capacity.
* Alternatively, run `df -h /` in the terminal to see the updated size for your root directory.


# Essential Command Line Interface (CLI) Toolkit for ROS 2

This guide covers the fundamental tools you need to navigate, search, and manage remote sessions effectively.

---

## 1. SSH (Secure Shell)
SSH is used to log into a remote computer (like your TurtleBot 4) securely.

* **The Command:** `ssh username@ip_address`
* **Why use it?** You can't plug a monitor into a robot while it's driving. SSH lets you control it over WiFi.
* **Pro Tip:** If you see a "Host key verification failed" error, it's usually because the robot was re-imaged. Fix it with: `ssh-keygen -R [IP_ADDRESS]`.

---

## 2. Bash & Basic Commands
**Bash** (Bourne Again SHell) is the command-line interpreter you see in the terminal. It is the language you use to tell the OS what to do.

### Core Commands:
* `pwd`: **Print Working Directory**. Shows exactly where you are in the folders.
* `ls`: **List**. Shows files in the current folder. (Use `ls -la` to see hidden files).
* `cd [path]`: **Change Directory**. Move into a folder. `cd ..` moves up one level.
* `mkdir [name]`: **Make Directory**. Creates a new folder.
* `sudo`: **SuperUser Do**. Runs a command with "Admin" privileges.
* `cat [file]`: **Concatenate**. Displays the text inside a file in the terminal.

---
## 3. Useful Tools
The VM also has a bunch of CLI tools that makes dev lives easier. 

### 1. fd-find (`fd`)
`fd` is a simple, fast, and user-friendly alternative to the standard `find` command.

* **Quick Start:** `fd my_node`
* **Why it's better:** It ignores `.git` folders by default, uses colors, and is significantly faster than the built-in search.
* **Note:** On some systems, the command is `fdfind`.

---

### 2. ripgrep (`rg`)
`rg` is the world's fastest tool for searching **inside** files for specific text.

* **Quick Start:** `rg "Namespace"`
* **Why it's better:** If you have thousands of lines of code in your ROS workspace, `rg` will find the exact line of code you're looking for almost instantly. It respects your `.gitignore` so it won't search through build logs.

---

### 3. autojump (`j`)
`autojump` learns your habits so you can navigate your filesystem faster.

* **Quick Start:** `j my_robot_pkg`
* **How it works (Frecency):** It uses a "Frecency" algorithm (Frequency + Recency). It tracks which folders you visit most often and most recently. 
* Instead of typing `cd ~/colcon_ws/src/my_packages/subfolder/robot_logic`, you just type `j robot` and it "jumps" there.

---

### 4. tmux (Terminal Multiplexer)
`tmux` is essential for ROS development and SSH. It allows you to run multiple terminal windows inside one connection and keeps them running even if your WiFi drops. It also allows you to create mutliple terminal windows easily without having to SSH a new terminal every time.

#### Why it's critical for SSH/ROS:
If you are driving a robot via SSH and your WiFi blips, a normal session will die—and the robot might keep driving! If you use `tmux` (a terminal program), the process keeps running on the robot even if you lose connection.

#### Your Config Shortcuts:
Once you start tmux, press your **Prefix** key (`Ctrl+b`) then the key:

* `|` : Split the screen **Horizontally**.
* `-` : Split the screen **Vertically**.
* `c` : Create a **New Tab** (Window).
* `x` : Create a **New Tab** (Custom config).
* `s` : **Switch Sessions**. Switch between different tmux sessions.

### Attaching:
If your terminal closes, just log back in and type:
`tmux a`
This "attaches" you back to your last session exactly where you left off.
---

### Tools Mentioned in Other Guides:
* [fd-find GitHub](https://github.com/sharkdp/fd)
* [ripgrep (rg) GitHub](https://github.com/BurntSushi/ripgrep)
* [autojump (j) Documentation](https://github.com/wting/autojump)
* [tmux Wiki](https://github.com/tmux/tmux/wiki)
