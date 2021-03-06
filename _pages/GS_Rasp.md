---
permalink: /GS_Rasp/
title: Getting Started with IoTivity - Raspberry Pi Kit
overview: false
layout: single

sidebar:
  nav: "gs_nav"
  
toc: true
toc_label: Table of Contents
toc_icon: cog
toc_sticky : true
---

## Introduction

This Getting Started guide will show you how to quickly download, build, and run IoTivity apps, featuring two-way communication between a Linux or Android device and a Raspberry Pi with an add-in Pimoroni board. This simple platform enables you to quickly prototype the capabilities of your planned smart device on real hardware:

* The server, a command-line app, runs on a smart home device, such as a smart switch or smart thermostat, represented by the Raspberry Pi board.
* The client, a GUI app, controls the smart device from a Linux computer or Android tablet.

The two apps talk to each other over Wi-Fi, using the OCF (Open Connectivity Foundation) protocol.

![Set up](../assets/images/communication-over-ocf-protocol.png "The configuration")

## Requirements

To carry out this tutorial, you will need the following:

* Raspberry Pi board with a microSD card with Raspbian OS
* Pimoroni Explorer HAT board
* Optional: HDMI monitor and cable for the Pi
* Optional: USB keyboard and mouse for the Pi
* Ethernet or Wi-Fi connection between a DHCP-enabled network and the Pi
* Linux computer or Android tablet on the same network as the Pi
* The first two listed items are sold together as a kit here. (scroll down, below "Development Resources and Solutions").

## Set up the Raspberry Pi

1. Before connecting power, attach the Explorer HAT board to the GPIO header on the Raspberry Pi. 
   Be careful not to bend the pins, as the connector fits very tightly. (For this example, you don’t need the blue breadboard, which may be attached later.)

   ![board](../assets/images/attach-HAT-plus-blue-board.png "The Pi-HAT board")

2. Insert the micro SD card with an updated version of Raspian already installed into its slot, located under the Pi board.
3. Connect an Ethernet cable between the Pi board and network or connect via Wi-Fi. This will be easier if you have a monitor and keyboard to check the IP address of the Raspberry Pi.
4. Attach the power cable to the Pi board, and plug it in. The board will boot.

## Connect to the Raspberry Pi

The steps in this section use SSH to open a monitor on your personal computer that will let you type commands for the Raspberry Pi.
As an alternative, attach a monitor and USB keyboard directly to the Raspberry Pi.

1. Open a terminal on your development PC.
2. Learn the Raspberry Pi board’s IP address by giving this command from the command line of your Linux PC, then pressing ctrl-C. Use the host name of the Pi, which is raspberrypi unless you have changed it:

   ```bash
   ping raspberrypi.local
   ```

    ![ping Pi](../assets/images/ping-pi-local.png)

   In the screen above, the IP address is 192.168.0.30. 
   Sometimes the ping command doesn't reach raspberrypi.local due to DNS issues. 
   An alternative is to connect a keyboard and monitor to the Pi and run the ifconfig command in the terminal.

3. Connect to the Pi via SSH, using either of these commands:
    For Linux or macOS, use either of these commands:

    ```bash
    ssh pi@raspberrypi.local #replace with current hostname if changed, OR...
    ```

     OR

    ```bash
    ssh pi@ipaddress #use IP address from previous step
     ```

4. Supply the password when prompted. Default login:

   username **pi**, password **raspberry**

   ![ssh prompt](../assets/images/ssh-prompt.png)

## Optionally Make Changes to Support Your Location and a Physical Keyboard and Monitor

1. Before you begin, be sure that the Raspberry Pi is set to use your preferred keyboard.
   UK English is the Pi’s preset keyboard layout. To choose another keyboard layout:

   ```bash
   sudo raspi-config #enter the Pi account password, if prompted.
   ```

   In the Raspberry Pi Software Configuration Tool (raspi-config) menu, choose:

   * Localization Options or Internationalization Options
   * Change Keyboard Layout

   Select your keyboard and preferred keyboard layout. 
   For example, for American English, after you select your connected keyboard, choose English (US). 
   Navigate through the menus to save and exit raspi-config.

   Then reboot with sudo:

   ```bash
   sudo reboot
   ```

   You’re now ready to install IoTivity code and samples.

## Install IoTivity and Pi Samples

1. Continuing on your development PC, from the SSH prompt, go to the home directory and install the IoTivity-lite development system, which takes several minutes to complete:

   ```bash
   cd ~
   curl https://openconnectivity.github.io/IOTivity-Lite-setup/install.sh | bash
   ```

   Alternatively, you can download the install.sh script, review and run it from anywhere.

2. Install the project scripts:

   ```bash
   curl https://openconnectivity.github.io/Project-Scripts/install.sh | bash
   ```

   Rerun this script if an error occurs.

3. Install the repository containing the Raspberry Pi sample code, which takes several minutes to run. Answer “y” if prompted:

   ```bash
   curl https://openconnectivity.github.io/Sample-Raspberry-Pi-Code/pi-boards/install.sh | bash
   ```

   You may need to reboot the Raspberry Pi at this point to ensure that your environment variables are set.

    ```bash
   sudo reboot -h now
   ```

   If you are connected via SSH, you will be disconnected and will have to reconnect when the Pi is booted (a minute is usually enough time)

4. On the Pi, create a project directory:

   ```bash
   cd ~
   mkdir workspace
   cd workspace
   ```

5. Create the IoTivity project (we'll call it myexample):

   ```bash
   create_project.sh myexample
   cd myexample
   ```

6. Copy the sample project to the current directory and set it up:

   ```bash
   cp ~/Sample-Raspberry-Pi-Code/IoTivity-lite/explorer-hat-pro/setup.sh ./
   ./setup.sh
   ```

   If you don't have a HAT board, you can get a sample project with limited functionality.
   Copy the sample project from this location instead: ~/Sample-Raspberry-Pi-Code/IoTivity-lite/nohat.
   (Just substitute **nohat** for **explorer-hat-pro** in the command above)

   The sample project now has Explorer HAT (or no HAT) drivers installed and is ready for code generation and building.

## Build and Run the Server App

1. On the Pi, generate code for the server app by running these commands:

   ```bash
   cd ~/workspace/myexample #(if not already there)
   gen.sh
   ```

   This script starts from a default JSON configuration file that can be edited to specify the capabilities of your actual device.
   The script calls the DeviceBuilder app to generate source code for the server app you’ll run on your smart device (in this case, the Pi).
   NOTE: We run gen.sh from the Project-Scripts directory (it's on the search path) NOT ./gen.sh

2. Build the server app with this command:

   ```bash
   build.sh
   ```

3. Reset (make the device unowned) and run the server app by running these commands:

   ```bash
   reset.sh
   run.sh
   ```

   Leave this terminal window open.
   The server app is now waiting for commands from the client app, which you’ll install next.

## Build and Run the Client App (If you've already installed OTGC once, you can skip the installation here and jump to step 3.)

NOTE: The instructions below are for building and running the OTGC client application on Linux. Alternatively, you can run the Android version of OTGC on your smart phone or tablet. Get the installation package here. You can then just run it and skip the Linux OTGC installation in the steps below. If you run the Android app, you can basically follow the instructions from step 5 below. The circle icon will start the discovery process and should find the Emulator server.

The sample client application is called OTGC (Onboarding Tool and Generic Client). You'll download and build the binary with a script:

1. On the development PC, open another terminal window.
2. Download and build the Linux OTGC client by running this command, which takes several minutes to complete:

   ```bash
   curl https://openconnectivity.github.io/otgc-linux/setup.sh | bash
   ```

   Troubleshooting: If the build process finishes but an error occurs, manually run the dpkg command from the setup.sh script.

   ```bash
   sudo dpkg -i ./otgc-linux/build/debian/out/otgc-2.9.0.deb
   #run only in the event of an error and substitute the version of otgc
   #that is available at this location (for example, the version number will
   #increment as new versions are created (e.g. otgc-2.10.0.deb))
   ```

3. Launch the Linux OTGC client by running this command:

   ```bash
   /usr/bin/otgc.sh
   ```

4. Click to OK the End User License Agreement.
   OTGC starts and automatically scans all visible OCF devices, listing them in the app's left-hand pane.
5. Locate and align both the terminal window that is awaiting incoming connections (as well as the dimming light GUI), plus the app window, so that both are visible.

   As you proceed with the remaining steps in this section, notice that each action taken in the client app generates console output in the terminal window that had been awaiting incoming connections.
   This simulates controlling your smart home device with, for example, a mobile phone client:

   * Click to select the device listed in the left-hand pane, then click the Onboard button. The Select OTM (Ownership Transfer Method) dialog box pops up.
   * Click OK in the Select OTM dialog box. As device ownership is transferred, the Select OTM dialog box closes and is replaced by the Set Device Name dialog box.
   * Change the device name, if you wish. Click OK to close the dialog box.
   * Click to reselect the device in the left-hand pane. In the Generic Client tab, toggle the Value switch on and off.
6. Once the + icon has changed to a gear, click the gear icon.
7. Find the /LED1, /LED2, /LED3 and /LED4 sections and in each, click the switch button on the left.
   The colored LEDs on the Explorer HAT board turn on or off, controlled by the client app over the OCF protocol.
   Notice that the console output in the server terminal on the Pi responds to your actions in the client.
   If you do not have the Explorer HAT board, the green light on the Pi will flash.
8. Now “observe” (monitor) the touch buttons on the Explorer HAT board in the OTGC app by clicking the switch buttons on the right of the /touch1, /touch2, and /touch3 sections.
   Physically touch the buttons on the board numbered 1, 2, and 3.
   Notice that the output in the OTGC app detects the touches. (This step does not apply if you do not have the Explorer HAT board.)
9. Press Ctrl-C in the server terminal on the Pi to exit the server app.

## Digging deeper: Next steps for development

Now that you've created a device simulation based on IoTivity, you're ready to [dig deeper](/gs_diggingdeeper) to explore other platforms and learn how to apply IoTivity to your own platform and devices.
