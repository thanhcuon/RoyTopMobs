# RoyTopMobs (v2026.1)

[![Version](https://img.shields.io/badge/version-2026.1-orange.svg)](https://www.spigotmc.org/resources/roytopmobs.126886/)
![Minecraft](https://img.shields.io/badge/minecraft-1.18.2%20--%201.21.x+-brightgreen.svg)
![Java](https://img.shields.io/badge/java-17+-blue.svg)
[![MythicMobs](https://img.shields.io/badge/MythicMobs-5.12.1+-purple.svg)](https://modrinth.com/plugin/mythicmobs)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-5865F2?logo=discord&logoColor=white)](https://discord.gg/zNpkXGFQRS)

**RoyTopMobs** is an enterprise-grade Minecraft server plugin engineered to turn boss encounters into competitive, high-stakes community events. It accurately tracks real-time damage dealt to **MythicMobs** and vanilla entities, calculates health-based ranking leaderboards, broadcasts rich announcements, manages persistent cooldowns across server restarts, and dispatches customizable console and player rewards.

---

## 📖 Overview

Designed from the ground up for modern Minecraft RPG, Survival, Skyblock, and Factions networks, **RoyTopMobs** features a modular multi-file architecture:

- **Modular Mob Definitions (`plugins/RoyTopMobs/tracked-mob/`):** Add unlimited individual boss YAML files.
- **Multi-Language Support (`plugins/RoyTopMobs/message/`):** Instant switching between English (`en.yml`) and Vietnamese (`vi.yml`).
- **Persistent Cooldown Data (`plugins/RoyTopMobs/respawn-data.yml`):** Cooldown timestamps and active boss states are preserved across server restarts and `/rtm reload` calls.
- **Native Floating Holograms:** Built-in Minecraft 1.19.4+ `TEXT_DISPLAY` hologram support (no external hologram plugin required), with full compatibility for **FancyHolograms**, **DecentHolograms**, and 1.18.2 ArmorStand fallbacks.
- **Rich Discord Webhooks:** Sends customizable Discord embeds on boss summon, boss defeat, and reward distribution with multi-line damage rankings.
- **Database Statistics:** Asynchronous SQLite and MySQL storage powered by **HikariCP** to track lifetime player kills and damage leaderboards.

---

## ⚡ Key Features

- **📁 Modular Multi-File Boss Configuration:** Define bosses independently in `tracked-mob/<mob_id>.yml`. Each file controls summon conditions, title/sound broadcasts, rewards, holograms, and webhooks.
- **⚔️ Accurate Damage & Percentage Tracking:** Tracks melee attacks, arrows, tridents, and projectiles. Computes exact damage contribution percentages relative to the boss's true maximum health.
- **🛡️ Exclusive RoyTopMobs Entity Tracking (PDC):** Uses Minecraft `PersistentDataContainer` (PDC) tagging to track only mobs summoned by RoyTopMobs (`/rtm spawn`, respawn timers, or fixed schedules). Mobs spawned from external commands (`/mm mobs spawn`, `/summon`) or natural spawners are completely ignored.
- **⏳ Persistent Respawn Cooldowns (`respawn-data.yml`):** Cooldown timestamps and boss states persist across server restarts and `/rtm reload` commands. Bosses will never prematurely respawn after a reboot.
- **🎁 Flexible Multi-Tiered Rewards:**
  - Tiered ranking rewards for Top #1 to Top #N fighters.
  - Participation rewards with customizable minimum damage qualification thresholds.
  - Final blow (last-hit) killer rewards.
  - Console and player command dispatching with dynamic placeholders (`%player%`, `{damage}`, `{percentage}`, `{mobname}`).
- **🤖 Rich Discord Webhook Embeds:** Sends automated Discord embed alerts on boss spawn, boss defeat, and reward distribution with clean multi-line ranking leaderboards.
- **🔮 Native Floating Hologram Countdowns:** Supports Minecraft 1.19.4+ `TEXT_DISPLAY` entities out-of-the-box, with support for **FancyHolograms**, **DecentHolograms**, and ArmorStands.
- **📏 Customizable Hologram Height Offset (`hologram.y-offset`):** Configure the exact floating altitude above the boss spawn point.
- **🎯 Line-of-Sight Spawn Point Setting:** `/rtm setspawn <mob_id>` uses raycasting to automatically lock onto the targeted block in the player's crosshairs.
- **🌈 Full RGB Hex & Gradient Support:** Supports MiniMessage tags, hex colors (`&#RRGGBB`, `#RRGGBB`), and multi-stop nested gradients (`<gradient:#FF4500:#FFA500>Text</gradient>`) with formatting preservation.
- **💾 Async Database Statistics (HikariCP):** Asynchronously records player kills, lifetime damage, and leaderboards using SQLite or MySQL.
- **🔄 Automatic Configuration Migration (`ConfigMigrator`):** Automatically detects and inserts new configuration keys into existing files upon plugin updates without overwriting custom settings.

---

## 📊 PlaceholderAPI Placeholders

RoyTopMobs provides a comprehensive suite of PlaceholderAPI expansions (replace `<mob>` with the mob ID configured in `tracked-mob/`, and `<1-N>` with the rank number):

| Placeholder | Description / Output |
| :--- | :--- |
| `%roytopmobs_spawned_<mob>%` | Returns whether the boss is currently alive (`Yes` or `No`). |
| `%roytopmobs_cooldown_<mob>%` | Returns formatted time remaining until respawn (e.g. `4m 30s`) or `Ready`. |
| `%roytopmobs_respawn_<mob>%` | Returns the full colorized respawn countdown message configured in language files. |
| `%roytopmobs_needed_<mob>%` | Returns remaining kill condition requirements (e.g. `ZOMBIE: 3/10, SKELETON: 0/5`). |
| `%roytopmobs_top_<mob>_<1-N>%` | Returns the full formatted damage leaderboard line for rank position `<1-N>`. |
| `%roytopmobs_top_<mob>_<1-N>_name%` | Returns the player username at rank position `<1-N>`. |
| `%roytopmobs_top_<mob>_<1-N>_damage%` | Returns the raw damage value dealt by the player at rank position `<1-N>`. |
| `%roytopmobs_top_<mob>_<1-N>_damage_formatted%` | Returns comma-formatted damage (e.g. `15,250.50`) dealt by rank position `<1-N>`. |
| `%roytopmobs_top_<mob>_<1-N>_percent%` | Returns damage percentage (e.g. `32.5%`) dealt relative to boss max health. |
| `%roytopmobs_player_kills_<mob>%` | Returns the viewing player's total lifetime kills for that specific boss from the database. |
| `%roytopmobs_player_damage_<mob>%` | Returns the viewing player's total lifetime damage dealt to that specific boss. |

---

## ⚙️ Commands & Permissions

Base command: `/roytopmob` (Aliases: `/rtm`)

| Command | Description | Permission Node | Default |
| :--- | :--- | :--- | :--- |
| `/rtm reload` | Reloads all configuration files, language strings, and mob definitions. | `roytopmob.reload` | OP |
| `/rtm toggle` | Toggles damage tracking globally on the server. | `roytopmob.toggle` | OP |
| `/rtm spawn <mob_id>` | Spawns a tracked boss mob at its configured spawn point or target block. | `roytopmob.spawn` | OP |
| `/rtm setspawn <mob_id>` | Sets the boss spawn point on the targeted block in the player's crosshairs. | `roytopmob.setspawn` | OP |
| `/rtm deletespawn <mob_id>` | Deletes the saved spawn point and removes its hologram timer. | `roytopmob.deletespawn` | OP |
| `/rtm help` | Displays the in-game command help menu. | *None* | Everyone |

> **Admin Notification:** Players with permission `roytopmob.update` receive in-game update notifications on join.

---

## 📁 Configuration Wiki

<details>
<summary><b>⚙️ 1. Main Configuration (config.yml)</b></summary>

```yaml
# ==============================================================================
#                      RoyTopMobs Main Configuration (v2026.1)
# ==============================================================================
# Language selection: en (English) or vi (Vietnamese)
# The corresponding language file is loaded from message/en.yml or message/vi.yml
message: en

# bStats metrics settings
metrics:
  enabled: true
  server-uuid: ''
  log-startup: true
  error-reporting: true

# Database configuration (Stores player kills, total damage, and statistics)
# Supported types: SQLITE (local database file) or MYSQL (remote database with connection pool)
database:
  type: SQLITE # SQLITE or MYSQL
  sqlite:
    file: "database.db"
  mysql:
    host: "localhost"
    port: 3306
    database: "roytopmobs"
    username: "root"
    password: ""
    use-ssl: false
    pool:
      maximum-pool-size: 10
      minimum-idle: 2
      connection-timeout: 30000

# Global placeholder settings (used when generating PlaceholderAPI values)
placeholder:
  damage-format: "#,###.##"
  percentage-format: "#.#"
  no-data: "No data"
  no-player: "No player"
  ready: "Ready"
  spawned-yes: "Yes"
  spawned-no: "No"
```
</details>

<details>
<summary><b>🌐 2. English Language File (message/en.yml)</b></summary>

```yaml
# ==============================================================================
#                      RoyTopMobs English Language File (en.yml)
# ==============================================================================
# Supports: Standard color codes (&a, &c), Hex Colors (#RRGGBB, &#RRGGBB),
# Gradients: <gradient:#color1:#color2>Text</gradient>
# ==============================================================================

prefix: "<gradient:#FFA500:#FF4500>&l[RoyTopMobs]</gradient> &8» "

reload: "<gradient:#00FF7F:#00CED1>&l✔</gradient> &aAll configurations and mob data have been reloaded successfully!"
toggle-on: "<gradient:#00FF7F:#00CED1>&l✔</gradient> &aDamage tracking has been globally enabled!"
toggle-off: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cDamage tracking has been globally disabled!"
no-permission: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cYou do not have permission to execute this command!"
only-players: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cOnly in-game players can execute this command!"
mob-not-tracked: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cBoss &e{mobtype} &cis not configured in the tracked-mob folder!"

spawn-success: "<gradient:#00FF7F:#00CED1>&l✔</gradient> &aSpawned boss &e{mobtype} &asuccessfully at your location!"
spawn-fail: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cFailed to spawn boss &e{mobtype}&c! Please check configuration."
setspawn-success: "<gradient:#00FF7F:#00CED1>&l✔</gradient> &aSpawn location saved for boss &e{mobtype}&a!"
deletespawn-success: "<gradient:#00FF7F:#00CED1>&l✔</gradient> &aSpawn location deleted for boss &e{mobtype}&a!"
deletespawn-fail: "<gradient:#FF4500:#DC143C>&l✖</gradient> &cFailed to delete spawn location for boss &e{mobtype}&c!"

usage-spawn: "&eUsage: &f/roytopmob spawn <mob_id>"
usage-setspawn: "&eUsage: &f/roytopmob setspawn <mob_id>"
usage-deletespawn: "&eUsage: &f/roytopmob deletespawn <mob_id>"

help-menu:
  - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  - "<gradient:#FFA500:#FF4500>&l       ROYTOPMOBS COMMAND SYSTEM (v2026.1)</gradient>"
  - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  - " &e• &f/rtm reload &8- &7Reloads all configurations and mob files"
  - " &e• &f/rtm toggle &8- &7Toggles server-wide damage tracking"
  - " &e• &f/rtm spawn <mob_id> &8- &7Spawns a boss immediately"
  - " &e• &f/rtm setspawn <mob_id> &8- &7Saves current location as spawn point"
  - " &e• &f/rtm deletespawn <mob_id> &8- &7Deletes saved spawn point"
  - " &e• &f/rtm help &8- &7Displays this help menu"
  - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

actionbar:
  format: "<gradient:#FFA500:#FFFF00>⚔ Damage:</gradient> &f{damage} &7(&e{percentage}%&7)"

messages:
  respawn-countdown: "&c✦ &e{mobname} &7will respawn in: &f{time}&7!"
  mob-ready: "&aReady"
  threshold-min-damage: "&cYou need to deal at least &e{damage} &cdamage to {mobname} to qualify for rankings!"
  threshold-max-damage: "&cYou reached the maximum tracked damage cap (&e{damage}&c) for {mobname}!"
  spawn-condition-failed: "&cSummon conditions for {mobname} have not been met!"
  participation-reward-received: "&aYou received a participation reward!"
  lasthit-reward-received: "&e{player} &areceived the final hit reward!"
```
</details>

<details>
<summary><b>📖 3. Tracked Mob Tutorial Guide (tracked-mob/_example.yml)</b></summary>

```yaml
# ==============================================================================
#                    RoyTopMobs Tracked Mob Template (_example.yml)
# ==============================================================================
# IMPORTANT NOTICE:
# 1. This file is a TUTORIAL TEMPLATE and is NOT loaded in-game.
# 2. Files starting with '_' (like _example.yml) are IGNORED by RoyTopMobs.
# 3. To create a new boss, duplicate this file as [mob_id].yml (e.g., eliteskeleton.yml).
# 4. This _example.yml file is automatically regenerated on reload if deleted.
# ==============================================================================

# Mark as documentation example (RoyTopMobs will never load this as an active boss)
is-example: true

# Mob Type (Supports MythicMobs and Vanilla Entity Types)
# Format: "mythicmobs:[mythicmob_internal_id]" or "vanilla:[ENTITY_TYPE]"
mob-type: "mythicmobs:EliteSkeleton"

# Display name of the Boss (Supports Hex: #RRGGBB, Gradient: <gradient:#color1:#color2>...</gradient>, Legacy: &c)
name: "<gradient:#FF4500:#FFA500>&lElite Skeleton</gradient>"

# Summon & Spawning Settings (Spawn)
spawn:
  max-mob-spawn: 1
  mob-per-spawn: 1

  condition:
    enabled: false
    max-player: 50
    kill-in-radius:
      radius: 50
      mob:
        ZOMBIE: 10
        SKELETON: 10
        "MYTHICMOBS:MinionSkeleton": 5

  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#FFA500:#FF4500>&l[BOSS SUMMONED]</gradient> &c✦ &f{mobname} &7has appeared at &e{location}&7!"
      - "&7Defeat the boss to claim legendary damage rewards!"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

  title:
    enabled: true
    title:
      - "<gradient:#FF0000:#FF4500>&lBOSS SUMMONED!</gradient>"
    subtitle:
      - "&f{mobname} &7has awakened at &e{location}"
    fade-in: 10
    stay: 50
    fade-out: 10

  sound:
    enabled: true
    type: "ENTITY_ENDER_DRAGON_GROWL"
    volume: 1.0
    pitch: 1.0

  effect:
    enabled: false
    effect: "GLOWING"
    level: 1
    duration: 60

# Death Announcements & Rewards (Death)
death:
  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#00FF7F:#00CED1>&l[BOSS DEFEATED]</gradient> &c✦ &f{mobname} &7has been slain by &e{killer}&7!"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

  title:
    enabled: true
    title:
      - "<gradient:#00FF7F:#00CED1>&lBOSS DEFEATED!</gradient>"
    subtitle:
      - "&7Final Blow: &e{killer}"
    fade-in: 10
    stay: 50
    fade-out: 10

  sound:
    enabled: true
    type: "UI_TOAST_CHALLENGE_COMPLETE"
    volume: 1.0
    pitch: 1.0

  effect:
    enabled: false
    effect: "REGENERATION"
    level: 1
    duration: 100

# Respawn Mechanism (Timer Cooldown or Fixed Schedule)
respawn:
  time:
    enabled: true
    time: 300 # Seconds until respawn (5 minutes)

  schedule:
    enabled: false
    time-zone: "SYSTEM"
    schedule:
      - "DAILY;08:00:00"
      - "DAILY;12:00:00"
      - "DAILY;18:30:00"
      - "SUNDAY;20:00:00"

# Reward System (Rankings, Thresholds, Final Blow, Participation)
reward:
  enabled: true

  damage-settings:
    enabled: false
    min-damage: 100.0
    max-damage: 10000.0

  top-damage:
    1:
      - "console: eco give %player% 5000"
      - "console: give %player% diamond 10"
      - "player: me I achieved Rank 1 Damage against {mobname}!"
    2:
      - "console: eco give %player% 2500"
      - "console: give %player% diamond 5"
    3:
      - "console: eco give %player% 1000"
      - "console: give %player% diamond 2"

  killer:
    - "console: eco give %killer% 1000"
    - "console: give %killer% nether_star 1"

  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#FFA500:#FFD700>&l         🏆 TOP DAMAGE DEALERS 🏆</gradient>"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - " &e🥇 &f#1 &6{top_1_name} &8» &e{top_1_damage_formatted} DMG &7(&a{top_1_percent}%&7)"
      - " &f🥈 &f#2 &f{top_2_name} &8» &e{top_2_damage_formatted} DMG &7(&a{top_2_percent}%&7)"
      - " &c🥉 &f#3 &c{top_3_name} &8» &e{top_3_damage_formatted} DMG &7(&a{top_3_percent}%&7)"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

# Hologram Countdown at the Spawn Point
hologram:
  enabled: true
  type: "TEXT_DISPLAY"
  y-offset: 3.0
  line:
    - "<gradient:#FF4500:#FFA500>&l✦ {mobname} ✦</gradient>"
    - "&7Respawning in: &f{time}"

# Discord Webhook Notifications
discord-webhook:
  enabled: false
  webhook: "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL_HERE"

  spawn:
    enabled: true
    title: "⚔️ WORLD BOSS SUMMONED ⚔️"
    description: "A legendary boss has awakened! Gather your allies and head to the battlefield immediately!"
    color: 65280
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    fields:
      - name: "👾 Boss Name"
        value: "**{mobname}**"
        inline: true
      - name: "📍 Spawn Location"
        value: "```{location}```"
        inline: true

  death:
    enabled: true
    title: "☠️ WORLD BOSS SLAIN ☠️"
    description: "The fearsome **{mobname}** has fallen in battle! All qualifying fighters have received their rewards."
    color: 16711680
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    fields:
      - name: "🗡️ Final Hit"
        value: "👑 **{killer}**"
        inline: true
      - name: "📍 Defeat Location"
        value: "```{location}```"
        inline: true

  reward:
    enabled: true
    title: "🏆 TOP DAMAGE RANKINGS 🏆"
    description: "Here are the top fighters who dealt the highest damage to **{mobname}**:\n\n{top_damage_list}"
    color: 16766720
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    format-line: "{medal} **#{position}** `{player}` — **{damage}** DMG ({percentage})"
    fields:
      - name: "🗡️ Final Hit"
        value: "👑 **{killer}**"
        inline: true
      - name: "💥 Total Damage Dealt"
        value: "```{total_damage}```"
        inline: true
      - name: "🎁 Reward Status"
        value: "✅ **All top rankings distributed**"
        inline: false

# Spawn Location Data (Automatically updated via /rtm setspawn [mob_id])
spawn-data:
  location:
    world: ""
    x: 0.0
    y: 0.0
    z: 0.0
    yaw: 0.0
    pitch: 0.0
```
</details>

<details>
<summary><b>👾 4. Active Mob Configuration (tracked-mob/eliteskeleton.yml)</b></summary>

```yaml
# ==============================================================================
#                    RoyTopMobs Tracked Mob (eliteskeleton.yml)
# ==============================================================================

mob-type: "mythicmobs:EliteSkeleton"
name: "<gradient:#FF4500:#FFA500>&lElite Skeleton</gradient>"

spawn:
  max-mob-spawn: 1
  mob-per-spawn: 1
  condition:
    enabled: false
    max-player: 50
    kill-in-radius:
      radius: 50
      mob:
        ZOMBIE: 10
        SKELETON: 10
  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#FFA500:#FF4500>&l[BOSS SUMMONED]</gradient> &c✦ &f{mobname} &7has appeared at &e{location}&7!"
      - "&7Gather your allies and defeat the boss for epic rewards!"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  title:
    enabled: true
    title:
      - "<gradient:#FF0000:#FF4500>&lBOSS SUMMONED!</gradient>"
    subtitle:
      - "&f{mobname} &7has risen at &e{location}"
    fade-in: 10
    stay: 50
    fade-out: 10
  sound:
    enabled: true
    type: "ENTITY_ENDER_DRAGON_GROWL"
    volume: 1.0
    pitch: 1.0
  effect:
    enabled: false
    effect: "GLOWING"
    level: 1
    duration: 60

death:
  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#00FF7F:#00CED1>&l[BOSS DEFEATED]</gradient> &c✦ &f{mobname} &7was slain by &e{killer}&7!"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  title:
    enabled: true
    title:
      - "<gradient:#00FF7F:#00CED1>&lBOSS DEFEATED!</gradient>"
    subtitle:
      - "&7Final Hit: &e{killer}"
    fade-in: 10
    stay: 50
    fade-out: 10
  sound:
    enabled: true
    type: "UI_TOAST_CHALLENGE_COMPLETE"
    volume: 1.0
    pitch: 1.0
  effect:
    enabled: false
    effect: "REGENERATION"
    level: 1
    duration: 100

respawn:
  time:
    enabled: true
    time: 300
  schedule:
    enabled: false
    time-zone: "SYSTEM"
    schedule:
      - "DAILY;08:00:00"
      - "DAILY;12:00:00"
      - "DAILY;18:30:00"

reward:
  enabled: true
  damage-settings:
    enabled: false
    min-damage: 100.0
    max-damage: 10000.0
  top-damage:
    1:
      - "console: eco give %player% 5000"
      - "console: give %player% diamond 10"
      - "player: me I achieved Rank 1 Damage against {mobname}!"
    2:
      - "console: eco give %player% 2500"
      - "console: give %player% diamond 5"
    3:
      - "console: eco give %player% 1000"
      - "console: give %player% diamond 2"
  killer:
    - "console: eco give %killer% 1000"
    - "console: give %killer% nether_star 1"
  broadcast:
    enabled: true
    line:
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - "<gradient:#FFA500:#FFD700>&l         🏆 TOP DAMAGE DEALERS 🏆</gradient>"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      - " &e🥇 &f#1 &6{top_1_name} &8» &e{top_1_damage_formatted} DMG &7(&a{top_1_percent}%&7)"
      - " &f🥈 &f#2 &f{top_2_name} &8» &e{top_2_damage_formatted} DMG &7(&a{top_2_percent}%&7)"
      - " &c🥉 &f#3 &c{top_3_name} &8» &e{top_3_damage_formatted} DMG &7(&a{top_3_percent}%&7)"
      - "&8&m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

hologram:
  enabled: true
  type: "TEXT_DISPLAY"
  y-offset: 3.0
  line:
    - "<gradient:#FF4500:#FFA500>&l✦ {mobname} ✦</gradient>"
    - "&7Respawning in: &f{time}"

discord-webhook:
  enabled: false
  webhook: ""
  spawn:
    enabled: true
    title: "⚔️ WORLD BOSS SUMMONED ⚔️"
    description: "A fearsome boss has awakened in the world! Gather your gear and prepare for battle!"
    color: 65280
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    fields:
      - name: "👾 Boss Name"
        value: "**{mobname}**"
        inline: true
      - name: "📍 Spawn Location"
        value: "```{location}```"
        inline: true
  death:
    enabled: true
    title: "☠️ WORLD BOSS SLAIN ☠️"
    description: "The boss **{mobname}** has met its demise! All participants have been rewarded."
    color: 16711680
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    fields:
      - name: "🗡️ Final Hit"
        value: "👑 **{killer}**"
        inline: true
      - name: "📍 Defeat Location"
        value: "```{location}```"
        inline: true
  reward:
    enabled: true
    title: "🏆 TOP DAMAGE RANKINGS 🏆"
    description: "Here are the top fighters who dealt the highest damage to **{mobname}**:\n\n{top_damage_list}"
    color: 16766720
    thumbnail: "https://minotar.net/helm/MHF_Skeleton/64.png"
    footer: "RoyTopMobs • World Boss System"
    format-line: "{medal} **#{position}** `{player}` — **{damage}** DMG ({percentage})"
    fields:
      - name: "🗡️ Final Hit"
        value: "👑 **{killer}**"
        inline: true
      - name: "💥 Total Damage Dealt"
        value: "```{total_damage}```"
        inline: true
      - name: "🎁 Reward Status"
        value: "✅ **All top rankings distributed**"
        inline: false

spawn-data:
  location:
    world: ""
    x: 0.0
    y: 0.0
    z: 0.0
    yaw: 0.0
    pitch: 0.0
```
</details>

<details>
<summary><b>💾 5. Persistent Respawn Data (respawn-data.yml)</b></summary>

```yaml
# ==============================================================================
#                  RoyTopMobs Persistent Respawn Data
# ==============================================================================
# This file stores live respawn countdown timestamps and active mob states.
# Automatically updated by the plugin to persist cooldowns across server restarts.
# ==============================================================================

respawns:
  eliteskeleton:
    spawned: false
    respawn-at: 1788289900000
    last-death: 1788289600000
```
</details>

---

## 📦 Step-by-Step Installation Guide

1. **Download:** Download the latest `RoyTopMobs-2026.1.jar` release.
2. **Server Environment:** Ensure you are running Java 17+ and Spigot, Paper, or Purpur (*Minecraft 1.18.2 – 1.21.x+*).
3. **Supported Plugins & Integrations (Optional):**
   - [MythicMobs](https://modrinth.com/plugin/mythicmobs) — *Create custom MMORPG boss monsters, advanced skills, and custom AI mechanics.*
   - [PlaceholderAPI](https://modrinth.com/plugin/placeholderapi) — *Display boss cooldowns, alive status, and top damage stats in TAB, Scoreboards, and GUIs.*
   - [FancyHolograms](https://modrinth.com/plugin/fancyholograms) — *Display floating holographic countdowns above boss spawn locations.*
   - [DecentHolograms](https://modrinth.com/plugin/decentholograms) — *Display floating holographic countdowns above boss spawn locations.*
   > *Note: On Minecraft 1.19.4+ servers, RoyTopMobs natively supports `TEXT_DISPLAY` holograms without requiring any external hologram plugin!*
4. **Installation:**
   - Place `RoyTopMobs-2026.1.jar` (and your chosen optional plugins) into your server's `plugins/` directory.
   - Start or restart your server to generate default configurations.
5. **Setting Boss Spawn Points:**
   - In-game, stand in front of where you want the boss to appear, aim your crosshairs at the target block, and run:
     ```
     /rtm setspawn <mob_id>
     ```
6. **Customization & Reload:**
   - Customize your boss settings in `plugins/RoyTopMobs/tracked-mob/<mob_id>.yml` and apply changes anytime using `/rtm reload`.

---

## 💬 Community Support & Feedback

If you find **RoyTopMobs** helpful for your server, please take a moment to leave a **⭐⭐⭐⭐⭐ 5-Star Review** on SpigotMC!

- **Need Support / Found a Bug?** Join our official [Discord Community Server](https://discord.gg/zNpkXGFQRS) for direct assistance.
