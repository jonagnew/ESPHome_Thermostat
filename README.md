# ESPHome_Thermostat

Implementing a dual-setpoint HVAC thermostat on an inexpensive quad-relay ESP32 board using ESPHome. Inspired by janick's post on home-assistant.io: https://community.home-assistant.io/t/how-to-replacing-ecobee-smart-thermostat-with-esphome-climate-control/755102 

In this implementation, we're getting our default temperature reading from Home Assistant, and we've added a local DHT22 temperature/humidity sensor as a fallback when the Home Assistant server is unavailable. 

This version adds interlocking logic for the relay outputs to prevent undesired conditions, such as running heating/cooling simultaneously or without also running the fan. I've also added a BYPASS mode to pass control to the existing wall-mount thermostat, when wired correctly (see below.)

#  Components
The target board:
https://www.aliexpress.com/item/2255799905174014.html

DHT22 sensor:
https://www.amazon.com/gp/aw/d/B0D93SY7G8

#  Software Installation
This relay board ships with the manufacturer's default firmware, so you will first need to re-flash the board with ESPHome firmware using a USB-to-serial adapter. This is outside the scope of this document, but the process is well-documented elsewhere.

Debug Port Pinout:
https://github.com/dtlzp/relay_dev_demo/blob/d0c83378aee794cc23beb60e51633c4a9e3a6e84/gpio_pinout/debug_port_4CH_V2.png

IMPORTANT: Make sure you are using a 3.3v serial adapter! 5V logic levels can permanently damage or destroy the ESP32 microcontroller. 

Once your relay board is flashed with ESPHome:

Edit the Thermostat.yaml file to include your OTA password and Home Assistant API encryption key, then flash using the ESPHome Web flasher or the ESPHome dashboard in Home Assistant. 

#  Hardware Installation
Local Temperature Sensor:

Connect the DHT22 sensor to your relay board using jumper wires. 3.3V, ground, and GPIOs are broken out on the relay board's diagnostic header. 

Relay Board GPIOs:
https://github.com/dtlzp/relay_dev_demo/blob/d0c83378aee794cc23beb60e51633c4a9e3a6e84/gpio_pinout/4ch.png

I've enabled the ESP32's internal pull-up in the configuration for the DHT22 input pin, but if you still find your DHT22 sensor works intermittently or stops returning data, you may need to add a 10K pull-up resistor between the sensor's VCC and Out pins. 

Preserving Existing Thermostat:

I've elected to keep the existing thermostat as a backup, so the output relays on the ESP32 board are wired as follows:

Relay COM - Wire to air handler

Relay NC  - Wire to existing thermostat

Relay NO  - Jumpered 24 VAC "hot"

In normal use, turn the existing thermostat OFF, and the ESP32 board will control the system. Otherwise, powering off the ESP32 board or activating BYPASS mode will connect the wall thermostat to the air handler. 
