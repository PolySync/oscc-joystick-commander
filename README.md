<img src="https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/oscc_logo_title.png">

# Joystick Commander

## Overview

Joystick commander is an example application designed to show how the Open Source Car Control API can be used to recieve reports from and send commands to a drive by-wire enabled vehicle.

Using an SDL2 supported game controller, inputs are normalized and converted to relative torque, throttle, and brake commands. This application also demonstrates registering callback functions to recieve and parse OSCC reports as well as vehicle state reports from the car's OBD-II CAN network.

For more information about OSCC, check out our [github](https://github.com/PolySync/oscc).


## Getting Started

These instructions have been tested with a new Ubuntu 18.04 installation.

- OSCC's API and firmware modules are both required, and the modules must be installed on the vehicle
- The socketcan driver for USB and PCIe CAN interfaces is required, and is pre-installed on most Linux systems
- An SDL2 supported game controller is also required, and the SDL2 library must be pre-installed
- A CAN interface adapter, such as the [Kvaser Leaf Light](https://www.kvaser.com), is also necessary in order to connect the API to the OSCC control CAN network via USB

This application has been tested with a Logitech F310 gamepad and a wired Xbox 360 controller, but should work with any SDL2 supported game controller. Controllers with rumble capabilities will provide feedback when OSCC is enabled or disabled.

[Xbox 360 Wired Controller](https://www.amazon.com/dp/B004QRKWLA)

[Logitech Gamepad F310](http://a.co/3GoUlkN)


### 1. Install Dependencies

You will need several packages installed on your machine in order to build the oscc-joystick-commander application. Use the following command to install them using `apt`:

```sh
sudo apt install git cmake libsdl2-dev
```

### 2. Set up this repository locally

Clone this repository to your machine, if you have not already done so:

```sh
# clone the repository
$ git clone git@github.com:PolySync/oscc-joystick-commander.git


# change the current directory to the repository
$ cd oscc-joystick-commander
```

Note: If you do not have a github account you will need to clone the repository using HTTPS.

Then, from within that directory, sync and initialize Git submodules:

```sh
$ git submodule sync
$ git submodule update --init --recursive
```

## Building Joystick Commander

### 1. CMake Configuration

Before you can build the oscc-joystick-commander application, you must use cmake to configure the project. From within the oscc-joystick-commander repository on your machine:

```sh
# create a build directory that will act as our workspace, and cd to it
$ mkdir build/
$ cd build/
```

From there, you will run `cmake` to build a platform-specific Makefile. `cmake` will require some arguments, including the vehicle that the firmware is being built for. You can get a list of supported vehicles using the following command:

```sh
$ cmake -LA .. 2>/dev/null | grep 'VEHICLE_VALUES'
```

For example, to configure `cmake` to build a platform-specific Makefile for the Kia Niro, you would use the following command (from within the `build/` directory you created):

```sh
$ cmake -DVEHICLE=kia_niro ..
```

Note: You may also choose to add the flag -DDEBUG=ON to enable debug messaging, serial console, etc. This should not be used for production builds.

### CAN interface

After initializing the CAN interface, use the channel number to start joystick commander and begin sending commands to the OSCC modules.

For example with a Kvaser Leaf Light attached, using a bitrate of 500000 and a CAN channel of 0:

```sh
 sudo ip link set can0 type can bitrate 500000
 sudo ip link set up can0
```

You would then run:

```sh
./oscc-joystick-commander 0
```

For more information on setting up a socketcan interface, check out [this guide](http://elinux.org/Bringing_CAN_interface_up).

# Application Details

## Controlling the Vehicle with the Joystick Gamepad

Once the joystick commander is up and running you can use it to send commands to DriveKit.
Use the left trigger to brake, the right trigger for throttle, and the left gamepad axis to control steering.

The vehicle will only respond to commands if control is enabled with the start button. The back button disables control.

### main

`main.c` is the entry point of joystick commander. Initializes OSCC interface, checks for controller updates in 5 ms intervals, and closes the interface when the program terminates. This contains the applications main loop.

### joystick

`joystick.c` contains the functionality necessary to initialize and interact with the game controller.

### commander

The commander files contain the joystick commander's interactivity with the OSCC API. It demonstrates opening and closing the CAN channel communications with OSCC's control CAN network, sending enable/disable commands to the modules through the API, retrieving OSCC reports through callback functions, and sending commands through the OSCC `publish` functions.

# Using OSCC API

To use the OSCC API in your applications, you need to include any relevant header files.

* The can message protocols are located in `oscc/api/include/can_protocols/`
    * These specify the structs we use for steering, throttle, brake, and fault reports
* Vehicle specific macros and values are located in `oscc/api/include/vehicles/`
	* You only need to include `vehicles.h`, which will include the relevant vehicle-specific header depending on the option provided to CMake (e.g., `-DVEHICLE=kia_niro` will include `kia_niro.h`)
* `oscc/api/include/oscc.h` includes the functionality to interface with the OSCC API


# License Information

MIT License

Copyright (c) 2017 PolySync Technologies

Please see [LICENSE.md](LICENSE.md) for more details.


# Contact Information

Please direct questions regarding OSCC and/or licensing to help@polysync.io.

*Distributed as-is; no warranty is given.*

Copyright (c) 2019 PolySync Technologies, Inc.  All Rights Reserved.
