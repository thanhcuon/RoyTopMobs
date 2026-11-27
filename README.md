<div align="center">

![RoyTopMobs Banner](https://i.imgur.com/lr0hZ8u.jpeg)

# RoyTopMobs (v2026.5.0)

[![Version](https://img.shields.io/badge/Version-2026.5.0-orange?style=for-the-badge)](https://bstats.org/plugin/bukkit/RoyTopMobs/26207)
![Minecraft](https://img.shields.io/badge/Minecraft-1.18%20--%2026.3-brightgreen?style=for-the-badge)
![Java](https://img.shields.io/badge/Java-17%2B-blue?style=for-the-badge)
![Platforms](https://img.shields.io/badge/Platforms-Spigot%20%7C%20Paper%20%7C%20Purpur%20%7C%20Folia%20%7C%20Leaf-blueviolet?style=for-the-badge)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/zNpkXGFQRS)
[![bStats](https://img.shields.io/badge/bStats-26207-teal?style=for-the-badge)](https://bstats.org/plugin/bukkit/RoyTopMobs/26207)

**Turn every boss fight into an exciting server-wide competition!**  
**RoyTopMobs** tracks real-time player damage on **MythicMobs** and **Vanilla** bosses, builds live damage leaderboards, hands out tiered rewards, manages persistent respawn timers & holograms, and sends rich **Discord Webhook** alerts — all with full **Folia** and **Minecraft 1.18 – 26.3** support.

</div>

---

<a id="table-of-contents"></a>
## 📑 Table of Contents

- [🌟 Overview & Key Features](#overview-and-key-features)
- [🔄 How It Works](#how-it-works)
- [📋 Requirements & Supported Platforms](#requirements-and-supported-platforms)
- [🚀 Quick Installation Guide](#quick-installation-guide)
- [⚙️ Configuration Overview](#configuration-overview)
- [🛡️ Commands & Permissions](#commands-and-permissions)
- [📊 PlaceholderAPI Placeholders](#placeholderapi-placeholders)
- [🤖 Discord Webhook Guide](#discord-webhook-guide)
- [📈 Live bStats Server Statistics](#live-bstats-server-statistics)
- [💬 Support & Community](#support-and-community)

---

<a id="overview-and-key-features"></a>
## 🌟 Overview & Key Features

Whether you run an RPG, Survival, Skyblock, or Factions server, **RoyTopMobs** makes boss events easy to set up and rewarding for players:

- **🛡️ Exclusive PDC Entity Tracking:** Uses Minecraft's `PersistentDataContainer` (PDC) so only bosses spawned by RoyTopMobs (`/rtm spawn`, respawn timers, or schedules) are tracked. Mobs from `/mm mobs spawn`, `/summon`, or natural spawners are ignored.
- **⚔️ Real-Time Damage Leaderboard:** Accurately tracks player damage and calculates exact percentage contributions relative to the boss's maximum health.
- **🎁 Tiered & Flexible Rewards:** Configure **Top #1 to Top #N** rewards, **`*` participation** rewards (with `min-damage` and `max-damage` thresholds), and **`killer`** final-hit rewards using `console:` or `player:` commands and broadcasts.
- **⏳ Persistent Cooldowns & Schedules:** Cooldowns are saved in `respawn-data.yml` so timers survive restarts and reloads. Choose between interval timers (`respawn.time` in seconds) or fixed weekly/daily schedules (`respawn.schedule` with `time-zone` and `DAY;HH:mm:ss`).
- **📏 Boss Leash Range (`lease-range`):** Keep bosses inside their arena by automatically tethering them back if they wander too far from their spawn point.
- **🔮 Native & Plugin Holograms:** Built-in `TEXT_DISPLAY` floating countdowns on **1.19.4+** (with `ArmorStand` fallback on **1.18 – 1.19.3**), plus optional **FancyHolograms** and **DecentHolograms** integration.
- **🤖 Overhauled Discord Webhooks:** Send up to **10 embeds** per message, customize `username`, `avatar-url`, and `content` per event (`spawn`, `death`, `reward`), post to Threads or Forums, use silent notifications, and display dynamic `top-limit` leaderboards.
- **🌐 14 Built-In Languages:** Switch instantly in `config.yml` between `en`, `vi`, `es`, `pt_br`, `fr`, `de`, `ru`, `zh_cn`, `ja`, `ko`, `id`, `tr`, `pl`, and `it` — with full Legacy `&`, Hex (`#RRGGBB`, `&#RRGGBB`), and `<gradient:#c1:#c2>...</gradient>` support.
- **⚡ Dynamic Command Aliases:** Add unlimited custom aliases in `config.yml` (`command-aliases`) and apply them immediately with `/rtm reload` — no server restart needed, complete with permission-aware tab completion.
- **🔄 Lossless Config Migration:** Update safely with `/rtm migrate`! Comments and key ordering are preserved, and automatic timestamped backups are saved in `backups/<timestamp>/`.
- **💾 SQLite & MySQL Storage:** Stores lifetime player stats (`kills`, `total_damage`, `last_damage`, `highest_damage`, `last_world`, `first_kill`, `last_kill`) with periodic `cache-refresh-seconds` for live database edits.

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="how-it-works"></a>
## 🔄 How It Works

Here is the simple lifecycle of a RoyTopMobs boss encounter from start to finish:

1. **Spawn:** A boss spawns via `/rtm spawn <mob_id>`, an automatic respawn timer, or a fixed schedule (after checking optional `spawn.condition` rules like `max-player` or `kill-in-radius`). RoyTopMobs tags the entity with PDC and triggers any configured spawn broadcast, title, sound, potion effect, and Discord webhook.
2. **Combat:** Players attack the boss. RoyTopMobs records each player's damage in real time and keeps the boss within `spawn.lease-range` if configured.
3. **Death:** When the boss is defeated, RoyTopMobs finalizes the damage rankings and saves player lifetime statistics to SQLite or MySQL. Death broadcasts, titles, sounds, effects, and Discord webhooks fire automatically.
4. **Leaderboard & Rewards:** Players who meet `min-damage` (and within `max-damage`) are ranked **#1, #2, #3...** and receive their tiered `top-damage` rewards, `*` participation rewards, and `killer` last-hit rewards.
5. **Respawn:** The persistent cooldown (`respawn.time` or `respawn.schedule`) begins and is saved to `respawn-data.yml`. If holograms are enabled, a live countdown appears above the spawn location until the boss returns!

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="requirements-and-supported-platforms"></a>
## 📋 Requirements & Supported Platforms

| Requirement | Details |
| :--- | :--- |
| **Minecraft Versions** | `1.18 – 26.3` (`api-version: '1.18'`) |
| **Java Version** | `Java 17+` |
| **Server Platforms** | `Spigot`, `Bukkit`, `Paper`, `Purpur`, `Folia` (`folia-supported: true`), `Leaf` |
| **Required Dependency** | **MythicMobs** *(plugin also logs a fallback warning and supports `vanilla:<ENTITY_TYPE>` mobs if MythicMobs is absent)* |
| **Optional Dependencies** | **PlaceholderAPI**, **FancyHolograms**, **DecentHolograms** |
| **Built-In Holograms** | Native `TEXT_DISPLAY` (`1.19.4+`) with `ArmorStand` fallback (`1.18 – 1.19.3`) |

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="quick-installation-guide"></a>
## 🚀 Quick Installation Guide

Follow these 6 quick steps to get your first boss running in minutes:

1. **Check Requirements:** Make sure your server runs **Java 17+** on **Minecraft 1.18 – 26.3** (Spigot, Paper, Purpur, Folia, or Leaf).
2. **Install Dependencies:** Place **MythicMobs** (required for MythicMobs bosses) and any optional plugins (**PlaceholderAPI**, **FancyHolograms**, **DecentHolograms**) into your `plugins/` folder.
3. **Install RoyTopMobs:** Drop `RoyTopMobs-2026.5.0.jar` into `plugins/` and **restart** your server to generate the default files.
4. **Pick Your Language:** Open `plugins/RoyTopMobs/config.yml` and set `message: en` (or any of the 14 supported language codes).
5. **Create Your First Boss:** Copy `plugins/RoyTopMobs/tracked-mob/_example.yml` to a new file (for example `tracked-mob/fire_dragon.yml`), set `mob-type` to `"mythicmobs:<id>"` or `"vanilla:<ENTITY_TYPE>"`, and customize your rewards.
6. **Set Spawn & Test:** Run `/rtm reload`, look at the block where the boss should appear, run `/rtm setspawn fire_dragon`, and test it with `/rtm spawn fire_dragon`!

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="configuration-overview"></a>
## ⚙️ Configuration Overview

RoyTopMobs keeps configuration clean and modular inside `plugins/RoyTopMobs/`. Click any section below to see what each file does:

<details>
<summary><b>⚙️ 1. Main Settings — <code>config.yml</code></b></summary>

Controls global plugin behavior:
- **`message`**: Active language code (`en`, `vi`, `es`, `pt_br`, `fr`, `de`, `ru`, `zh_cn`, `ja`, `ko`, `id`, `tr`, `pl`, `it`).
- **`command-aliases`**: List of custom command aliases (default `["rtm", "roytopmobs"]`), applied immediately on `/rtm reload`.
- **`metrics`**: bStats settings (`enabled`, `server-uuid`, `log-startup`, `error-reporting`).
- **`database`**: Choose `SQLITE` (`database.db`) or `MYSQL` (`host`, `port`, `database`, `username`, `password`, `use-ssl`, `pool.*`), plus `cache-refresh-seconds` (default `30`). Stores table `roytopmobs_player_stats`.
- **`placeholder`**: Global formatting for PlaceholderAPI (`damage-format`, `percentage-format`, `no-data`, `no-player`, `ready`, `spawned-yes`, `spawned-no`).

</details>

<details>
<summary><b>🐉 2. Per-Boss Configs — <code>tracked-mob/&lt;mob_id&gt;.yml</code> (Template: <code>_example.yml</code>)</b></summary>

Each `.yml` file in `tracked-mob/` defines one tracked boss. Here is a minimal overview of the main sections:

```yaml
# Target MythicMobs ID or Vanilla entity (e.g., "vanilla:WITHER")
mob-type: "mythicmobs:SkeletonKing"
name: "<gradient:#FF4500:#FFA500>Skeleton King</gradient>"

spawn:
  max-mob-spawn: 1
  mob-per-spawn: 1
  lease-range: 40          # Teleports boss back if it leaves this radius
  condition:
    max-player: 1          # Minimum online players required to spawn
    kill-in-radius: []     # Optional mob kill requirements before spawning

respawn:
  time:
    enabled: true
    time: 3600             # Respawn cooldown in seconds
  schedule:
    enabled: false
    time-zone: "UTC"
    schedule:
      - "SUNDAY;20:00:00" # Fixed weekly schedule (DAY;HH:mm:ss)

reward:
  enabled: true
  damage-settings:
    min-damage: 100
    max-damage: -1
  top-damage:
    "1":
      - "console: give %player% diamond 5"
    "*":                   # Participation reward for all qualifying players
      - "console: give %player% emerald 1"
  killer:                  # Final-hit (last blow) reward
    - "console: give %player% gold_ingot 3"

hologram:
  enabled: true
  type: "TEXT_DISPLAY"     # TEXT_DISPLAY, FANCY_HOLOGRAM, or DECENT_HOLOGRAMS
  y-offset: 2.5
```

> Each boss file also supports `spawn`/`death` `broadcast`, `title`, `sound`, `effect`, `discord-webhook`, and `spawn-data.location` (`world`, `x`, `y`, `z`, `yaw`, `pitch`).

</details>

<details>
<summary><b>🌐 3. Language Files — <code>message/message_&lt;lang&gt;.yml</code></b></summary>

RoyTopMobs includes **14 built-in languages**:
`en`, `vi`, `es`, `pt_br`, `fr`, `de`, `ru`, `zh_cn`, `ja`, `ko`, `id`, `tr`, `pl`, and `it`.

All messages support standard `&` color codes, Hex colors (`#RRGGBB`, `&#RRGGBB`), and `<gradient:#c1:#c2>...</gradient>` tags.

</details>

<details>
<summary><b>⏳ 4. Persistent Respawn State — <code>respawn-data.yml</code></b></summary>

Automatically managed by RoyTopMobs to store active boss states and remaining respawn cooldown timestamps. This ensures bosses never reset or respawn early when your server restarts or when you run `/rtm reload`.

</details>

<details>
<summary><b>💾 5. Automatic Backups — <code>backups/&lt;timestamp&gt;/</code></b></summary>

Before running lossless configuration migrations (automatically on update or manually via `/rtm migrate`), RoyTopMobs saves a full timestamped copy of your existing configuration files into `backups/<timestamp>/` so nothing is ever lost.

</details>

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="commands-and-permissions"></a>
## 🛡️ Commands & Permissions

- **Main Command:** `/roytopmob`
- **Default Aliases:** `/rtm`, `/roytopmobs` *(add unlimited custom aliases in `config.yml` under `command-aliases` and apply them live with `/rtm reload`)*

| Command | Description | Permission | Default |
| :--- | :--- | :--- | :--- |
| `/rtm reload` | Reloads `config.yml`, language files, tracked mobs, database pool/cache, and dynamic command aliases. | `roytopmob.reload` | OP |
| `/rtm toggle` | Toggles global server-wide boss damage tracking on/off. | `roytopmob.toggle` | OP |
| `/rtm spawn <mob_id>` | Summons a tracked boss at its saved spawn point (or targeted block if run by a player without a saved point). | `roytopmob.spawn` | OP |
| `/rtm despawn <mob_id>` | Silently removes all active instances of the boss without death broadcasts or rewards, and restarts time respawn cooldown if enabled. | `roytopmob.despawn` | OP |
| `/rtm resetcooldown <mob_id>` | Despawns active instances and resets the boss respawn countdown back to its configured initial duration (requires `respawn.time.enabled: true`). | `roytopmob.resetcooldown` *(or `roytopmob.admin`)* | OP |
| `/rtm setspawn <mob_id>` | Saves the block in the player's line of sight (up to 50 blocks, fallback to player location) as the boss spawn point. | `roytopmob.setspawn` | OP |
| `/rtm deletespawn <mob_id>` | Deletes the saved spawn point and removes any active hologram timer for that boss. | `roytopmob.deletespawn` | OP |
| `/rtm migrate [all\|config\|mobs\|messages]` | Runs lossless configuration migration with automatic timestamped backups in `backups/<timestamp>/`. | `roytopmob.migrate` | OP |
| `/rtm help` | Displays the command help menu. | *None* | Everyone |

> **Update Notifications:** Players with `roytopmob.update` (Default: **OP**) receive in-game update notifications when joining the server.

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="placeholderapi-placeholders"></a>
## 📊 PlaceholderAPI Placeholders

- **Identifier:** `roytopmobs` (Prefix: `%roytopmobs_...%`)
- **Flexible Syntax:** You can also place `<mob>` before the suffix (for example, `%roytopmobs_<mob>_cooldown%`).

### Mob Info & Status

| Placeholder | Description |
| :--- | :--- |
| `%roytopmobs_name_<mob>%` | Colorized boss display name (Hex & Gradients supported) |
| `%roytopmobs_name_clean_<mob>%` | Plain boss display name stripped of color codes |
| `%roytopmobs_name_raw_<mob>%` | Raw unparsed name string from YAML |
| `%roytopmobs_spawned_<mob>%` | Alive status (`Yes` / `No`, configurable in `config.yml`) |
| `%roytopmobs_cooldown_<mob>%` | Formatted remaining respawn time (`HH:mm:ss` or `mm:ss`), `Ready` when alive on field, or `00:00` |
| `%roytopmobs_cooldown_formatted_<mob>%` | Alias for `%roytopmobs_cooldown_<mob>%` |
| `%roytopmobs_respawn_<mob>%` | Full colorized respawn countdown message from language file (`Ready` when alive) |
| `%roytopmobs_needed_<mob>%` | Remaining kill condition progress (e.g. `ZOMBIE: 3/10, SKELETON: 5/10`, or `N/A`) |

### Top Damage Rankings (`<N>` = `1`, `2`, `3`, ...)

| Placeholder | Description |
| :--- | :--- |
| `%roytopmobs_top_<mob>_<N>%` | Full formatted leaderboard line |
| `%roytopmobs_top_<mob>_<N>_name%` | Player username at rank `N` |
| `%roytopmobs_top_<mob>_<N>_damage%` | Unformatted rounded damage at rank `N` |
| `%roytopmobs_top_<mob>_<N>_damage_raw%` | Raw unformatted damage value at rank `N` |
| `%roytopmobs_top_<mob>_<N>_damage_formatted%` | Formatted damage (e.g. `15,250.5`) at rank `N` |
| `%roytopmobs_top_<mob>_<N>_percent%` | Formatted percentage with `%` (e.g. `32.5%`) |
| `%roytopmobs_top_<mob>_<N>_percent_raw%` | Raw percentage number without `%` |

### Player Lifetime Database Stats (SQLite / MySQL)

| Placeholder | Description |
| :--- | :--- |
| `%roytopmobs_player_kills_<mob>%` | Viewing player's total kills for `<mob>` |
| `%roytopmobs_player_damage_<mob>%` | Formatted lifetime damage dealt to `<mob>` |
| `%roytopmobs_player_damage_raw_<mob>%` | Raw lifetime damage dealt to `<mob>` |
| `%roytopmobs_player_highest_damage_<mob>%` | Formatted highest single-fight damage to `<mob>` *(also supports `_raw` variant)* |
| `%roytopmobs_player_last_damage_<mob>%` | Formatted most recent fight damage to `<mob>` *(also supports `_raw` variant)* |

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="discord-webhook-guide"></a>
## 🤖 Discord Webhook Guide

RoyTopMobs v2026.5.0 includes a completely overhauled Discord Webhook engine built into every `tracked-mob/<mob_id>.yml` file:

- **Per-Event Overrides:** Set global webhook defaults or customize `spawn`, `death`, and `reward` events individually with their own `username`, `avatar-url`, and `content`.
- **Single or Multi-Embeds:** Send up to **10 embeds** per webhook message, each supporting `title`, clickable `url`, `description`, `color`, `timestamp` toggle, `author`, `thumbnail`, `image`, `footer`, and up to **25 `fields`**.
- **Threads & Forum Channels:** Post directly into an existing thread using `thread-id`, or automatically create a new Forum post using `thread-name`.
- **Silent Pings & Mention Control:** Enable `suppress-notifications` for quiet alerts and configure `allowed-mentions` to control role/user/everyone pings safely.
- **Dynamic Reward Leaderboards:** Use `top-limit` and `format-line` inside the `reward` event to display clean Top #1–#N damage rankings right inside Discord.
- **Built-In Reliability:** Automatic webhook URL validation (with token masking in console logs), Discord character-limit truncation, and automatic HTTP 429 rate-limit retries.

```yaml
discord-webhook:
  enabled: true
  webhook: "https://discord.com/api/webhooks/..."
  username: "RoyTopMobs Boss Alert"
  suppress-notifications: false
  reward:
    top-limit: 5
    # See tracked-mob/_example.yml for available format-line & embed placeholders
    format-line: "..."
    embeds:
      - title: "Boss Defeated!"
        color: "#FFA500"
        timestamp: true
```

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="live-bstats-server-statistics"></a>
## 📈 Live bStats Server Statistics

RoyTopMobs uses **bStats** to collect anonymous server and version usage metrics (configurable under `metrics:` in `config.yml`).

[![bStats](https://bstats.org/signatures/bukkit/RoyTopMobs.svg)](https://bstats.org/plugin/bukkit/RoyTopMobs/26207)

[⬆ Back to Table of Contents](#table-of-contents)

---

<a id="support-and-community"></a>
## 💬 Support & Community

Need help setting up a boss, configuring a webhook, or migrating your configs? Join our community!

- **Discord Support Server:** [https://discord.gg/zNpkXGFQRS](https://discord.gg/zNpkXGFQRS)
- **Author:** `athanh` • **Website:** `roymcvn.com`

⭐ **Enjoying RoyTopMobs?**  
If RoyTopMobs has made your server's boss fights more exciting, please consider leaving a **5-star review on SpigotMC**! Your feedback helps us keep improving and adding new features.

[⬆ Back to Table of Contents](#table-of-contents)
