# Smart Vacuum & Light Sync V1.0

Synchronize your lights with your vacuum robot.  
Lights turn on when the vacuum starts or enters a room, and turn off when it stops or leaves.  
Optional conditions for **presence** and **ambient light** make the automation even smarter.  
You can also configure the **brightness level (0–100%)** or simply turn on the lights without changing their brightness.

---

### Import Blueprint
[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FHoM3r17%2Fvacuum-lights%2Frefs%2Fheads%2Fmain%2Fsmart-vacuum-lights-sync)

---

### Requirements
- **Vacuum entity** (`vacuum.xxx`)
- **Room sensor** (`sensor.xxx`) → must report the current room name
- **Room → Light mapping (JSON)**  
  Example:
  ```json
  {
    "Bathroom": "light.lumieres_sdb",
    "Dining Hall": "light.lumieres_sam",
    "Living Room": "light.lumieres_salon",
    "Kitchen": "light.lumieres_cuisine"
  }

Presence entities (optional) → list of person.xxx

Light sensor (optional) → sensor.xxx reporting lux

Light threshold (optional) → default: 100 lux

Light brightness (optional) → percentage (0–100). Leave empty to just turn on the light without changing brightness.

Features

🔄 Sync lights with vacuum state (cleaning, paused, docked, etc.)

💡 Turn lights on when vacuum enters a room

🚪 Turn lights off when vacuum leaves or stops

👥 Optional presence check (only run if nobody is home)

🌞 Optional ambient light check (only run if lux < threshold)

🔆 Configurable brightness (0–100%) or default light activation

🛠 Restart mode ensures automation always follows the latest vacuum state

License

This project is licensed under the MIT License
See the LICENSE file for full details.
