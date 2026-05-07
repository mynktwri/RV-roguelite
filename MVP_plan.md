# RV Roguelite — MVP Plan

## Project Overview

**Concept:** A 1–4 player online co-op roguelite centered around an upgradeable RV. Players drive through procedurally generated maps, scavenge for resources, fight enemies, and manage survival systems — all while keeping the RV intact. The RV serves as mobile shelter, storage, and the primary vehicle for progression between runs.

**Engine:** Unity
**Team Size:** 1–4 developers
**Target Timeline:** 3–6 months for rough playable demo
**Art Style:** Stylized-realistic hybrid (~60% realistic, ~40% stylized)
**Perspective:** 3D top-down 
**Multiplayer:** Online co-op via Steam P2P (host-based)

---

## Core Gameplay Loop

```
CAMP (Menu Hub)
  → Manage storage, craft, upgrade RV, heal/rest characters
  → Select run type, choose loadout from camp storage
  → Reroll biome modifiers (costs in-game time: hunger/thirst drain)

RUN (Procedural Map)
  → Drive RV along a winding procedural path from start to exit
  → Stop at POIs to loot (risky — time-consuming, stealth-dependent)
  → Fight or avoid enemy groups (skeletons)
  → Navigate environmental hazards and biome modifiers
  → Manage survival systems (health, food, sanity)
  → Reach exit to extract

EXTRACTION
  → Characters and RV return to camp with all loot
  → Loot auto-deposited into communal camp storage
  → Surviving characters gain skill progress
  → Repeat
```

---

## The RV

The RV is the central gameplay system. It is home base, transport, and the single most important asset the party owns.

### MVP RV Systems 

For MVP, the RV needs at minimum:

- A drivable vehicle with basic physics (acceleration, steering, braking)
- A health/damage system (anything can damage the RV, including players)
- An interior space players can enter/exit (RV must be stationary for entry/exit)
- Basic storage capacity for loot
- Fuel consumption during runs

### Deferred RV Features (Post-MVP)

- Component-based damage (tires, engine, windshield, etc.)
- Upgrade tiers and parts
- Extensions (roof upgrade, trailer, armor plating, rollout canopy)
- Repair/maintenance mechanics during runs

---

## Run Structure

Each run is a single procedurally generated map with hand-crafted POIs. Maps follow a winding path from a start point to one or more exits. Run types unlock progressively via meta-progression.

### Run Types for MVP

| Run Type | Size | Duration | Exits | Loot Pool | Biome Modifiers |
|----------|------|----------|-------|-----------|-----------------|
| **Run A (Intro)** | Small | ~20 min | 1 | Scrap, low gas, Tier 1 weapons | Easy |
| **Run B (Easy)** | Medium | ~40 min | 1 | Scrap, medium gas, Tier 1–2 weapons, medical | Some; chance of a bad one |
| **Run C (Intermediate)** | Medium | ~45–60 min | 2 (1 rewarding but hazardous) | Gas, Tier 2–3 weapons, medical, crafting mats, unique items | Hard |
| **Run D (Freedom Run)** | Large | ~1h 30m | 2 (1 freedom exit, 1 bail-out) | Gas, Tier 3 weapons, medical, unique items (low density) | Hard |

**Duration note:** Run C's longer time is due to difficulty, not map size.

### Biome Modifier Rerolling

Players can reroll biome modifiers and loot estimates before launching a run. Each reroll costs in-game time (advances the clock), causing hunger and thirst drain on characters. This mechanic doubles as a "sleep through the night" function — spending 3 hours of game time at camp can fully restore sanity.

---

## Map Generation

### Approach

Procedurally generated layouts with hand-crafted POI templates and randomized loot/enemy placement. Maps follow a winding road from start to exit(s).

### MVP Map Content

**POIs (Hand-Crafted Templates)**
- Abandoned houses
- Convenience stores
- Gas stations

**Enemy Encounters**
- Skeleton groups, wandering randomly across the map
- Once aggroed, skeletons chase until 30m away
- Skeletons can damage the RV

**Environmental Hazards**
- Acid pits
- Gas spills
- Trash piles

**Biome Modifiers**
- Heavy storm
- High wind
- Extreme heat

### MVP Biome Count

One biome is sufficient for MVP. POIs and loot tables should reflect the biome context (e.g., desert locale vs. swamp). Additional biomes are a post-MVP expansion.

---

## Player Systems

### MVP Survival Stats

| Stat | Purpose | Drain Source | Recovery Source |
|------|---------|-------------|-----------------|
| **Health** | Core life resource; death at zero | Combat damage, hazards, falls | Medical items, rest at camp |
| **Food** (hunger + thirst combined) | Time pressure; forces looting | Time in map, rerolling at camp | Food/water items found in POIs |
| **Sanity** | Perception and awareness | Monster proximity, taking damage, certain food/water, certain POIs, inclement weather, time in map | Camp rest (3hr reroll = full refill), certain food/water, driving in clear weather, being inside the RV, proximity to teammates |

**Deferred stats (post-MVP):** Stamina, separate hunger/thirst, disease system.

### Sanity System

Sanity uses a threshold system with escalating effects at 40%, 25%, 10%, and 5% of max sanity.

**Effects of low sanity:**
- Reduced **perception cone** (screen vignette closes in)
- Reduced **vision cone** (what the character can see in the top-down view shrinks)
- Audio distortion
- Debuffs (TBD — placeholder for MVP)

**Co-op design note:** "Being near friends" as a sanity recovery source creates a natural mechanical incentive to stay together, while looting efficiency encourages splitting up. This tension is intentional.

### Injury System (Deferred)

A limb-based injury system (limbs, torso, head) is planned but deferred past MVP. For MVP, health is a single pool.

### Character Equipment

- Limited personal inventory (expandable with backpack)
- Clothing items equippable per body region (aligned with future injury system)

---

## Combat

### Melee (MVP)

- Simple swing-and-hit
- No combos, stamina costs, or directional attacks for MVP
- Expand post-MVP

### Ranged (MVP)

- Aim with mouse + right-click
- Shoot with left-click
- Guns attract nearby enemies (noise-based aggro system)

### Enemy Behavior (MVP — Skeletons)

- Wander randomly in groups across the map
- Aggro on proximity or noise (gunfire)
- Chase players until 30m distance
- Can damage the RV
- Can damage players

### Stealth

- Looting is safer via stealth
- Stealth effectiveness affected by weather and biome modifiers
- Guns break stealth and attract attention

---

## Loot and Inventory

### Loot Tiers

| Category | Examples |
|----------|----------|
| **Scrap** | Basic crafting/upgrade currency |
| **Gas** | RV fuel |
| **Weapons (Tier 1–3)** | Melee and ranged, scaling in power |
| **Medical** | Health recovery items |
| **Crafting Materials** | Used at camp for crafting |
| **Unique Items** | Rare finds, run C+ |

### Looting Mechanics

- Opening a container starts an automatic search timer
- Player is vulnerable while searching
- Items are **discovered on a server basis** — once one player discovers an item, all players can see it without re-searching
- Loot availability reflects biome context and POI type

### Inventory Flow

1. Players carry loot in personal inventory (limited space, expandable with backpack)
2. Players can store loot in the RV during a run
3. Upon extraction, all loot is auto-deposited into communal camp storage
4. Before a run, players can load items from camp storage into the RV
5. Only the RV (with upgrades), the character (with skills/gear), and chosen loadout items enter the map

---

## Death and Failure

### Player Death

A dead player has two options:

1. **Haul to RV (revive):** Teammates carry the body back to the RV. The player respawns as the same character with all skills, items, and upgrades intact. This can only happen **once per character per map**.

2. **Respawn as new character:** The dead player respawns as a brand-new character with no skills, items, or upgrades. The new character parachutes in at any point on the map. The previous character is **permanently lost**. The new character can be cosmetically identical if desired.

### RV Destruction

- The RV can be destroyed during a run
- Players can still survive on foot and attempt to reach the exit
- If they extract without the RV, the run ends immediately
- Characters return to camp, but the RV is lost
- A new baseline RV (no upgrades) is provided at camp

---

## Progression

### Character Progression

- Surviving characters gain skill levels after successful runs
- Progression caps at a ceiling to prevent veterans from being game-breaking
- Veteran characters are meaningfully stronger but carry higher stakes on death (permanent loss)

### Freedom Run (Veteran Retirement)

- Players can take a veteran character on a Freedom Run (Run D)
- If the character reaches the freedom exit, the player earns **special meta-currency**
- The retired character is **permanently removed** from the roster — they cannot be used again
- Creates a core tension: keep using a strong character vs. bank their value before losing them

### Meta-Progression

Special currency is earned from:
- Completing Freedom Runs (primary source)
- Quest completion
- Rare loot finds

Special currency is spent on permanent unlocks that persist regardless of character or RV loss. Unlock specifics are TBD but may include: new weapon types, RV upgrade blueprints, new run types, crafting recipes, cosmetics.

---

## Camp (Between Runs)

### MVP Implementation

Menu-based UI hub (not a physical walkable space).

### Camp Activities

| Activity | Description |
|----------|-------------|
| **Manage Storage** | View and organize communal loot storage; move items into RV for next run |
| **Craft** | Use crafting materials to create items (recipes TBD) |
| **Upgrade RV** | Spend resources to improve RV systems (specifics TBD) |
| **Heal / Rest** | Restore character health and sanity; rerolling time at camp refills sanity |
| **Launch Run** | Select run type, review biome modifiers, reroll if desired, deploy |

### Co-op Camp

- All players share the same camp
- Storage is communal
- All players can access all camp activities

---

## Multiplayer

### MVP Networking

- **Online co-op:** 1–4 players
- **Steam Peer-to-Peer:** One player hosts, others connect
- **Host authority:** Host machine handles world state (enemy positions, loot generation, RV state)
- **Solo play:** Functional but not specifically balanced for MVP; balancing is deferred

### Key Sync Requirements

- RV position and state
- Player positions, health, inventory
- Enemy positions and aggro state
- Loot container states (searched/unsearched, discovered items)
- Sanity effects (per-player, but shared proximity bonuses)

---

## MVP Scope Summary

### In Scope (Must Have)

- 3D top-down driving and on-foot gameplay
- Drivable RV with health, fuel, storage, and entry/exit
- 1 biome with procedural map generation and hand-crafted POI templates
- 1 run type Intro
- 3 POI types (house, convenience store, gas station)
- 1 enemy type (skeletons) with wander + chase AI
- 3 environmental hazards (acid pit, gas spill, trash pile)
- 3 biome modifiers (heavy storm, high wind, extreme heat)
- Melee and ranged combat (simple)
- Noise-based aggro from gunfire
- Stealth-influenced looting with search timers
- Shared loot discovery system
- Player survival: health, food, sanity
- Sanity threshold system with perception/vision cone shrinkage and audio distortion
- Death system (haul revive or respawn as new character)
- RV loss and recovery flow
- Character skill progression with caps
- Freedom Run retirement mechanic
- Meta-currency and permanent unlock system (framework)
- Camp menu hub (storage, craft, upgrade RV, heal, launch)
- Online co-op (1–4 players) via Steam P2P
- Biome modifier rerolling with time cost

### Out of Scope (Post-MVP)

- Multiple biomes / regional settings
- more types of runs
- Multi-map successive runs
- Component-based RV damage
- RV extensions (roof, trailer, armor, canopy)
- Physical walkable camp
- Stamina system
- Separate hunger and thirst
- Disease system
- Limb-based injury system
- Advanced melee combat (combos, blocking, dodging)
- Solo play balancing
- Additional enemy types
- NPC survivors / random encounters
- Optional objectives / side quests
- Advanced crafting trees

---

## Open Questions and Placeholders

These items were intentionally left as placeholders during planning. Each needs a design pass before or during development:

1. **RV systems detail** — What specific components can break, be repaired, or be upgraded? What does the upgrade tree look like?
2. **Crafting recipes** — What can be crafted, and what materials are needed?
3. **Meta-currency unlock tree** — What permanent unlocks are available and how are they priced?
4. **Sanity debuffs** — Beyond perception/vision/audio, what mechanical debuffs occur at each threshold?
5. **Weapon roster** — Specific weapons per tier, damage values, noise levels, durability
6. **Biome modifier effects** — Exact mechanical impact of each modifier on gameplay
7. **Character skill system** — What skills exist, how do they level, what's the cap?
8. **Loot tables** — Drop rates, per-POI loot pools, per-run-type distributions
9. **Run unlock requirements** — What must players achieve to unlock Run B, C, and D?
10. **Solo play considerations** — If solo becomes a priority, what needs rebalancing?

---

## Recommended Development Phases (3–6 Month Timeline)

### Phase 1 — Core Movement and World (Weeks 1–4)

- RV driving (basic physics, acceleration, steering)
- Player on-foot movement
- RV entry/exit (stationary only)
- Basic procedural map generation (winding path, flat terrain)
- Camera system (top-down, follows RV while driving, follows player on foot)

### Phase 2 — Combat and Enemies (Weeks 5–8)

- Melee attack (swing and hit)
- Ranged attack (aim + shoot)
- Skeleton enemy: wander, aggro, chase, disengage at 30m
- Noise-based aggro from gunfire
- Health system (player and RV)
- Player death flow (ragdoll/body, haul mechanic, respawn option)
- RV destruction state

### Phase 3 — Looting and Survival (Weeks 9–12)

- POI templates (house, convenience store, gas station)
- Lootable containers with search timer
- Shared discovery system (server-authoritative)
- Inventory system (personal + RV storage)
- Food system (combined hunger/thirst)
- Sanity system (drain sources, recovery sources, threshold effects)
- Vision/perception cone rendering
- Environmental hazards (acid pit, gas spill, trash pile)

### Phase 4 — Progression and Camp (Weeks 13–16)

- Camp menu UI (storage, craft placeholder, upgrade RV placeholder, heal/rest, launch)
- Communal storage system
- Run type selection and biome modifier reroll
- Character skill progression (framework)
- Meta-currency tracking
- Freedom Run exit logic
- Run extraction flow (loot transfer to camp)
- Fuel consumption and gas loot

### Phase 5 — Multiplayer and Polish (Weeks 17–22)

- Steam P2P networking
- Host-authoritative world state sync
- Player join/leave handling
- Biome modifiers (storm, wind, heat — visual and mechanical)
- Stealth system (basic detection)
- Audio distortion for low sanity
- Run A through D map size and difficulty scaling
- Playtesting and iteration

### Phase 6 — Demo Polish (Weeks 23–26)

- Bug fixing and stability
- UI/UX pass on camp and in-run HUD
- Audio pass (ambient, combat, RV, weather)
- Art pass on POIs and map elements
- Performance optimization
- Demo packaging and distribution
