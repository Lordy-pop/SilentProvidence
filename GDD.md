# Project Mobius — Game Design Document
*Set in the Silent Providence universe*
*Version 0.1 — Draft, May 5 2026*

---

## 1. Overview

**Title:** Project Mobius
**Universe:** Silent Providence
**Genre:** Co-op Horror Roguelike with Proximity Voice Chat
**Engine:** Unity
**Player Count:** 1–4 (difficulty tuned for 4)
**Platform:** TBD
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
[Extraction] → Call ship, escape with loot
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

**Save/Load:** Available from the ship hub between raids. Timer pauses when saved. Session can be resumed later without penalty.

**Run End:** When the timer hits 0 and the Legion zone reaches the players' position. Distance traveled and time survived are recorded as the run score.

---

## 5. The Star Map

An FTL-style navigational map displayed on a shared panel inside the ship. All players can see and discuss it together.

- **Star Systems:** Populated with space stations, planets, and points of interest
- **Jump Range:** Determined by current fuel supply — more fuel = farther jumps
- **Legion Zone:** Visually closes in on the map as the timer counts down
- **Decision Layer:** Players choose between nearby low-risk targets vs. distant high-reward ones. Fuel management determines freedom of movement.
- **Wormholes:** Pre-existing man-made wormholes exist between certain solar systems — fixed shortcuts that don't require fuel but may be patrolled

---

## 6. The Ship Hub

A physical interior space the players walk around in between raids.

**Key Areas:**

| Area | Function |
|---|---|
| Navigation Panel | Star map access — shared screen, everyone sees it |
| Upgrade Panels | Apply player upgrades collected during raids |
| Equipment Shelves | Store and organize tools between runs |
| Fuel Deposit | Load collected canisters into the warp drive |
| Save Terminal | Save and load the run |

The ship is the only safe space. Atmosphere here should contrast with the stations — quieter, more intimate. The hum of the drive. The city of stars outside the window.

---

## 7. Station Raids

### 7.1 Station Types
- **Warehouses** (core type — all stations have this)
- **Science Labs**
- **Farms**
- *(More facility types planned)*

### 7.2 Procedural Generation
Stations are procedurally generated from modular rooms, giant warehouses, and connecting hallways. Each run generates a new layout.

**Key spawns (min/max limits per station):**
- Charge stations (fuel sources)
- Equipment caches
- Upgrade items
- Keycards (unlock restricted areas with extra loot)
- Medical/Cloning bays (player revival points)
- Mission objectives

### 7.3 Fuel — Battery Canisters
Fuel is collected physically. Charge stations inside the station charge battery canisters (jerrycan form factor, battery internals). Players carry canisters out and deposit them in the ship's fuel system.

Canisters are subject to the inventory system — they take up hand/suitcase space like any other item.

### 7.4 Extraction
Players call the ship to a landing zone and extract with their loot. If no player escapes a raid, the team respawns on the ship — but loses time (Legion timer accelerates). There is no permanent death between stations.

---

## 8. Player Systems

### 8.1 Inventory — Two-Hand System
Players have two hands. Each hand carries one item.

| Configuration | Hands Used | Capacity | Tradeoff |
|---|---|---|---|
| Default | 2 | 2 items | Full mobility |
| One suitcase | 1 hand | 2 + 3 = 5 items | One hand free |
| Two suitcases | 2 hands | 2 + 6 = 8 items | Cannot use items |

### 8.2 Weight System
Heavier loads reduce movement speed. Players must balance loot value against mobility. A fully loaded two-suitcase player is a liability in a chase.

### 8.3 Health & Downed State
- HP depletes from damage
- At 0 HP: player falls to the ground (downed, not dead)
- Downed timer: teammates can pick up a downed player within a window
- If timer expires with no rescue: player dies
- Dead players spectate the remaining team
- **Revival:** Medical/cloning bays inside stations can revive dead players — finding one becomes a team priority when players are down

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

**Soldiers** deploy when the timer hits zero *(see open questions — confirm if this replaces or follows the star map game over condition)*. They enter stations and sprint directly to player locations, hunting one by one. Armed. Fast. They know where you are.

This is the final escalation — the Legion has arrived, the ship is cornered, and now it's a race inside the station itself.

### 9.5 The Super Soldier
A single massive threat. Activates after the players escape Ember Moth soldiers **3 times**. The super soldier does not patrol — it hunts. One target at a time. Persistent.

**Design implication:** Escaping soldiers is not a safe strategy — it's a counter. The players are on a clock within the clock. Escape three times and something worse wakes up.

---

## 10. Equipment

Tools collected during raids. Subject to the inventory system — carry cost matters.

**Known equipment:**
- Radar scanner
- Gas mask
- Sponge shoes *(movement/sound reduction)*
- Walkie talkies *(communication — relevant with proximity voice chat)*
- Keycards *(unlock restricted zones)*

*Equipment list to be expanded. Each item should have a clear use case and meaningful inventory cost.*

---

## 11. Missions

Procedurally assigned per station. Displayed on the map panel before entry.

**Mission types:**
- Retrieve crate from [specific location]
- Activate terminal
- Activate door
- Eliminate target

**Mission tiers:**
- **Optional:** Bonus loot for completion
- **Required:** Must be completed before extraction becomes available

Missions layer on top of the fuel objective — a station may be fuel-light but mission-rich, forcing a tradeoff decision on the star map.

---

## 12. Progression

### 12.1 In-Run Progression
Collected during raids, lasts for the current run only:
- **Equipment:** Tools that help players survive and loot more efficiently
- **Player Upgrades:** Stat improvements (speed, carry weight, HP, etc.) — labeled "ship upgrades" in early design notes but apply to the player

### 12.2 Meta Progression
Persists between runs:
- Skins
- Visual customization
- Animations
*Currency/unlock mechanism: TBD*

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
- Proximity voice chat integrated as gameplay (walkie talkies, distance attenuation)
- Enemy audio cues that communicate behavior and threat level
- Ambient station audio that distinguishes facility types
- The Legion's approach on the star map should be felt, not just seen
- Ship interior audio: contrast against the stations — mechanical comfort

**Composer:** Ko *(transitioning from engineering role — knows the universe, scoring the game)*
*Full audio design document: TBD — develop with Ko*

---

## 15. Scope

### Current Focus
- Infinite run with distance/time records
- Core loop: star map → raid → extract → repeat
- Up to 4 players co-op
- Proximity voice chat
- Basic enemy types (workers, robots, scouts)
- Warehouse stations + 1–2 additional types

### Future / Out of Scope for v1
- Multiple story endings
- Full enemy roster expansion
- Additional station types
- Full meta progression system
- Console ports

---

## 16. Open Questions

- Platform target (PC first?)
- Meta progression currency — how is it earned per run?
- Station naming conventions — do stations have identities/factions?
- Ship name
- Is there any in-run narrative delivery? (terminals, logs, environmental storytelling)
- Exact upgrade types — what stats does the player upgrade?
- Is there friendly fire?
- Does proximity voice chat extend to enemies hearing players?

---

*GDD drafted by Sparkle. Built from design session with Lordy, May 5 2026.*
*Silent Providence lore reference: D:\Sparkle\lore\silent-providence.md*
