# YERP (Your Emergency Response Platform)

This project aims to suggest bundles of software and hardware for people and/or organizations looking to increase access to their bathrooms without sacrificing safety or privacy (for providers: no cloud services!; for end users: the literal privacy bathrooms provide!). Along with a hope that increased access to sanitation can also improve general public health in areas where public bathrooms are scarce (which has resulted in hepatitis-A outbreaks in Philadelphia, home of this repo's maintainer), due to fears of potential accidental overdose.

As there is always potential for monitoring technologies to be used to further restrict peoples' freedoms, these builds are not intended to detect or even preventing any sort of drug use or illicit activity. They are engineered to detect potential medical emergency scenarios to better facilitate medical intervention.

The warning signs this system looks for are based around symptoms for opioid overdose (lack of respiration, detected implicitly through lack of motion), but could also apply to any condition that results in loss of consciousness. This system is not currently planned to trigger because of abnormal body activities such as seizures (as can commonly occur in withdrawal from benzodiazepenes), but any ideas on addressing other emergency scenarios like this are welcome and encouraged.

Multiple versions are planned to accommodate varying budgets. Success is measured in terms of effectiveness, reliability, inobtrusiveness (the best system shouldn't require end users to change their actions at all), and cost:

### 1. Budget build

**PIR-based**, using an inexpensive and easy to find PIR sensor. (Keeping in mind that is will run off Hassio, so any PIR that fits the task and works with the ecosystem will be usable)

### 2. Middle-of-the-road build

**Thermal and PIR-based**, using the above along with an IR grid sensor such as the AMG8833, running attached to a Pi, BeagleBoard, Arduino etc. and sending data as an MQTT device.

### 3. Best-case scenario build

**Radar-based** using a Xethru sensor on a yet-to-be-determined board (probably also Pi, Beagleboard or Arduino), set up as an MQTT device. Initial interested here was specifically for the respiration monitor, but due to the stringent requirements under which the X4M200 can detect respiration (20 seconds of holding still!) this will probably not be a very useful metric. However, these units' presence and movement detection is far more fine-grained than standard PIR sensors, so the need for end users to change their actions to accommodate this system will still be the most minimal using this approach.

### Components for both builds

- Raspberry Pi as a hub, running Hassio (HA-Core could work just as well; just wouldn't be as simple to implement due to the required addons), Mosquitto, and Grafana.
- Door sensor, for the bathroom door.
- Z-Wave dongle (will also include Zigbee instructions later on, and further out PoE to remove batteries and wifi from the equation entirely).
- Speaker or Alarm (still working this out)
- User bypass system (still figuring out hardware, but could be a bulb that flashes and emits an audio warning and a button to reset the timer)

Each build will require one hub, along with individual sensors per each bathroom. An android tablet or desktop PC is suggested for system monitoring, with tablets potentially mounted outside of every bathroom in kiosk mode (showing occupancy, if any warnings have been triggered, and a console for deactivation if the alarm is triggered).

### Home Assistant Setup

- Multiple timers: for first warning, second warning, the alarm, and total occupancy time. 
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
- Advice on wifi setup: routers, securing this system over wifi, how the system could be run as an offline intranet, and isolating smart devices with IP addresses on their own subnet.
