# 🧹 Smart Vacuum & Light Sync 💡

## 📖 Overview
This Home Assistant automation synchronizes your home lights with the movements of your robot vacuum.  
It automatically turns on the light in the room where the vacuum is cleaning or entering, and switches it off when the vacuum leaves or stops.

## ✨ Features
- 🔦 Lights turn on when the vacuum enters a room (while cleaning or returning to its dock).
- 🚪 Lights turn off when the vacuum leaves a room, and when it is paused, docked, idle or in error.
- 🙋 Lights that were already on before the vacuum came are **never switched off**.
- ♻️ Optional exact restore of the previous state (on/off, brightness, color) once the vacuum has left.
- ⏱️ Optional delay before turning lights off, to avoid flickering when the vacuum goes back and forth.
- 🧹 Every room the vacuum lit is handled when it stops, even if the room sensor is stale. Optional safety net to turn off all mapped lights.
- 🌞 Ambient light condition: one default lux sensor, or one sensor and threshold per room.
- 🌙 Optional "only after sunset" and time window conditions (overnight windows supported).
- 👥 Optional presence detection (lights only turn on when nobody is home).
- 🏠 Several lights per room, or automatic use of your Home Assistant **areas**.
- 🚨 On vacuum error: flash the lights where it is stuck and/or send a notification.
- 💡 Optional brightness level for lights.

## 🧠 How it works
When the vacuum enters a room, the automation saves the state of the lights it is about to turn on in a temporary scene
(`scene.vacuum_lights_<automation>_…`), then turns them on. When the vacuum leaves the room (after the optional delay) or stops,
that scene is restored (or its lights simply turned off) and deleted. Presence, lux, sun and time conditions only prevent turning
lights **on**: turning them off is never blocked.

> ⚠️ Temporary scenes are not kept across a Home Assistant restart. If HA restarts during cleaning, enable
> *Turn off all mapped lights when the vacuum stops* so nothing stays on.

## ⚙️ Requirements

### Mandatory
- Home Assistant **2024.10** or newer.
- A robot vacuum that exposes a **current room sensor** (e.g., Dreame L40s Pro Ultra with `sensor.current_room`).
- Light entities (`light.xxx`) mapped to your rooms.

### Optional
- Presence entities (`person.xxx`) to prevent lights from turning on if someone is home.
- Ambient light sensors (`sensor.xxx`, device class *illuminance*).
- Home Assistant areas named exactly like the rooms reported by the vacuum.
- A notify action (e.g. `notify.mobile_app_my_phone`) for error notifications.

## 📥 Import Blueprint
[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FHoM3r17%2Fvacuum-lights%2Frefs%2Fheads%2Fmain%2Fsmart-vacuum-lights-sync)


## 🛠️ Setup
1. Copy the blueprint YAML file into your `blueprints/automation/` folder. *(Only needed if the import button above cannot be used.)*
2. Restart Home Assistant or reload automations. *(Only needed if the import button above cannot be used.)*
3. Create a new automation using this blueprint.
4. Fill in the inputs:
   - **Vacuum & rooms**: vacuum entity, room sensor, rooms → lights mapping and/or *Use Home Assistant areas*.
   - **When to turn lights on** (optional): presence, lux sensor & threshold, after sunset, time window.
   - **Light behaviour** (optional): brightness, restore previous state, delay, turn off all on stop.
   - **Vacuum error** (optional): flash lights, notification.

## 📋 Example mapping of rooms → lights

Room names must match exactly the values reported by your room sensor.
A room can point to a single light, a list of lights, or an object with its own lux sensor and threshold.
With *Use Home Assistant areas* enabled, rooms missing from the mapping use the lights (and illuminance sensor)
of the area with the same name, so the mapping can even stay empty.

YAML:
```yaml
Bathroom: light.lumieres_sdb
Dining Hall: light.lumieres_sam
Living Room:
  lights:
    - light.lumieres_salon_1
    - light.lumieres_salon_2
  lux_sensor: sensor.salon_illuminance
  lux_threshold: 50
Kitchen: light.lumieres_cuisine
```

JSON (also accepted):
```json
{
  "Bathroom": "light.lumieres_sdb",
  "Dining Hall": "light.lumieres_sam",
  "Living Room": ["light.lumieres_salon_1", "light.lumieres_salon_2"],
  "Kitchen": "light.lumieres_cuisine"
}
```

## 🔄 Upgrading from V1.x
- Existing automations keep working: the old JSON text mapping is still accepted.
- Brightness `0` is now treated as "keep last brightness" (it used to turn the light off).
- Lights that were already on before the vacuum came are no longer turned off. With *Restore previous state* (on by default) they get back their exact brightness/color.
- Presence, lux, sun and time conditions only block turning lights on.
- Inputs are now grouped in collapsible sections; re-open the automation to see the new options.
