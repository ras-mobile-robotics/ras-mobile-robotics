---
layout: default
title: "Robot Setup"
sort: 3
---

```danger
## Under Construction
This guide is currently being updated for the <strong>Fall 2025</strong> semester. Please note that some screenshots or terminal commands may change before the final release.
```

This guide covers the configuration of the your **Host Computer** and a **TurtleBot 4** using the Discovery Server protocol. This ensures stable communication in a shared classroom network environment.

## Network Connectivity

Follow these steps to setup a VPN to bypass the university network restrictions and connect your VM to your Robot in a secure way.


### Step 1: Singup for Tailscale to create Your VPN Network

1. Go to [tailscale.com](https://tailscale.com) and sign up for a free "Personal" account.

### Step 2: Connect the VM to Your Network

1. Open a terminal window.
2. Run the following command to begin the link process:
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```
3. After installing Tailscale, run the below command to run it:
```bash
sudo tailscale up --accept-dns=true
```
4. The terminal will display a **URL**. Copy and paste this link into your host web browser.
5. If not logged in, log in using the account created in Step 1. The VM is now part of your private virtual network.

### Step 3: Connect the Robot to Your Network

1. Access the Raspberry Pi terminal (via SSH over a cable, or a monitor and keyboard).
2. Run the following command to begin the link process:
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```
3. After installing Tailscale, run the below command to run it:
```bash
sudo tailscale up --accept-dns=true
```
3. The terminal will display a **URL**. Copy and paste this link into your host web browser.
5. If not logged in, log in using the account created in Step 1. The Turtlebot is now part of your private virtual network.

### Step 4: Get Machine Names from the Tailscale Admin Console
1. **Open the Admin Console:** Log in at [login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines).
2. **Locate the "Machines" List:** You will see a table of all devices currently connected to your network.
3. Under **Machine Name** you should see a row for the turtlebot (ex: turtlebot8) and a row for the vm (ubuntuvm12). Copy the machine names for each of them.
4. Click on the **DNS** link or go to [login.tailscale.com/admin/dns](https://login.tailscale.com/admin/dns).
5. Copy the **Tailnet DNS Name** (ex: tailcdXXXXX.ts.net).

### Step 5: Test SSH
1. To ssh into the robot from the VM:
```bash
ssh <robot_machine_name>@<tailnet_dns_name>
```
2. To ssh into the VM from the robot:
```bash
ssh <vm_machine_name>@<tailnet_dns_name>
```

If both of them work, you have successfully setup a VPN where your Ubuntu VM can communicate with the Turtlebot.

### Step 6: Share your Turtlebot
```bash
Skip this step if you are not sharing your turtlebot.
```
1. Open the [Tailscale Admin Console](https://login.tailscale.com/admin/machines) on your host computer.
2. Locate your Turtlebot in the list, click the **"..."** (three dots) menu, and select **Share**.
3. Copy the invitation link and send it to your teammate(s).
4. Teammates must click the link and log in with **their own Tailscale accounts**. Once accepted, the Pi will appear in their Tailscale device list as if it were on their own network.
---

#### **On All Team Laptops:**

To tell ROS 2 where the robot is, add these lines to the end of your `~/.bashrc` file:

```bash
export ROS_DISCOVERY_SERVER=<PI_TAILSCALE_IP>:11811
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export ROS_DOMAIN_ID=<YOUR_ASSIGNED_TEAM_NUMBER> 

```

*Note: The **Domain ID** ensures your team's traffic does not interfere with other robots in the lab.*

---

### ⚠️ Troubleshooting

* **Verification:** Try to `ping` the Pi's Tailscale IP from your laptop. If it responds, the "tunnel" is working.
* **Connection Quality:** Run `tailscale status` in your terminal.
* **Direct:** Optimal performance.
* **Relay:** You may experience lag. This happens when the university firewall is particularly strict. Try reconnecting to the Wi-Fi or using a mobile hotspot if the lag is unworkable.


* **No Data in ROS:** Ensure the `ROS_DISCOVERY_SERVER` IP address on your laptop exactly matches the IP found in Step 2.

---

**Would you like me to provide a script for the Pi that automatically starts this Discovery Server whenever the robot is turned on?**


---


#### Startup
1. Move the robot to the charger dock.
2. The light on the charger would light up, foloowed by the ring light on the robot.
3. Wait for about 3 mins until you hear a chime, and see a fast pink light rotating in the ring.
> This chime signifies that both the RPi and the Create 3 robot controller are powered on and communicating with each other. It also confirms that the ROS 2 environment is fully initialized and operational.


#### Set Turtlebot to WiFi AP mode
1. After the robot is succesffully booted.
2. Make sure the light is Solid White.
3. Press "Button 1" 5 times. 
   > You will hear a long beep.
4. The robot will now restart and go into Wifi AP mode. Wait for the startup process (!3 mins).
5. Connect to the Wifi SSID "Tutlebot_AP_X" (X is your ID)
6. ssh ubuntu@10.42.0.1
7. Run turtlebot4-setup
@todo


## Robot Configuration 

You must configure your specific TurtleBot 4 hardware via the physical screen.

### 1: Setup SSH Tutorial: Remote Access Made Simple
SSH (Secure Shell) is the industry-standard method for connecting to a remote computer (like your TurtleBot 4 or a VM) over a network. It encrypts all traffic to prevent eavesdropping.

---

> **Credentials**
> * **User:** `eva`
> * **Password:** `wall-E`

#### Basic Connection
From your terminal (Linux, macOS, or Windows PowerShell), use the following syntax:

```bash
ssh ubuntu@<ROBOT_IP>
```

> **First Time Connection?** > You will see a warning: "The authenticity of host... can't be established." 
> Type **yes** and press Enter. This adds the remote machine to your `~/.ssh/known_hosts` file.

---

#### Passwordless Login (SSH Keys)
Typing a password every time is slow. SSH Keys allow you to log in securely without a password using a "lock and key" system.

##### Step 1: Generate your Keys
On **your laptop**, run:
```bash
ssh-keygen -t ed25519
```
*Press Enter through all prompts to use the defaults.*

##### Step 2: Copy the Key to the Remote Machine
```bash
ssh-copy-id eva@<ROBOT_IP>
```

Once this is done, you can log in simply by typing `ssh eva@<ROBOT_IP>`.

---

#### Common SSH Tasks
| Action | Command Syntax |
| :--- | :--- |
| **Run a command & exit** | `ssh user@ip "ls -la"` |
| **Copy file TO remote** | `scp local_file.txt user@ip:/home/user/` |
| **Copy file FROM remote** | `scp user@ip:/path/to/file.txt ./` |
| **Edit remote file** | `ssh user@ip "nano config.yaml"` |

---


### 2: Turtlebot ROS2 Setup: Set Namespace and Discovery Server

Run the built-in ROS2 setup utility for turtlebot4:

```bash
turtlebot4-setup

```
Navigate through the robot menu and follow the interactive prompts carefully:

1. **ROS Setup** -> **Bash Setup**
* Set `ROBOT_NAMESPACE` to: `robot_X` (Replace `X` with your assigned ID).

2. **ROS Setup** -> **Discovery Server**
* **Onboard Server - Server ID**: `X` (Must match your namespace ID).
* Select **Save**.

3. Return to the **Main Menu**.
4. Select **Apply Settings**.
* Confirm **YES**.
* **Wait:** The process takes 1–3 minutes to reconfigure the network layers.

5. **Exit** the utility.

Apply the changes to your current session and restart the ros2 daemon processes:

```bash
turtlebot4-source
turtlebot4-daemon-restart
```

> [!IMPORTANT]
> **Wait for the Chime:** Once the Create® 3 base config is loaded, the white spinning LED will stop. Wait until the LED ring settles on a **Solid White** state before proceeding.


### 3: VM ROS2 Setup: Set Namespace and Discovery Server
These steps allow your personal computer to "see" the robot on the network, when the robot is running.

#### Step 1: Run Discovery Script
Run the command below to get the VM's IP address:
```bash
ip -4 addr show ens33 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```

Run the configuration script provided in your setup folder:

```bash
. ~/vm_setup/configure_discovery.sh
```

Follow the interactive prompts carefully:

* **ROS_DOMAIN_ID [0]:** `0`
* **Discovery Server ID [0]:** `X` (Matches your robot ID)
* **Discovery Server IP:** `Y` (The IP of your TurtleBot)
* **Discovery Server Port [11811]:** Press **Enter** (Default)
* **Next Action:** Enter `d` for **Done**.

#### Step 2: Finalize EnvironmentSmart Series 4000

Source your bash configuration and restart the ROS 2 daemon to clear any old discovery caches:

```bash
source ~/.bashrc
ros2 daemon stop; sleep 5; ros2 daemon start
```


#### Step 3: Setup Environment to use ROBOT
1. Open a terminal and run 
```bash
set-ros-env robot
```
> This a wrapper script that helps you setup the necessary env variables to work with a robot or with a simulator. The available options for `set-ros-env` are `sim` or `robot` to use a simulation environment or real robot environment, respectively.

2. Close ALL the open terminals before you continue so that the new changes are in effect.



#### Shutdown Gracefully

1. Move the robot out of the charger.
2. Open a terminal and run 
```bash
sudo shutdown -h now
```
3. Wait for 20 secs to allow the RPi to shutdown completely.
4. Press and hold the Power Button (Large button with a ring light) for about 8 secs, until you here a chime.