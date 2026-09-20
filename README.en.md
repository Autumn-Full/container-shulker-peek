# Container Content Preview — crosshair preview & shulker box preview

A container content preview tool for Minecraft Bedrock: **see what is inside a container without
opening it**. Two preview modes cover different situations:

| Feature | How it is triggered | Where it is drawn | Addon edition | Server plugin edition |
|---|---|---|---|---|
| **Crosshair preview** | Aim the crosshair at a container block or container entity | Centre of the HUD, laid out by slot | ✔ | ✔ |
| **Shulker box preview** | **Click a shulker box item** inside an open container / inventory UI | **Left edge** of the screen | — | ✔ |
| Shulker box badge | Boxes matching the tier test get a small picture of their contents in the icon's bottom-right corner | Bottom-right of the icon | — | ✔ |
| Preview scaling (three steps) | `/peek:scale` | Crosshair preview only | — | ✔ |

> **This repository is a release-only repository** (behavior pack, resource pack, server plugin and
> documentation). It contains **no source code**.
>
> License: **[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)** —
> Attribution-NonCommercial-ShareAlike.
> · **You may** use it, modify it, and redistribute it free of charge (your own server, playing with
>   friends, and public **non-commercial** servers are all fine).
> · **You may not** make money with it (selling it, paid modpacks, monetised platforms, or using it as a
>   selling point for a paid server or service).
> · **You must** keep the attribution and copyright notices; a modified version has to stay under the same
>   license and must state that it is based on this project.
>
> The binding text is `LICENSE` (official CC legal code, English); `LICENSE.txt` is the Chinese
> explanation, including where the "no commercial use" line is drawn.

Versions: behavior pack **2.994.22** (1.21.x) / **2.994.21** (1.26.x) · resource pack **2.994.18** ·
server plugin **2.994.19** (built 2026-09-20)

---

## 1. Which file do I need?

| File | For | What's inside |
|---|---|---|
| `容器预览-0919-1.21.x.mcaddon` | **Single player / mobile / Realms**, game 1.21.x | Behavior pack + resource pack (one import installs both) |
| `容器预览-0919-1.26.x.mcaddon` | Game 1.26.x (script module 2.4.0, min_engine 1.21.100) | Same, different script module version |
| `容器预览_服务器版_交付.zip` | **Dedicated BDS server** (LeviLamina + LegacyScriptEngine) | LSE plugin + resource pack + deploy script + install manual + command reference |

Not sure? Phone, single player, Realms, ordinary multiplayer → grab a `.mcaddon` (try the 1.21.x one
first). Your own BDS server → the server zip. Install only **one** of the two `.mcaddon` files.

---

## 2. Installing the addon (`.mcaddon`)

1. Copy the `.mcaddon` to your device and **open it** with a file manager — Minecraft installs the
   behavior pack **and** the resource pack;
2. Create or edit a world: **enable both the resource pack and the behavior pack**
   (with either one missing nothing is drawn at all, and no error is shown);
3. **Turn on "Experimental Gameplay → Beta APIs"** — the behavior pack depends on
   `@minecraft/server 2.1.0-beta`, and without it the script never loads;
4. **Set "Video → GUI Scale" to 0** — in game 1.21.93 a known bug (MCPE-184059) makes custom glyph
   colours wrong whenever the GUI scale is not 0;
5. Enter the world and aim at a container. In-game commands need **operator (OP)** permission;
   the world owner has it by default in single player.

**Updating**: just import the newer `.mcaddon` over the old one. The version is shown at the end of the
pack name in the pack list — "nothing changed" is nearly always a stale pack that was not replaced.

---

## 3. What you get

**Supported containers**: chests, trapped chests, barrels, shulker boxes (all 16 colours), dispensers,
droppers, hoppers, crafters, chest minecarts, hopper minecarts.

**Not supported**: the player inventory, ender chests, lecterns, campfires (they have no container
component).

The preview is laid out **by slot**: an item is drawn in the cell matching its slot, empty slots stay
blank — 3 rows for a small chest, 6 for a double chest, 3 for a hopper. Optional extras: item counts,
the container's name, a slot-position grid, item names.

**Shulker box preview** (server plugin edition, section 5): a server cannot observe hovering (there is
neither a packet nor an event for it), so **clicking a shulker box item** inside a container / inventory
UI is the only per-slot signal it receives. The box's contents are then drawn with the same slot layout
along the **left edge** of the screen (the centre is covered by the container panel and its dimming
overlay, the edge is not). It is withdrawn automatically — and the crosshair preview takes over again —
when the player turns the view, closes the UI, or aims at a container once more; it also expires after
5 seconds. Its position is configured separately with `/peek:boxpos` and `/peek:boxhspace`.

---

## 4. Common commands (addon edition, OP required)

Type `/peek:usage` in game for the full list. The usual ones:

| Command | What it does |
|---|---|
| `/peek:display on\|off` | master switch for the preview |
| `/peek:top <n>` | vertical position (positive = down, **negative = up**) |
| `/peek:hspace <n>` | horizontal position (positive = right) |
| `/peek:gap` `/peek:lines` `/peek:perline` `/peek:width` | row gap / max rows / cells per row / row width budget |
| `/peek:counts on\|off` | show item counts |
| `/peek:header on\|off` | show the container's name |
| `/peek:grid on\|off` | slot-accurate grid (off = compact list) |
| `/peek:names on\|off` | show item names |
| `/peek:slotgrid on\|off` | show the slot-position grid |
| `/peek:reach` `/peek:poll` `/peek:refresh` `/peek:resend` | reach / poll / re-read / resend intervals |
| `/peek:status` `/peek:about` | current values and where they come from / name, author, anti-tamper fingerprint |
| `/peek:perf` `/peek:probe` | performance counters / dump the components readable from the held item |

Leaving the value out toggles a switch. Values are stored in scoreboards, so they survive a restart.
(`/peek:scale`, `/peek:box`, `/peek:boxbadge`, `/peek:badgelevel`, `/peek:log` belong to the **server**
edition and do not exist in the addon. The addon edition provides the **crosshair preview only** — the
shulker box preview, the shulker box badge and three-step scaling all require the server plugin.)

---

## 5. Server edition (BDS + LeviLamina + LegacyScriptEngine)

Unzip `容器预览_服务器版_交付.zip`. The authoritative steps (including the manual route and every trap
we hit) are in the bundled **`安装手册.md`**; the full command reference is **`指令速查.md`**. Summary:

1. `dist-lse/CrosshairContainerPeek/` → `<server>/plugins/CrosshairContainerPeek/`
2. `packs/crosshair_container_peek_icons/` → `<server>/resource_packs/CrosshairContainerPeekIcons/`
   (the folder name must match exactly)
3. Register the resource pack inside the world (the two JSON files
   `world_resource_packs.json` and `valid_known_packs.json`) — `tools/pack/sync-server-rp.mjs` in the
   bundle does this and verifies versions (needs Node.js 18+)
4. Restart the server, then have clients **rejoin once** (a changed resource pack version is what makes
   the client download it)
5. The server edition needs **no** behavior pack and **no** Beta APIs

Extra features over the addon edition:

| Feature | Notes |
|---|---|
| **Shulker box preview** | **Click a shulker box item** inside a container / inventory UI → its contents are drawn along the **left edge**; withdrawn automatically when the view turns, the UI closes, or the crosshair aims at a container again (5 s maximum). Position: `/peek:boxpos`, `/peek:boxhspace` |
| Shulker box badge | Boxes matching the tier test (four tiers, `/peek:badgelevel`) get a small picture of their contents in the bottom-right corner of the icon |
| Three-step scale | `/peek:scale full\|three4\|half` (crosshair preview only) |
| Position knobs | `/peek:top` `/peek:hspace` `/peek:boxpos` `/peek:boxhspace` |
| On-screen self-test | `/peek:hudtest` — draws three lines of plain text on the player's screen, the fastest way to tell "why are my icons boxes/question marks" (run it again to stop) |
| Server self-test | `/peek:check` — verifies the whole chain item by item (read-only, players may run it) |
| Log switch | `/peek:log off` — stop the routine console log (one line at boot, command replies and errors stay) |

---

## 6. Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Nothing shows up at all | Resource pack not enabled / behavior pack not enabled / Beta APIs not enabled / crosshair not on a container. On a server: make sure the client rejoined after the pack was (re)registered |
| Icons render as **question marks or boxes** | The client does not have (or has not enabled) the resource pack — our glyphs are private-use code points and cannot be drawn without the font pages. On a server, ask the player to run `/peek:hudtest`; an admin can also set `texturepack-required=true` in `server.properties` to force the download |
| Icons are too small / blurry | The client still has an older resource pack — re-import it / rejoin the server |
| Colours look wrong | GUI Scale is not 0 (MCPE-184059) |
| `/peek:` does not appear in the command list | No OP permission, or custom commands are unavailable in that game version (they need 1.19.70+ with Beta APIs enabled) |
| Position is awkward | `/peek:top`, `/peek:hspace` (server edition also has `/peek:boxpos`, `/peek:boxhspace`) |
| Works on a phone but not on a PC | That PC never downloaded the resource pack — not a platform difference (the glyph pages and the HUD override work the same way everywhere) |

---

## 7. Credits, assets and license

- **Author**: shykingsui. `/peek:about` in game prints the attribution and an anti-tamper fingerprint
  (`CCP/…`).
- **Icons** are rendered from the **vanilla Minecraft Bedrock block models and textures**. Those assets
  belong to Mojang / Microsoft and this project claims no rights over them; use them in line with the
  Minecraft EULA and Usage Guidelines. Note that this project's license is **stricter** than the official
  guidelines when it comes to commercial use.
- If you got the **Wiki comparison build**, it additionally contains isometric renders from the Chinese
  Minecraft Wiki, licensed **CC BY-NC-SA 3.0**, attributed in its manifest.
- One naming clarification: a license that forbids commercial use is **not** open source as defined by
  the OSI. The accurate description of this project is
  "**free to use and modify, non-commercial only**". If you need a commercial licence (for a monetised
  server or a paid product), contact the author to arrange one.
- **Reporting problems**: open an issue in this repository, or QQ **3251099642**.
  For "it does not show up", please include the game version, the pack version, the output of
  `/peek:about`, and — on a server — the output of `/peek:check`.
