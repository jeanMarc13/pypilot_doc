# Introduction

## Motivation

The pypilot_mfd project is intended to provide a free and open design for a marine multifunction display.  This is primarily for sensors using open standards such as SignalK and from free software instruments to integrate with solutions like pypilot and openplotter.

## Philisophy

pypilot is free software (GPLv3) and as such you can trust it for use as intended and for future use without restrictions.  Because the freedom to modify is guarenteed, users from around the world continuously contributed improvements, even small ones to the benefit of all users.

# Hardware and Installation

The pypilot_mfd has an internal esp32 microcontroller.   This processor can receive the ESP-Now packets produced from various wireless weather instruments as well as communicate wirelessly to opencpn, signalk server, pypilot and others.

## Mounting Instructions

Find a good location on the boat to mount the pypilot_mfd display.  The enclosure is fully waterproof and can be exposed to the elements however using a cover over the screen can extend its useful life when not in operation especially if mounted in direct sun.

The display can be mounted in any orientation.  Internal accelerometers detect the orientation of the screen on powerup.   It may be an advantage to mount the screen in portrait or landscape depending on the intended use.

## Electrical Connections Overview

diagram

### USB-C data connection

USB-C can be used to power the display as well as provide serial communication with other devices.   The power consumption is light (0.4 Watts) and is slightly higher when the backlight is enabled.  The baud rate can be selected from the web interface.

### rs422 data connection

The mfd also supports a galvanically isolated rs422 data connection.  This has 2 differential pairs (4 wires) for transmit and recieve of nmea0183 data.  The baud rate can be selected from the web interface

## Maintainance

The mfd does not require much maintinance besides covering it when not in use.   It is recommended to mount it so that the usb and other wire connections are protected from the elements or sealed.

## Mechanical Installation

## Suggested sensors

### Wind sensors

<picture of wind sensors>

### Temperature Pressure Sensors

### Outdoor weather sensors

### Depth/Water speed sensors

## Power off

The pypilot_mfd uses flash memory and therefore should be able to always poweroff simply by removing power.   Should the flash become corrupted it is possible that certain settings would be lost.

# Usage

## Keypad

The MFD has a simple keypad with 4 keys
<picture of display>

- left and right arrows page through different displays. You can enable/disable pages from the web interfaces
- hamburger key toggles through different time scales on history displays.
- power key toggles display on/off

holding the hamburger key will enter the menu

## Menu

...

### Alarms

# Web Configuration

holding the left and right keys for 3 seconds will enable the access point ensuring you can connect with a device to access the web configuration if the wifi settings are wrong or they cannot connect

## Sensors

### Wind Sensors

### Water Speed Sensors

### Temperature and Air Pressure Sensors

### UV and lightning Sensors

## USB data interface

## rs422 data interface

## wifi data

The internal esp32 can only operate on one wifi channel at a time.  For this reason, it is recommended that if you have an access point on 2.4ghz with significant bandwidth, to ensure it is on a different channel than the one the mfd and wireless sensors are using.

This could unfortunately make it more difficult a

### Interfacing with pypilot

### Interfacing with OpenCPN

### Interfacing with SignalK

## Automatic Configuration

## Alarms

Various alarms can be set and configured from the web interface rather than the menu

## History plots

Historical data from the sensors can also be viewed on the web browser

## Software Updates
