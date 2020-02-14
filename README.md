# Bathroom Overdose Sensor

Plans to use Hassio/Home Assistant as a harm reduction tool; specifically to monitor bathrooms for signs of potential accidental drug overdose.

Multiple versions are planned, with two initial builds in mind:

### 1. Budget build

**PIR-based**, using an inexpensive and easy to find PIR sensor. (Keeping in mind that is will run off Hassio, so any PIR that fits the task and works with the ecosystem will be usable)

### 2. Best-case scenario build

**Radar-based** using a Xethru sensor, on a yet-to-be-determined board, set up as an MQTT device. Initial plans were to specifically use the respiration monitor, but due to the stringent requirements at which the sensor can detect respiration this will probably not be a very useful metric. However, these units' presence and movement detection is far more fine-grained than standard PIR sensors, so until respiration becomes easier to measure this should still work well.

### Components for both builds

- Raspberry Pi running Hassio (HA-Core could work just as well; just wouldn't be as simple to implement due to the required addons), Mosquitto, and Grafana.
- Door sensor, for the bathroom door.
- Z-Wave dongle (will also include Zigbee instructions later on, and further out PoE to remove batteries and wifi from the equation entirely).
- Speaker or Alarm (still working this out)
- User bypass system (still figuring out hardware, but could be a bulb that flashes and emits an audio warning and a button to reset the timer)

### Home Assistant Setup

- Multiple timers: for first warning, second warning, and the alarm. 
- Automation to begin the timers, triggered on detecting motion inside the bathroom with the condition that the door is shut (this will be the most important part of choosing a PIR sensor, since many of them have long intervals after detecting motion before they'll start detecting again. Won't be an issue for the XeThru sensor, as this reports continuously out of the box.)
- Automation to reset the timers if motion is sensed inside the bathroom
- Automation to manually reset the timers from inside the bathroom
- Automation to deactivate the timers if the door is closed and no motion is detected inside the bathroom
- Options for notification of alarms on staff devices (push alerts, audio message, etc) for each warning and alarm
- Lovelace panel which shows all active timer states, sensor states, battery levels, signal strength, overview of recent use metrics, etc.
- Grafana and Mosquitto integration (I haven't used Grafana yet, but some instructions should be provided to best display use metrics)
- Battery monitoring for all devices, with warnings every 10% below 50%.

### Items to be included in this repo

- Hassio config files
- Link to separate repo for X4M200 integration, once that's up and running
- Hardware suggestions and compatibility lists (wiki)
- Guide for general bathroom setup (where to place sensors, general best practice suggestions for harm reduction in bathroom setups such as  sharps containers and an outward-swinging door)
