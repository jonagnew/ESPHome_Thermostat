# ESPHome_Thermostat

Implementing a dual-setpoint HVAC thermostat on an inexpensive quad-relay ESP32 board using ESPHome. Inspired by janick's post on home-assistant.io: https://community.home-assistant.io/t/how-to-replacing-ecobee-smart-thermostat-with-esphome-climate-control/755102

In this implementation, we're getting our default temperature reading from Home Assistant, and we've added a local DHT22 temperature/humidity sensor as a fallback when the Home Assistant server is unavailable. 

This version adds interlocking logic for the relay outputs to prevent undesired conditions, such as running heating/cooling simultaneously or without also running the fan. I've also added a BYPASS mode to pass control to the existing wall-mount thermostat, when wired correctly (see below.)

#  Components
The target board:
https://www.aliexpress.com/item/2255799905174014.html

DHT22 sensor:
https://www.amazon.com/gp/aw/d/B0D93SY7G8

#  Hardware Installation
I've elected to keep the existing thermostat as a backup, so the output relays on the ESP32 board are wired as follows:

Relay COM - Wire to air handler
Relay NC  - Wire to existing thermostat
Relay NO  - Jumpered 24 VAC "hot"

In normal use, turn the existing thermostat OFF, and the ESP32 board will control the system. Otherwise, powering off the ESP32 board or activating BYPASS mode will connect the wall thermostat to the air handler. 
