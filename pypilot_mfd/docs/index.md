# Introduction

## Motivation

The pypilot_mfd project is intended to provide a free and open design for a marine multifunction display.

For example, wind gauges, but many sensor inputs and other displays are implemented.

This is primarily for sensors using open standards such as SignalK and from free software instruments to integrate with solutions like pypilot and openplotter.

## Philisophy

pypilot is free software (GPLv3) and as such you can trust it for use as intended and for future use without restrictions.  Because the freedom to modify is guarenteed, you can be assured that you have the right system and that malicious intentions cannot be hidden in the firmware.

# Hardware and Installation

The pypilot_mfd has an internal esp32 microcontroller.   This processor can receive the ESP-Now packets produced from various wireless weather instruments as well as communicate wirelessly to opencpn, signalk server, pypilot and others.

## Mounting Instructions

Find a good location on the boat to mount the pypilot_mfd display.  The enclosure is fully waterproof and can be exposed to the elements however using a cover over the screen can extend its useful life when not in operation especially if mounted in direct sun.

The display can be mounted in any orientation.  Internal accelerometers detect the orientation of the screen on powerup.   It may be an advantage to mount the screen in portrait or landscape depending on the intended use.

Optional threads on the back of the enclosure may make it simple to mount into a hole.
![enclosure](img/enclosure.png)

The hole diameter to fit the threads should be at least 48mm or 1.9"

## Electrical Connections Overview

<diagram>

### USB-C data connection

USB-C can be used to power the display as well as provide serial communication with other devices.   The power consumption is light (0.4 Watts) and is slightly higher when the backlight is enabled.  The baud rate can be selected from the web interface.

### rs422 data connection

Some models of the MFD also support a galvanically isolated rs422 data connection.  This usually is on a black 4 wire cable.  There are 2 differential pairs (4 wires) for transmit and recieve of nmea0183 data.  Enabling and disabling the port as well as the baud rate can be selected from the web interface.

## Maintainance

The MFD does not require much maintinance besides covering it when not in use.   It is recommended to mount it so that the usb and other wire connections are protected from the elements or sealed.

## Mechanical Installation

## Suggested sensors

### Wind sensors

![sensors](img/windsensors.jpg)

Wireless wind sensors supply wind speed and direction data.   These sensors also incorporate accelerometers which can help to compensate and cancel out effects from boat motion.  Despite this, the sensors readings is better with less motion, however better readings also are where air is clear in front of the sensors.   Unfortunately these locations like the top of the mast often have the most motion from the boat moving.

One solution is to use multiple wind sensors.   The pypilot MFD fully supports several wind sensors.  For example, port and starboard.   In this situation the sensors must be configured for their respective position and the MFD will automatically switch between the sensors depending on wind angle.

Another configuration is primary/secondary sensors.   In this case a primary sensor will be used before a secondary one, and individual sensors can be manually switched off to select which sensor is used.    This could be useful for example if there is a sensor at the top of the mast and another one on the stern.    Each sensor location may be better depending on the point of sail and sea state.

The wind sensors do require 5-40 volt power.   This can be supplied directly, or shared using the power for anchor or running lights.   In that situation, a 5 volt converter should be wired to a SPDT switch.  When 5 volts is supplied the wind sensors have power, but there is not enough voltage to light the light.

![wind power diagram](img/windsensors_power_diagram.png)

The advantage over integrating a solar and battery into the sensors includes
1) Higher reliability, no contacts or encloures, no issue with low solar output, failed solaroutput from bird poop, or overcast winter,  no battery to fail over time.
2) Better data output.  Not relying on limited power allows for a higher data rate (10 readings per second)
3) Weight aloft -  reduced weight and windage aloft is always a benefit for sailing performance
4) Simplicity/cost - reduced components reduces cost
5) Reuse exist wires - most masts already have power wires for light anyway, and this system can piggyback off those avoiding additional wires

### Temperature, Pressure Humidity Sensors

![sensors](img/pressure_humidity_gas_sensor.jpg)

Wireless sensors for temperature, pressure and humidity sensors can be integrated simply by mounting them somewhere in the boat and supplying 12/24 volt power.

These sensors monitor the barometric pressure, relative humidity as well as can detect volatile gas such as gas leaks, solvents or other forms of pollution.   Finally these sensors can accurately monitor the battery voltage.

### Outdoor weather sensors

The outdoor weather sensors incorporate a lightning sensor, UV sensor and ambient light sensor.  There is an integrated battery and solar panel to keep the system isolated from the main battery system and make it more sensive to lightning detection.   Alarm for detecting lightning can be set.

### Depth/Water speed sensors

To be developed

## Power off

The pypilot_mfd uses flash memory and therefore should be able to always poweroff simply by removing power. Should the flash become corrupted it is possible that certain settings would be lost.

# Usage

## Keypad

The MFD has a simple keypad with 4 keys
<picture of display>

- up and down (left and right) arrows page through different displays. You can enable/disable which pages are available from the web interface.

- menu key !(hamburger icon)[img/hamburger.png] key toggles through the different time scales on history displays.  Holding the menu key enters the menu.

- power key toggles display on/off.   This can be configured to either turn off the display and backlight, or actually powerdown into a low power mode using the minimum amount of electricity.   If powered down, the alarms will not work, and data cannot be received wirelessly and relayed to other devices.  Holding the power key resets the device.


- Holding both arrow keys at the same time enables/disables wifi AP mode which can be useful to configure the display from another device using a browser.

- Entering bootloader to reprogram
1)  OTA update.   Hold menu key while applying power.  The screen should remain blank.  Connect to OTA access point and browser and submit .bin file.
2)  serial Bootloader - Hold power key while applying power.  Use esptool to upload.   Use usb device port to communicate.

## Menu

To enter the menu, hold the menu key for 1-2 seconds.   The menu key selects items in the menu, but holding it can also exit the menu.

< picture of main menu >

### Alarms

Various alarms can be set to alert you of changing data.  To enable an alarm, hold the menu key to enter the Main Men, and then press the menu key to select the Alarm menu and then select an alarm.

When an alarm is triggered, an audible beeping is issued, and the screen will switch to the menu for particular alarm that was triggered.  From here you can disable the alarm.

Most alarms will be triggered if the particular data needed to monitor the alarm becomes unavailable.   For example losing GPS may trigger the anchor alarm even if the boat has not changed position.

#### Anchor Alarm

The anchor alarm can be set to the boat position by enabling it.   Disabling and enabling the alarm will reset it to the current boat position.   Generally the alarm should be set when the boat is directly over the anchor.   The range can be set in meters.

The anchor alarm display renders the boat position in the alarm circle showing the current distance from the set point.

#### Course Alarm

The course alarm monitors the GPS course and can be triggered if the boat is no longer moving in the desired direction.   This may be useful for example if the boat is steering to wind to detect a wind shift, but generally could be used to ensure the boat is keeping course.

The course alarm display renders a simple highway showing the actual course between two lines.   This indicates the current course and where an alarm would trigger.

#### GPS/Wind/Water Speed Alarm

The speed alarms are handled similarly.  For example, the GPS speed alarm will trigger if it falls below the minimum speed set, or increases above the maximum speed.
The alarm shows a gauge and history indicting the min and max alarm settings

The speed alarms can be a useful alert to let you know if the boat speed or wind speed changes significantly or outside set bounds.

#### Weather alarm menu

The weather alarm has 3 different potential triggers, each one can be enabled/disabled.  The pressure sensors require barometric pressure information such as a pypilot air sensor which can wirelessly transmit this information to the display.   The lightning alarm requires a pypilot lightning/uv sensor.

##### Pressure

The pressure alarm can be set to trigger if the barometric pressure falls below a given value.  This could indicate a low pressure system.

##### Falling Pressure

The falling pressure alarm is set on the rate of change of pressure.   It will trigger if the barometric pressure falls too quickly regardless of the absolute value.   This can be useful to indicate an approaching storm.

##### Lightning Sensor

The lightning alarm can trigger if a lightning strike is detected.   The distance can prevent triggering from more distant strikes, however the absolute distance accuracy is not great.

#### Depth Alarm

The depth alarm can trigger from either a specific depth, or if the rate of change of depth is decreasing too quickly.   Each setting can independently be enabled to trigger the alarm.

#### pypilot Alarm

The pypilot alarm can be used to alert you about potential issues with the autopilot.  These include:

- no connection - display is unable to communicate with pypilot
- over temperature - the motor controller or drive motor is too hot
- no imu - the inertial sensors are not working
- no motor controller - no communication between autopilot and motor controller
- lost mode - for example, if in GPS mode and the GPS to pypilot is lost, it will fallback to compass and continue steering, but this alarm will trigger.  Similarly for wind and other modes.

# Web Configuration

pressing both left and right keys at the same time will enable/disable the access point (AP mode).   In this mode it is easy to connect to the device using a browser.  Look for the access point "pypilot_mfd" and connect to it.  Once connected, visit http://pypilot_mfd.local or http://192.168.4.1

Once configured, if the mfd is a wifi client and connects to a local network, it is possible to also access it via a browser from whatever IP your router gives it, or usually also via http://pypilot_mfd.local however if there are several pypilot_mfd on the network, or other reasons it may be preferable to use AP mode to configure the device.

## WIFI Network Configuration

The Wifi network configuration consists of the SSID and psk to use.   Be aware that the wireless sensors, although they are no WIFI, they use the same radio and frequency.   Therefore they must be on the same channel as the access point.  You may need to switch the sensors to the channel to match your network since the sensors must be on the same channel.

On 2.4ghz wifi, there are really only 3 distinct channels,  channel 1, 6 and 11.   The other channels overlap and interfere with the rest.  If you have a high traffic network on channel 6 for example, you should put the sensors on channel 1, or 11.   If your main wifi network is 5ghz there should be no conflicting issues, but you will need to use usb or rs422 to bridge the data onto the network if needed.

Lots of wifi traffic can interfere and disrupt the reception of sensor data.    For this reason it is recommended to only use wireless networks without much traffic (internet data) such as a "pypilot" network to a tinypilot computer, or to use a separate network or disable wifi in use (use wifi in ap mode only to configure) and receive any wireless sensor data via the usb and rs422 wired connections.   If you experience disruption in sensor data, be aware of this limitation.

## Data Configuration

<screenshot>

This section allows enabling/disabling data input and output to the display on various interfaces. 

### USB data

Data can be input/output on the USB port that also supplies power.   It may be inconvenient to use data on this port and instead use it just for power, in this case data can be disabled.

### RS422 data

Some displays may support a 422 output port which is galvanically isolated.  This port can be wired directly to a pypilot hat, or using an rs422 isolated usb adapter it can be connected via usb to a tinypilot computer or other computer.   Furthermore the outputs can be split (parallel) to output data to multiple devices.  This also can be used to interface legacy nmea equipment including radios, depth sounders, ais and others.

### WIFI data

Enable/disable input or output on the configured wifi data.

- pypilot NMEA - automatically detects pypilot and attempt to communicate to it using NMEA0183 data.  This may be a good option for example to get wind data into pypilot.
- signalk NMEA - automatically detect signalk and attempt to communicate via NMEA0183 with it.   This may be useful for simplified configuration, receiving sentences not supported by signalk directly or debugging, but generally consider directly communicating with signalk.
- TCP client NMEA - connect to any tcp server as a client for NMEA0183 communication.   For example manually specifying the ip and port (20220) of pypilot has the same effect as the pypilot NMEA option.   This could be useful for connecting to other devices, or even directly to opencpn.
- TCP server NMEA - act as a tcp server accepting connections from clients.  This can be used to accept connections from other tcp clients, for example opencpn.
- signalk - automatically detect and connect to signalk-node-server, see [#interfacing-with-signalk]

## Sensors

pypilot_mfd can receive from pypilot wireless weather sensors and display the data directly on screen.  It can also relay the received wireless sensor data over USB, rs422 (nmea0183) or wifi to other systems.   While it does support sensors input via nmea0183 or signalk, this section describes free software wireless sensors using esp-now developed for the pypilot_mfd.

### Wind Sensors

Multiple wind sensors can be configured to be recieved.  This allows the use of primary, secondary and the ability ignore nearby boats using similar sensors.  It is also possible to configure wind sensors as port and starboard.  This is often useful if sensors are mounted on either side of the boat to automatically switch to the sensor in clear air.

### Water Speed Sensors

Multiple water speed sensors can also be used, and similarly to wind sensors can be designated as primary, secondary, port and starboard.   In the case of port/starboard water sensors, the heel angle will be used to determine which sensor to read water speed from.

### Temperature, Air Pressure and Relative Humidity Sensors

The air sensors measure temperature, air pressure and relative humidity and air quality using the bme680 sensor.   They also monitor the battery voltage.   These sensors are normally mounted in the boat and monitor these settings.

### UV and lightning Sensors

The UV and lightning sensors measure ultraviolet irradiance, visible light as well as lightning strikes.

### WIFI data

The internal esp32 can only operate on one wifi channel at a time.  For this reason, it is recommended that if you have an access point on 2.4ghz with significant bandwidth, to ensure it is on a different channel than the one the MFD and wireless sensors are using.

This could unfortunately make it more difficult to interface all of the sensors.   For example, if your boat network uses channel 1, and the wireless sensors and mfd channel 6 (channels 2-5 are overlapping with 1 and 6)  then pypilot_mfd cannot relay the wireless sensors,

Consider a usb cable for a wired connection from pypilot_mfd to pypilot.  This ensures an sensors the mfd is receiving and sending to pypilot have the lowest latency.  It is possible to do it wirelessly as well provided that everthing is running on the same channel.

If you pull full bandwidth on another channel, it can interfere with wireless sensor packets.   They will not all be received anymore.   For this reason it is essential to configure the sensors network to use a different wifi channel from one that might stream large data (like a general internet gateway) and have a separate network for the sensors.

Another way to avoid interference would be to use 5ghz network for the boat and 2.4ghz channel for pypilot, mfd and sensors network.

### Interfacing with pypilot

When interfacing with pypilot it is both possible to communicate directly using the pypilot protocol as well as with nmea data.   It is also possible to indirectly interface via signalk.

### Interfacing with OpenCPN

OpenCPN can directly interface (via nmea0183 sentences) or indirectly via SignalK.  To directly interface, put the MFD into tcp nmea server mode with a specific port, and make a tcp nmea connection to this address:port in OpenCPN.  Conversely it is possible for opencpn to listen on a particular port and to make the MFD act as a tcp client.

### Interfacing via NMEA0183

NMEA0183 is an old open standard for marine data communication.  It can still serve an important purpose as the data is generally fairly concise, readable and supported by a wide variety of equipment.   The MFD supports this format for input and output on both USB and rs422 ports as well as via wifi.

#### Supported sentences for receiving NMEA0183

- $xxMWV - wind speed and direction apparent/true
- $xxMWD - wind speed and direction true
- $xxVWR - wind speed and direction apparent
- $xxVWT - wind speed and direction true
- $xxMDA - air pressure, air temperature, water temperature
- $xxMTA - air temperature
- $xxRMB - route following information
- $xxRMC - GPS time, position, speed and heading
- $xxVHW - Water speed
- $xxHDM - Compass heading
- $xxXDR - Pitch and Roll
- $xxROT - Rate of Turn
- $xxRSA - Rudder Angle
- $xxAPB - Autopilot Bearing
- !xxVDM - AIS ships

#### Supported sentences for sending NMEA0183

If wireless sensors are connected, the data can be relayed and send out via NMEA0183.  Other data received is not relayed.

- $QYMWV - Apparent Wind Speed and Direction
- $QYMDA - Barometric Pressure and Relative Humidity
- $QYMTA - Air Temperature
- $QYVHW - Water Speed
- $QYDBT - Depth
- $QYMTW - Waterm Temperature

### Interfacing with SignalK

pypilot_mfd can send and receive SignalK data.  The signalk server requires access requests to be granted to receive data

#### Supported keys for receiving data from SignalK

- environment.wind.speedApparent
- environment.wind.angleApparent
- environment.wind.angleTrue
- environment.wind.speedTrue
- environment.outside.temperature
- environment.outside.pressure
- depth.belowSurface
- navigation.speedOverGroundTrue
- steering.rudderAngle
- steering.autopilot.target.headingTrue
- navigation.headingMagnetic
- navigation.rateOfTurn
- navigation.speedThroughWater

#### Supported keys for sending data to SignaK

If wireless sensors are connected, the data can be relayed and send out via SignalK.  Other data received is not relayed

Wireless wind sensors:

- environment.wind.angleApparent
- environment.wind.speedApparent

Wireless pressure/humidity sensors:

- environment.outside.temperature
- environment.outside.pressure
- environment.outside.relativeHumidity
- electrical.batteries.house.voltage

Wireless depth/speed sensors:

- navigation.speedThroughWater
- environment.depth.belowSurface
- environment.water.temperature

For lightning/UV sensors no signalk keys exist they must be monitored via the display

## Display Pages

The display pages each show different combinations of data on the screen.   Most users will only want to enable certain displays depending on the data available and the intended use.  This makes paging through the displays simpler.   With multiple pypilot mfd units, each one can be set to enable certain pages.

## Display Options

There are several options that can be set to configure the displayed information.

- use360 - This will display gauges from 0-180 in port and starboard rather than 0-360

These options select imperial units rather than metric

- usefahrenheit - display temperatures in farenheit (F) rather than celcius (C)
- useinHg - displays pressure using inches mercury instead of millibars
- usedepthft - displays depth in feet rather than meters

The latitude and longitude can be displayed in 3 basic formats:

- degrees - eg: 78.234410   N 32.932330     E
- minutes - eg: 78 14.065'  N 32 55.940'    E
- seconds - eg: 78 14 3.60" N 32 55' 56.39" E

The LCD contrast and backlight brightness can be set here.

It is also possible to set the rotation of the display.   Normally the accelerometers detect the rotation, but in the event they do not, you can change the rotation.  The unit should be power cycled to utilize the new orientation.

The mirror should normally be set with a factory jumper, but in the event the diplay shows everything in a mirror image, this option can be toggled.

- powerdown

The powerdown option specifies if the power button should simply turn off the display and backlight, but continue to receive from wireless sensors,  or actually powerdown the microprocessor and go into a sleep mode using the lowest amount of power consumption (microamps)

### Pages

The MFD has various pre-programmed display pages.   Most users will only want to enable the pages that can display information that is useful to them.  If you have multiple MFD displays it might be useful to program different ones to different pages.  If for example you have programmed 4 pages, then paging through them with the arrow buttons will not page through information that is not helpful or available.

### Automatic Configuration

Automatic configuration button will scan the available incoming sensors, and enable relevant display pages.   This should be performed 

## Alarms

Various alarms can be set and configured from the web interface rather than the menu

## History plots

Historical data from the sensors can also be viewed on the web browser

## Software Updates

It is possible to update the software either via usb or wirelessly.

### OTA Update

stands for Over The Air Updates.  It is possible either to select a firmware file in the web configuration to update the software, or if the device is connected on a network with internet access, it can automatically initiate an update from the pypilot.org website.

### USB Update

Updatting via usb is more fool-proof and should work in all conditions.   To initiate the bootloader, hold the power button on the keypad while applying power to the mfd.   The screen should not display anything and the backlight will not turn on.   At this point the device is in the bootloader and the commandline tool "esptool.py" can be used to flash new data to the device.
