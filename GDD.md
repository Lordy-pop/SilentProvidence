# Project Mobius — Game Design Document
*Set in the Silent Providence universe*
*Version 0.2 — May 14 2026*

> **About this document.** This is the design-intent doc — what the game *is* and what players *experience*. Implementation lives in the [system specs](Systems/). Decisions made after the initial draft are tracked in [`GDD_ADDENDUM.md`](GDD_ADDENDUM.md). When the GDD and the addendum agree, both are authoritative. When they disagree, the addendum wins (it's the dated decision log).

---

## 1. Overview

**Title:** Project Mobius
**Universe:** Silent Providence
**Genre:** Co-op Horror Roguelike with Proximity Voice Chat
**Engine:** Unity 6
**Player Count:** 1–4 (party is the baseline; solo runs are easier — see §12.4)
**Platform:** PC (v1)
**Run Length:** 1–2 hours per session

**Tagline:** *The first warp drive ever built. Most of the crew who stole it are dead. Keep running.*

---

## 2. Concept

Project Mobius is a co-op roguelike set in the White Throne era of Silent Providence. Players are Tato survivors of a failed heist — inside human robot bodies, carrying a stolen warp drive, hunted by the White Throne Legion across the galaxy.

The game has no win condition by design. The goal is distance. Go as far as possible. Set a record. When the Legion catches you, it's over — not failure, just how far you got.

**Tone:** Pressure and stress over horror. Enemies are not designed to be frightening in appearance. They are frightening in behavior and consequence. Atmosphere and sound carry the dread. The visual language is graphic and stylized — legible, color-coded, cool to look at. The Legion is scary the way a machine is scary: not because it wants to hurt you, but because it doesn't care either way.

---

## 3. Story & Setting

**Era:** The White Throne War. The millennium of galactic peace is over. The White Throne — a human-built AI — has declared judgment on alien civilizations and launched a crusade across the stars.

**The Murmur** is the resistance. Humans and alien allies who believe the original mission of The Holding was compassion, not control. They operate in the margins, moving like a heartbeat beneath the White Throne's notice.

**The Heist:** The Murmur identified the Mobius Drive — the first fully independent warp drive, requiring no fixed points in space — as the key to outrunning the Legion. They launched a heist to steal it. It went wrong. Most of the Murmur operatives were killed. A handful of Tato survivors escaped with the drive and a stolen ship.

**The Players:** Tatos — the small, dense creatures native to Proxima Centauri b, allies of humanity since first contact. During the Golden Era, humans together with Tatos developed human robot bodies for competition and exploration. These same robots are now survival gear. To the White Throne Legion, the players look human. They are not. This has no gameplay implication — it is story texture only.

**The Goal:** Use the Mobius Drive to jump across the galaxy, raiding stations for fuel, outrunning the Legion for as long as possible. There is no safe harbor. There is only distance.

---

## 4. Core Gameplay Loop

```
[Star Map] → Choose jump destination (range limited by fuel)
     ↓
[Station Raid] → Collect fuel, equipment, complete missions
     ↓
[Extraction] → Call ship at the station radio, escape with loot
     ↓
[Ship Hub] → Deposit fuel, manage upgrades, plan next jump
     ↓
[Star Map] → Legion timer ticks down — jump again or die
```

**The Timer:** Represents the White Throne Legion fleet closing in. Displayed on the star map as a closing zone.

**Timer phases:**
- *Timer running:* Normal play. Jump, raid, extract, repeat.
- *Timer hits 0:* Ember Moth soldiers deploy into the current station. Players must extract.
- *After extraction at 0:* The outcome depends on fuel.
  - **Enough fuel to jump outside the Legion zone → survive.** The ship escapes the zone, run continues.
  - **Not enough fuel to escape the zone → game over.** Ship is annihilated.

**Design implication:** Fuel is a dual-purpose resource. It determines jump range during the run *and* determines whether the ship can escape when the Ember Moths arrive. Every canister matters twice.

**Ship is the safe space.** The Legion timer **pauses entirely** while the ship is the active scene. Players can plan, save, deposit fuel, and apply upgrades without time pressure. The timer resumes the moment a station raid begins.

**Save/Load:** Available from the save terminal in the ship hub. Session can be resumed later without penalty.

**Run End:** When the timer hits 0 and the Legion zone reaches the players' position, or when the players fail to escape with fuel after the Ember Moths arrive. **The Legion destroys the ship. Nothing carries between runs except cosmetic meta-progression (see §12).** Distance traveled and time survived are recorded as the run score and posted to one of two leaderboards (solo or party).

---

## 5. The Star Map

An FTL-style navigational map displayed on a shared panel inside the ship. All players can see and discuss it together.

- **Star Systems:** Populated with space stations, planets, and points of interest
- **Jump Range:** Determined by current fuel supply — more fuel = farther jumps
- **Legion Zone:** Visually closes in on the map as the timer counts down
- **Decision Layer:** Players choose between nearby low-risk targets vs. distant high-reward ones. Fuel management determines freedom of movement.
- **Wormholes:** Pre-existing man-made wormholes exist between certain solar systems — fixed shortcuts that don't require fuel but may be patrolled

While the ship is the active scene, the Legion timer pauses. While in a station raid, it resumes.

---

## 6. The Ship Hub

A physical interior space the players walk around in between raids.

**Key Areas:**

| Area | Function |
|---|---|
| Navigation Panel | Star map access — shared screen, everyone sees it |
| Upgrade Panels | Apply collected Upgrade Parts to grant player upgrades |
| Equipment Storage | **No UI.** Items physically dropped on the ship floor between raids (Lethal-style). The ship scene is never unloaded, so dropped items persist across raids. |
| Fuel Deposit | Load collected canisters into the warp drive |
| Save Terminal | Save and load the run |

The ship is the only safe space. Atmosphere here should contrast with the stations — quieter, more intimate. The hum of the drive. The city of stars outside the window. **The Legion timer is paused while you are aboard.**

The ship starts each session with **one iPad** (see §8.4) sitting on a deck shelf, ready for the team to take into the first station.

---

## 7. Station Raids

### 7.1 Station Types
- **Warehouses** (core type — all stations have this flavor)
- **Science Labs**
- **Farms**
- *(More facility types planned)*

Many stations have multiple floors. Connectors (stairs, elevators) are placed during generation.

### 7.2 Procedural Generation
Stations are procedurally generated from modular rooms, giant warehouses, and connecting hallways. Each run generates a new layout. See [`Systems/ProcGen.md`](Systems/ProcGen.md) for the technical approach.

**Key spawns (min/max limits per station):**
- Charge stations (fuel sources)
- Equipment caches
- Upgrade Parts
- Keycards (unlock restricted areas with extra loot)
- Medical/Cloning bays (player revival points)
- Mission objectives
- The Security Camera Room (see §8.4)
- Extraction radio + landing zone

**Multi-floor:** Vertical layouts use stacked floors connected by stairs / elevators. Floor index is tracked everywhere it matters (the iPad map, AI patrol baking, escape destinations).

### 7.3 Fuel — Battery Canisters
Fuel is collected physically. Charge stations inside the station charge battery canisters (jerrycan form factor, battery internals). Players carry canisters out and deposit them in the ship's fuel system.

Canisters are subject to the inventory system — they take up hand/suitcase space like any other item.

### 7.4 Extraction

Extraction has three flavors:

**Default — the station radio.** Stations are designed with a subway-station flavor for boarding the ship: a designated landing zone with a nearby radio interactable. A player must reach the radio, call the ship, and the team boards at the landing zone.

**Power-outage variant.** Some stations spawn unpowered. The radio is inert until power is restored at an in-station electrical room (an interaction objective that turns into a required side-trip).

**Costly escapes.** Some stations have alternative one-way exits, like a trash chute. These are:
- **Single-use** — first player closes it for everyone else.
- **No suitcase** — anything in a suitcase has to be dropped before taking the chute.
- **One player only** — the chute closes behind the user.
- **Not guaranteed extraction** — the chute may dump the player at another part of the station rather than the landing zone. It's a gamble that bypasses some of the radio path, not a free exit.

**If no player escapes** a raid, the team respawns on the ship — but loses time (the Legion timer accelerates by a tunable penalty). There is no permanent death between stations, but death during the run-end push (timer = 0) is final until the next run.

---

## 8. Player Systems

### 8.1 Inventory — Two-Hand System + Worn Slots

Players have two hands. Each hand carries one item. They also have a small **Worn Slot** pool for items that are worn rather than held (gas mask, sponge shoes).

| Configuration | Hands Used | Capacity | Tradeoff |
|---|---|---|---|
| Default | 2 | 2 items | Full mobility |
| One suitcase | 1 hand | 2 + 3 = 5 items | One hand free |
| Two suitcases | 2 hands | 2 + 6 = 8 items | Cannot use items |
| Worn slots | 0 hands | 1 → 3 worn items | Worn items are passive effects (no carry cost) |

**Worn slot expansion:** players start with **1 worn slot**. In-run player upgrades can expand to 2, then 3. Worn slots reset between runs along with everything else. See [`Systems/Equipment.md`](Systems/Equipment.md).

### 8.2 Weight System

Heavier loads reduce movement speed. Players must balance loot value against mobility. A fully loaded two-suitcase player is a liability in a chase. Worn items have no weight cost in v1.

### 8.3 Health, Downed State, and Spectator Mode
- HP depletes from damage.
- At 0 HP: player falls to the ground (**downed**, not dead).
- Downed timer: teammates can revive a downed player within a window.
- If the timer expires with no rescue: player **dies**.
- **Revival:** Medical/cloning bays inside stations can revive dead players — finding one becomes a team priority when players are down.

**Spectator mode (dead players):**
- Cycle through alive teammates in **third-person camera**.
- **Voice is fully muted** — dead players cannot speak on proximity or walkie. AI never hears them.
- **iPad / Map Device access is disabled** — dead players cannot see the station map.
- Inventory drops as physics pickups at the death location for teammates to recover.

The run continues as long as **one player is alive.** When teammates die, the atmosphere escalates (see §9 — the Director).

### 8.4 The iPad — Map Device

The iPad is a handheld device that displays a 2D map of the station. The ship starts each session with one; additional iPads can be found as rare loot inside stations.

**How the map fills in:**
- The map data is **team-shared** at the station level. Any teammate's movement reveals rooms — not just the iPad-holder's.
- A player needs to *hold* an iPad to view the map, but the underlying data is collected at team level. If you find an iPad halfway through a station, it reveals all rooms the team has been through.
- **Security Camera Room.** Every station spawns a security console with a map screen. Interacting with the screen downloads the *full* station layout to all iPads, including unvisited rooms. The console screen itself shows **live enemy positions** (only while interacting) and has a button to flip between floors. Future feature: cycling through individual CCTV camera feeds for specific rooms.
- **Map data does not carry between stations.** Each station is fresh procgen.
- Dead players cannot view the iPad (see §8.3).

See [`Systems/MapDevice.md`](Systems/MapDevice.md).

---

## 9. Threats

**Design philosophy:** Not horror, pressure. Enemies are not designed to be scary in appearance. Their behavior creates the dread. Sound design amplifies this.

### 9.1 Alien Workers
Station inhabitants — enslaved by or working under human authority. Behavior varies:
- **Passive:** Believe the players are human, ignore them
- **Hostile:** Know the players don't belong, attack trespassers

*No gameplay distinction based on the players' Tato identity — hostility is based on trespass, not species.*

### 9.2 Service Robots
Maintenance and security automatons. Guard restricted areas, patrol routes. More predictable than workers — follow logic, not judgment.

### 9.3 Legion Scouts
**Priority threat.** White Throne advance units that may appear in stations. If a scout detects the players and escapes, it broadcasts the location to the Legion fleet — the timer accelerates.

**Design implication:** Scouts must be neutralized before they can escape. Introduces stealth/priority kill decisions. A scout spotted across a warehouse is an emergency.

### 9.4 Ember Moth Soldiers *(The Annihilation Army)*
**The Ember Moths** are the White Throne's elite annihilation force — distinct from standard Legion units.

**Soldiers** deploy when the Legion timer hits zero. They enter stations and sprint directly to player locations, hunting one by one. Armed. Fast. They know where you are.

This is the final escalation — the Legion has arrived, the ship is cornered, and now it's a race inside the station itself.

### 9.5 The Super Soldier
A single massive threat. Activates after the players escape Ember Moth soldiers **3 times**. The super soldier does not patrol — it hunts. One target at a time. Persistent.

**Design implication:** Escaping soldiers is not a safe strategy — it's a counter. The players are on a clock within the clock. Escape three times and something worse wakes up.

### 9.6 The Hollow *(last-player-alive only)*
A special threat that spawns **only when one player is alive on the team.** Working name: The Hollow.

**Behavior:** Persistent hunter, biased toward extraction routes. Where Ember Moths and the Super Soldier are pure-pressure killers, the Hollow is *pressure with a direction* — its job is to push the lone survivor out of the station, not necessarily to kill them. Designed to make solo survival feel desperate and motivated.

**Despawn:** When the team is no longer last-man-alive (a teammate is revived from a cloning bay), the Hollow leaves after a short delay.

### 9.7 The Director *(atmospheric layer)*
Behind the named threats, a **Director system** drives macro tension and conditional spawns. The Director:
- Raises the audio mix and atmospheric pressure when teammates die or go down.
- Flags rooms a player has lingered in too long; AI threats are biased toward converging on stale rooms.
- Triggers the Hollow spawn on last-player-alive.
- Drives `DifficultyDef` scaling by player count (see §12.4).

The Director itself is invisible — players experience it as escalating sound and "the monsters are getting closer." See [`Systems/Director.md`](Systems/Director.md).

---

## 10. Equipment

Tools collected during raids. Most are subject to the inventory system — carry cost matters. A few are worn (no hand cost, no weight cost in v1).

| Item | Form | Effect |
|---|---|---|
| **Radar scanner** | Hand-held | When activated, opens a screen-obscuring device overlay showing nearby enemies on a minimap. Range + refresh rate are tuning numbers. |
| **Gas mask** | Worn (head) | Protection toggle. Future gas hazards won't hurt the wearer. (v1: no gas content yet — the mask is groundwork for post-v1.) |
| **Sponge shoes** | Worn (feet) | Reduces footstep audio signature. AI hearing sensors detect quieter footsteps. (Movement audio is a separate channel from voice — see §14.) |
| **Walkie talkies** | Hand-held | Routes voice through a private channel. Walkie audio is **not** heard by AI, but you give up the proximity channel while equipped (only walkie-equipped teammates hear you). |
| **Keycards** | Hand-held | Unlocks restricted zones with extra loot. Color/tier varies. |
| **iPad / Map Device** | Hand-held | See §8.4. |

Equipment list will expand. Each item should have a clear use case and a meaningful tradeoff.

See [`Systems/Equipment.md`](Systems/Equipment.md).

---

## 11. Missions

Procedurally assigned per station. Displayed on the iPad / nav panel before entry.

**Mission types:**
- **Retrieve crate** from a specific location.
- **Deliver item** — bring a generic item type to a dropoff anchor.
- **Turn-in** — hand specific items (often multiple) to an in-station console. Plays into the warehouse flavor: "ship these crates to bay 4."
- **Activate terminal**.
- **Activate door** (often gated by a keycard or by power restoration).
- **Eliminate target** — kill a specific threat.
- *(Future)* NPC delivery / escort, when NPCs land.

**Mission tiers:**
- **Optional:** Bonus loot for completion.
- **Required:** Must be completed before extraction becomes available.

**Mission rewards:** Each mission awards **Credits** (the team currency — see §12) and may also drop **items** at a reward anchor near the dropoff.

Missions layer on top of the fuel objective — a station may be fuel-light but mission-rich, forcing a tradeoff decision on the star map.

See [`Systems/Missions.md`](Systems/Missions.md).

---

## 12. Progression

### 12.1 In-Run Progression — Three Resources

Three resources, collected during raids, lost when the run ends:

| Resource | Form | Source | Used for |
|---|---|---|---|
| **Fuel** | Battery canisters (physical items) | Charge stations inside raided stations | Jumps on the Star Map; escaping the Legion zone at timer = 0 |
| **Upgrade Parts** | Physical items | Station loot drops + mission rewards | Applied at the ship's Upgrade Panel to grant **Player Upgrades** (stat improvements: speed, carry weight, HP, worn-slot expansion, etc.) |
| **Credits** | Team-shared wallet (abstract) | Mission completions | *(Post-v1)* vending machines in stations + shops on the star map. v1: Credits accumulate and are recorded in the final run record, but there is no spend mechanic yet. |

**Player Upgrades** include the stat improvements above plus structural upgrades like adding a 2nd or 3rd worn slot.

### 12.2 Meta Progression
Persists between runs — **cosmetic only** for v1:
- Skins
- Visual customization
- Animations

There is no persistent currency or stat-power-creep across runs. The roguelike pressure is by design.

### 12.3 Run End — Everything Resets
When the Legion catches you, **the ship is destroyed.** Inventory, Upgrade Parts, Credits, ship-floor drops, applied upgrades — all lost. Only cosmetic meta-progression survives. Reinforces the "no win condition, distance is the score" thesis.

### 12.4 Difficulty Scaling and Leaderboards

**Solo runs are easier than party runs.** Three knobs scale by player count (1, 2, 3, 4):
- Threat spawn count (fewer in solo)
- Legion timer base duration (more time in solo)
- Loot density (more loot in solo)

Threat HP and damage are **not** scaled — encounter feel stays consistent.

**Two leaderboards:**
- **Solo Leaderboard** (PlayerCount == 1)
- **Party Leaderboard** (PlayerCount ≥ 2)

Sorted by Distance traveled, tiebreak by Time Survived. Local only for v1; online leaderboards considered for post-v1.

See [`Systems/Persistence.md`](Systems/Persistence.md) for the run record schema.

---

## 13. Art Direction

**Style:** Graphic and stylized. Low poly models with high-quality textures. Style is the priority — readable, cool, intentional.

**Color language:** Color-coded factions and threats. The White Throne Legion is **neon red** — they should read as cool and dangerous. Every major entity and zone should have a visual signature.

**Enemy design:** Not visually scary. Behavior and context provide the dread. A robot guard in the distance doing its patrol is not frightening. A robot guard that has noticed you and is moving differently is.

**Environment:** Stations should feel lived-in and functional — warehouses, labs, farms. Alien scale and layout. Not haunted houses. Industrial spaces that happen to contain things that will kill you.

---

## 14. Audio

Sound is a primary atmospheric pillar. The visual style is legible and stylized — sound carries the horror weight.

**Design targets:**
- **Director-driven mix.** As the Director's tension level rises (teammates die, last-man-alive triggers), the audio mixer escalates to dissonant tracks. Players hear pressure climb without being told.
- Proximity voice chat integrated as gameplay (walkie talkies, distance attenuation).
- Enemy audio cues that communicate behavior and threat level.
- Ambient station audio that distinguishes facility types (warehouse vs lab vs farm).
- The Legion's approach on the star map should be felt, not just seen.
- Ship interior audio: contrast against the stations — mechanical comfort. The hum of the drive. Distant stars.

**Two audio channels for AI perception** (server-authoritative — see [`Systems/Voice.md`](Systems/Voice.md), [`Systems/AI.md`](Systems/AI.md)):
- **Voice (proximity)** — Dissonance VAD amplitude is reported to the server, which alone decides whether a player is "audible" to AI. Walkie audio bypasses this and is never heard by AI.
- **Movement** — footsteps and motion produce a separate audio signature. Sponge shoes reduce this signature.

**Composer:** Ko *(transitioning from engineering role — knows the universe, scoring the game)*.
*Full audio design document: TBD — develop with Ko.*

---

## 15. Scope

### v1 — Current Focus
- Infinite run with distance/time records
- Core loop: star map → raid → extract → repeat
- Up to 4 players co-op
- Solo and Party leaderboards (local)
- Proximity voice chat (Dissonance) — heard by AI via server-authoritative bridge
- Walkie talkies — AI-inaudible private channel
- Friendly fire ON
- Basic enemy roster (Workers, Service Robots, Legion Scouts, Ember Moth Soldiers, Super Soldier, The Hollow)
- Director system (tension layer, stale-room flagging, Hollow spawn)
- Warehouse stations + 1–2 additional types (Lab, Farm)
- Multi-floor stations
- iPad / Map Device + Security Camera Room (static layout download + live enemy view + multi-floor toggle)
- Extraction via radio + costly escape variants + power-outage variant
- Three in-run resources (Fuel, Upgrade Parts, Credits) — Credits tracked but no spend mechanic v1
- Cosmetic meta progression (skins, animations)
- PC only

### Future / Out of Scope for v1
- Multiple story endings
- Full enemy roster expansion
- Additional station types
- Currency spend mechanics (vending machines, star-map shops)
- NPC interactions and NPC-driven missions
- Gas hazards (gas mask is groundwork)
- Cycling through individual CCTV camera feeds at the Security Camera Room
- Host migration (PEAK-style "host leaves, another player takes over")
- Online leaderboards
- Console ports

---

## 16. Resolved Decisions and Open Tuning

The original §16 open questions (platform, friendly fire, meta currency, etc.) are now **resolved** and documented in [`GDD_ADDENDUM.md`](GDD_ADDENDUM.md) §1 (Locked Decisions).

What remains is **tuning** — numeric values that should be locked when we can feel them in-game (timer durations, weight curves, threat density numbers, VAD thresholds, etc.). These live in [`GDD_ADDENDUM.md`](GDD_ADDENDUM.md) §2 as `P1`–`P30` and are deliberately deferred until playable.

---

*GDD drafted by Sparkle. Built from design session with Lordy, May 5 2026.*
*Revised 2026-05-14 to reflect Phases 1–3.6 design decisions (worn slots, iPad / Map Device, Director, The Hollow, three-resource taxonomy, dual leaderboards, multi-floor stations, extraction modes, ship-as-safe-space, run-end strictness).*
*Silent Providence lore reference: D:\Sparkle\lore\silent-providence.md*
