# HA-NeoPool-MQTT
Home Assistant MQTT integration for Tasmota NeoPool module.

![image](https://github.com/alexdelprete/HA-NeoPool-MQTT/assets/7027842/3f39dd99-d962-4720-b96f-f7ee4da78a83)

Since the native tasmota integration wasn't exporting all entities (switches, selects, numbers, etc.) to HA correctly, I opened a [discussion](https://github.com/arendst/Tasmota/discussions/19811) on Tasmota's repo with NeoPool dev, and we decided for now to integrate all possible entities using purely HA's MQTT integration entities.

So you can actually disable Tasmota's HA integration with `SetOption19 0` or `Discover 0` on the NeoPool device, because all the entities will be created through the package via MQTT.

Hoping one day to see Tasmota's HA integration natively creating ALL possible entity types. That will probably happen when @curzon01 switches to Home Assistant. :)

Pre-requirements:

1. The integration is based on NeoPool's extended commands for ESP32 devices, so tasmota32 firmware is required: read [here](https://tasmota.github.io/docs/NeoPool/#esp32-adding-user-defined-neopool-commands-to-tasmota) for more info
2. The integration is currently based on latest dev release of Tasmota, because of recent modification requested to @curzon01 that he quickly implemented in [#19857](https://github.com/arendst/Tasmota/pull/19857), and has already been merged. Next stable release (after v13.2.0.1, current stable version) should contain #19857. I will update notes to specify the stable min. version once known.
3. From Tasmota console, run these commands to optimize the device configuration:
```
# This sets the SENSOR topic update frequency to 10s and sets the Retain flag so that HA entities are immediately available
BackLog TelePeriod 10; SensorRetain 1; SetOption4 1;

# This rule keeps the Sugar Valley device clock in sync with Tasmota's device clock
Backlog Rule1 ""; Rule1 0; Rule1 4
Rule1
  ON Time#Initialized DO NPTime 0 ENDON
  ON Time#Set DO NPTime 0 ENDON
Rule1 1
```
4. Tasmota `%topic%` must be set to `SmartPool`
5. Home Assistant MQTT integration properly configured and working
6. The integration is released as a Home Assistant package, check HA docs on how to configure HA for package usage
7. The lovelace UI is extracted from the raw lovelace file, edit your raw lovelace config and paste the contents of the yaml file

# Credits
Big thanks to @fdebrus for inspiring me and @curzon01 for the great support.

# Coffee

If you like this integration, I'll gladly accept some quality coffee, but please don't feel obliged. :)

<a href="https://www.buymeacoffee.com/alexdelprete" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/black_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a><br>
