# 🧹 Smart Vacuum & Light Sync 💡

## 📖 Overview
This Home Assistant automation synchronizes your home lights with the movements of your robot vacuum.  
It automatically turns on the light in the room where the vacuum is cleaning or entering, and switches it off when the vacuum leaves or stops.

## ✨ Features
- 🔦 Lights turn on when the vacuum starts cleaning.
- 🚪 Lights turn on when the vacuum enters a room.
- 🚪 Lights turn off when the vacuum leaves a room.
- ⏸️ Lights turn off when the vacuum is paused, docked, idle, or in an error state.
- 🌞 Optional ambient light sensor with lux threshold.
- 👥 Optional presence detection (prevents lights from switching when someone is home).
- 💡 Optional adjustable brightness level for lights.
- 🏠 Several lights per room supported.
- 🛡️ Lights always turn off when the vacuum leaves or stops, even if someone came home or the room got brighter in the meantime (presence and lux only block turning lights **on**).

## ⚙️ Requirements

### Mandatory
- Home Assistant **2024.10** or newer.
- A robot vacuum that exposes a **current room sensor** (e.g., Dreame L40s Pro Ultra with `sensor.current_room`).
- Light entities (`light.xxx`) mapped to your rooms.

### Optional
- Presence entities (`person.xxx`) to prevent lights from switching if someone is home.
- An ambient light sensor (`sensor.xxx`) with a lux threshold to avoid turning lights on when it’s already bright.
- Adjustable brightness level for lights when turning them on.

## 📥 Import Blueprint
[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FHoM3r17%2Fvacuum-lights%2Frefs%2Fheads%2Fmain%2Fsmart-vacuum-lights-sync)


## 🛠️ Setup
1. Copy the blueprint YAML file into your `blueprints/automation/` folder. *(Only needed if the import button above cannot be used.)*
2. Restart Home Assistant or reload automations. *(Only needed if the import button above cannot be used.)*
3. Create a new automation using this blueprint.
4. Fill in the inputs:
   - Vacuum entity.
   - Room sensor.
   - Mapping of rooms → lights (YAML or JSON).
   - (Optional) Presence entities, light sensor, brightness level.

## 📋 Example mapping of rooms → lights

Room names must match exactly the values reported by your room sensor.
A room can point to a single light or to a list of lights.

YAML:
```yaml
Bathroom: light.lumieres_sdb
Dining Hall: light.lumieres_sam
Living Room:
  - light.lumieres_salon_1
  - light.lumieres_salon_2
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

## 🔄 Upgrading from V1.00
- Existing automations keep working: the old JSON text mapping is still accepted.
- Brightness `0` is now treated as "keep last brightness" (it used to turn the light off).
- Lights that someone turned on manually may be turned off when the vacuum leaves or stops in that room.
