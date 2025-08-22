# RoyTopMobs Wiki

**RoyTopMobs** is a powerful Minecraft plugin that tracks damage dealt to **MythicMobs**, manages rewards, and provides rich display features for epic boss battles.

---

## 📖 Table of Contents

* Dependencies
* Features
* Configuration
* Commands
* Permissions
* PlaceholderAPI
* Health Announcements
* Discord Integration
* Movement Restriction
* Hologram Support
* Respawn System
* Common Issues & Solutions
* Support

---

## 📦 Dependencies

* MythicMobs (Required)
* PlaceholderAPI (Optional – placeholders)
* FancyHolograms or DecentHolograms (Optional – holograms)

---

## ✨ Features

* Track damage dealt to specific MythicMobs
* Customizable reward system based on damage rankings
* Health percentage announcements
* Titles and sounds on spawn/death
* Discord webhook integration
* Movement restriction for bosses
* Respawn system with countdowns
* PlaceholderAPI support
* Hologram support

---

## ⚙️ Configuration

Basic Setup:

1. Install the plugin and required dependencies.
2. Configure `tracked-mobs` in config.yml.
3. Set spawn locations with `/rtm setspawn <mobtype>`.

Tracked Mobs Example:

```yaml
tracked-mobs:
  EliteSkeleton:
    display-name: "&c&lElite Skeleton"
    announcement:
      - "&c&lElite Skeleton &7has appeared at &f{location}!"
    respawn-time: 300
    rewards:
      top-1:
        - "give {player} diamond 5"
        - "eco give {player} 5000"
damage-settings:
  threshold:
    enabled: true
    default:
      minimum-damage: 1000.0
      message: "&cYou need to deal at least {damage} damage!"
display:
  actionbar:
    enabled: true
    format: "&6Damage: &f{damage} &7(&e{percentage}%&7)"
  spawn-title:
    enabled: true
    title: "&c✦ &l{mobname} &c✦"
boss-health:
  announcements:
    enabled: true
    format: "&c&l{mobname} &fhas reached &c{percentage}% &fhealth!"
    thresholds:
      - 70
      - 50
      - 30
discord:
  webhook:
    enabled: true
    url: "your-webhook-url"
    spawn-format: "{mobname} has spawned at {location}"
    death-format: "{mobname} has been defeated!"
mob-settings:
  movement-restriction:
    enabled: true
    max-distance: 7
    check-interval: 2
    teleport-back: true
hologram:
  provider: FANCY
```

Commands:

```
/rtm reload - Reload configuration
/rtm toggle - Toggle damage tracking
/rtm setspawn <mobtype> - Set spawn location
/rtm spawn <mobtype> - Spawn a tracked mob
/rtm info - Show plugin information
```

Permissions:

```
roytopmobs.admin - Access to all commands
roytopmobs.reload - Reload configuration
roytopmobs.toggle - Toggle tracking
roytopmobs.setspawn - Set spawn points
roytopmobs.spawn - Spawn mobs
```

PlaceholderAPI:

```
%roytopmobs_respawn_<mobtype>% - Shows respawn time
%roytopmobs_top_<mobtype>_<position>% - Shows top damager info
```

Respawn System:

* Configurable respawn times per mob
* Visual countdown with holograms
* Automatic respawn at configured locations

Common Issues & Solutions:

Mobs not spawning:

* Check if MythicMobs is installed.
* Ensure internal mob names match MythicMobs config.
* Verify spawn locations are correctly set.

Rewards not working:

* Check console for command execution errors.
* Verify reward command syntax.
* Ensure required economy plugins are installed.

Placeholders not working:

* Verify PlaceholderAPI is installed.
* Ensure placeholders are properly formatted.
* Reload PlaceholderAPI with `/papi reload`.

Support:

* Create an issue on GitHub
* Join our Discord server
* Contact via email: [thanhss119@gmail.com](mailto:thanhss119@gmail.com)
