
## Introduction

This Getting Started guide will show you how to quickly download, build, and run IoTivity apps, featuring two-way communication between a Linux or Android device and an ESP32 microcontroller board (tested with Adafruit Huzzah32 and Espressif Pico Kit). This simple platform enables you to quickly prototype the capabilities of your planned smart device on real hardware:

- The server, a command-line app, runs on a smart home device, such as a smart switch or smart thermostat, represented by the ESP32 board.
- The client, a GUI app, controls the smart device from a Linux computer or Android tablet.

The two apps talk to each other over Wi-Fi, using the OCF (Open Connectivity Foundation) protocol.


[OCF protocol](sss)

## Requirements

To carry out this tutorial, you will need the following:

- An ESP32 board (such as Adafruit Huzzah32 or Espressif Pico Kit)
- A computer capable of running the Espressif ESP32 toolchain (idf.py)
- Ethernet or Wi-Fi connection between a DHCP-enabled network and the ESP32 board
- Linux computer or Android tablet on the same network as the ESP32 board 

## Connect to the ESP32

The code for the ESP32 is compiled using the Espressif tool chain on a compatible computer. This environment can be set up for different operating systems, but this example will use the Ubuntu Linux operating system. This example uses FreeRTOS. The code will be compiled and linked on the host computer, then uploaded via USB to the ESP32 board. Once the code is uploaded, it will immediately begin to execute. At that point, it will connect via Wi-Fi to the specified Wi-Fi network. After the code has been uploaded, the device can run autonomously simply by powering the ESP32. The USB connection is only required to upload the code.

1. Open a terminal on your development computer.
2. Connect the ESP32 to the deveopment computer with an appropriat USB cable.

3. The USB port may already be enabled on the computer, but if not, you may need to execute the following:

```
sudo usermod -a -G tty <username> (where <username> is your login name)
or
sudo usermod -a -G dialout <username> 
````

1. You may need to logout and back in for the change to take effect. You can check by doing the following:
```
groups
```
You should see "tty" and "dialout" listed in the names of the groups.

## IoTivity-lite setup for ESP32

IoTivity-lite is an open source code base for OCF. You will need to install it to build your OCF server device. Use the following

```
curl https://openconnectivity.github.io/IOTivity-Lite-setup/install.sh | bash
```

1.  Install the necessary tools and libraries:
    ```
    sudo apt install -y git wget flex bison gperf python3 python3-pip python3-setuptools python3-serial python3-click python3-cryptography python3-future python3-pyparsing python3-pyelftools cmake ninja-build ccache libffi-dev libssl-dev libusb-1.0-0
​    ```

2. Install the Espressif command line tools:


    a. For Linux:
```
       cd ~/iot-lite/iotivity/port/esp32
git clone --recursive https://github.com/espressif/esp-idf.git
./esp-idf/install.sh
. ./esp-idf/export.sh
```
For Windows:
```
cd ~/iot-lite/iotivity/port/esp32
git clone --recursive https://github.com/espressif/esp-idf.git
./esp-idf/install.sh
. ./esp-idf/export.sh
```
3. Now do the common steps below
username pi, password raspberry

ssh prompt / diagram

Optionally Make Changes to Support Your Location and a Physical Keyboard and Monitor
Before you begin, be sure that the Raspberry Pi is set to use your preferred keyboard. UK English is the Pi’s preset keyboard layout. To choose another keyboard layout:

sudo raspi-config #enter the Pi account password, if prompted.
In the Raspberry Pi Software Configuration Tool (raspi-config) menu, choose:

Localization Options or Internationalization Options
Change Keyboard Layout
Select your keyboard and preferred keyboard layout. For example, for American English, after you select your connected keyboard, choose English (US). Navigate through the menus to save and exit raspi-config.

Then reboot with sudo:

```
sudo reboot
```
You’re now ready to install IoTivity code and samples.

## Install IoTivity and Pi Samples

1. Continuing on your development PC, from the SSH prompt, go to the home directory and install the IoTivity-lite development system, which takes several minutes to complete:

cd ~
curl https://openconnectivity.github.io/IOTivity-Lite-setup/install.sh | bash
Alternatively, you can download the install.sh script, review and run it from anywhere.

2. Install the project scripts:

curl https://openconnectivity.github.io/Project-Scripts/install.sh | bash
Rerun this script if an error occurs.

3. Install the repository containing the Raspberry Pi sample code, which takes several minutes to run. Answer “y” if prompted:

curl https://openconnectivity.github.io/Sample-Raspberry-Pi-Code/pi-boards/install.sh | bash
You may need to reboot the Raspberry Pi at this point to ensure that your environment variables are set.

sudo reboot -h now
If you are connected via SSH, you will be disconnected and will have to reconnect when the Pi is booted (a minute is usually enough time)

4. On the Pi, create a project directory:

cd ~
mkdir workspace
cd workspace

5. Create the IoTivity project (we'll call it myexample):

create_project.sh myexample
cd myexample
Copy the sample project to the current directory and set it up:

cp ~/Sample-Raspberry-Pi-Code/IoTivity-lite/explorer-hat-pro/setup.sh ./
./setup.sh
If you don't have a HAT board, you can get a sample project with limited functionality. Copy the sample project from this location instead: ~/Sample-Raspberry-Pi-Code/IoTivity-lite/nohat. (Just substitute nohat for explorer-hat-pro in the command above)

The sample project now has Explorer HAT (or no HAT) drivers installed and is ready for code generation and building.

## Build and Run the Server App

1. On the Pi, generate code for the server app by running these commands:

cd ~/workspace/myexample (if not already there)
gen.sh
This script starts from a default JSON configuration file that can be edited to specify the capabilities of your actual device. The script calls the DeviceBuilder app to generate source code for the server app you’ll run on your smart device (in this case, the Pi). NOTE: We run gen.sh from the Project-Scripts directory (it's on the search path) NOT ./gen.sh

2. Build the server app with this command:

build.sh

3. Reset (make the device unowned) and run the server app by running these commands:

reset.sh
run.sh
Leave this terminal window open. The server app is now waiting for commands from the client app, which you’ll install next.

## Build and Run the Client App (If you've already installed OTGC once, you can skip the installation here and jump to step 3.)

- link to other page, do not do this again here.

NOTE: The instructions below are for building and running the OTGC client application on Linux. Alternatively, you can run the Android version of OTGC on your smart phone or tablet. Get the installation package here. You can then just run it and skip the Linux OTGC installation in the steps below. If you run the Android app, you can basically follow the instructions from step 5 below. The circle icon will start the discovery process and should find the Emulator server.

The sample client application is called OTGC (Onboarding Tool and Generic Client). You'll download and build the binary with a script:

On the development PC, open another terminal window.
Download and build the Linux OTGC client by running this command, which takes several minutes to complete:

curl https://openconnectivity.github.io/otgc-linux/setup.sh | bash
Troubleshooting: If the build process finishes but an error occurs, manually run the dpkg command from the setup.sh script.
sudo dpkg -i ./otgc-linux/build/debian/out/otgc-2.9.0.deb
#run only in the event of an error and substitute the version of otgc
#that is available at this location (for example, the version number will
#increment as new versions are created (e.g. otgc-2.10.0.deb))
Launch the Linux OTGC client by running this command:

/usr/bin/otgc.sh
Click to OK the End User License Agreement. OTGC starts and automatically scans all visible OCF devices, listing them in the app's left-hand pane.
Locate and align both the terminal window that is awaiting incoming connections (as well as the dimming light GUI), plus the app window, so that both are visible.

As you proceed with the remaining steps in this section, notice that each action taken in the client app generates console output in the terminal window that had been awaiting incoming connections. This simulates controlling your smart home device with, for example, a mobile phone client:

Click to select the device listed in the left-hand pane, then click the Onboard button. The Select OTM (Ownership Transfer Method) dialog box pops up.
Click OK in the Select OTM dialog box. As device ownership is transferred, the Select OTM dialog box closes and is replaced by the Set Device Name dialog box.
Change the device name, if you wish. Click OK to close the dialog box.
Click to reselect the device in the left-hand pane. In the Generic Client tab, toggle the Value switch on and off.
Once the + icon has changed to a gear, click the gear icon.
Find the /LED1, /LED2, /LED3 and /LED4 sections and in each, click the switch button on the left. The colored LEDs on the Explorer HAT board turn on or off, controlled by the client app over the OCF protocol. Notice that the console output in the server terminal on the Pi responds to your actions in the client. If you do not have the Explorer HAT board, the green light on the Pi will flash.
Now “observe” (monitor) the touch buttons on the Explorer HAT board in the OTGC app by clicking the switch buttons on the right of the /touch1, /touch2, and /touch3 sections. Physically touch the buttons on the board numbered 1, 2, and 3. Notice that the output in the OTGC app detects the touches. (This step does not apply if you do not have the Explorer HAT board.)
Press Ctrl-C in the server terminal on the Pi to exit the server app.
Digging deeper: Next steps for development
Now that you've created a device simulation based on IoTivity, you're ready to dig deeper to explore other platforms and learn how to apply IoTivity to your own platform and devices.