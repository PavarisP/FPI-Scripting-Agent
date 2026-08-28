# Special Subdirectories

## Weapon Wheel

**Location**: `scriptbank/weaponwheel/`

A complete weapon selection wheel system.

| Script | Description |
|--------|-------------|
| `weaponwheel_main.fpi` | Main weapon wheel logic |
| `weaponwheel_main2.fpi` | Alternate version |
| `weaponwheel_mouse_scroll.fpi` | Mouse scroll support |
| `colt_weapon.fpi` | Colt weapon pickup |
| `colt_weaponglow.fpi` | Colt glowing pickup |
| `commando_weapon.fpi` | Commando weapon |
| `commando_weaponglow.fpi` | Commando glowing |
| `law_weapon.fpi` | LAW rocket launcher |
| `law_weaponglow.fpi` | LAW glowing |
| `mills36_weapon.fpi` | Mills 36 grenade |
| `mossberg_weapon.fpi` | Mossberg shotgun |
| `python_weapon.fpi` | Python revolver |
| `tavor_weapon.fpi` | Tavor assault rifle |
| `uzi_weapon.fpi` | Uzi SMG |

Each weapon has a normal and glow variant. The glow variants add `rundecal=5,spinrate=4,floatrate=10` for visual attraction.

## 341 Mod

**Location**: `scriptbank/341/`

A complete mod with its own folder structure:
- `Appear/` — custom appear scripts
- `Destroy/` — custom destroy scripts
- `Items/` — custom item scripts
- `Main/` — main AI scripts
- `Shoot/` — shooting behavior scripts
- `TF341_anim_viewer.fpi` — animation viewer tool

## Conjured Teleporter System

**Location**: `scriptbank/conjured/`

Multi-entity teleporter system:

| Script | Description |
|--------|-------------|
| `teleporter_main.fpi` | Main teleporter logic |
| `teleporter_activation.fpi` | Activation trigger |
| `teleporter_beam.fpi` | Beam visual effect |
| `teleporter_conduit.fpi` | Conduit entity |
| `teleporter_destination.fpi` | Destination marker |
| `teleporter_top.fpi` | Top cap entity |

## Flak Scripts

**Location**: `scriptbank/flakscripts/`

| Script | Description |
|--------|-------------|
| `40mm.fpi` | 40mm grenade launcher flak |

Flak cam scripts for camera switching during weapon use:

| Script | Description |
|--------|-------------|
| `flakcam_on.fpi` | Always-on flak cam |
| `flakcam_off_idle.fpi` | Flak cam on for LAW/grenade, off when idle |
| `flakcam_on_law.fpi` | Flak cam for LAW |
| `flakcam_plronground.fpi` | Player on ground check |

## Post Effects

**Location**: `scriptbank/posteffects/`

26 post-processing scripts covering: fog, rain, bloom, nightvision, underwater, filmgrain, filmnoir, filmreel, filmsepia, gasmask, motionblur, motionsickness, pain, refract, surreal, television, tonemapping, western, bleachbypass, cellshading, depthoffield, none, and mod-specific variants.

## Day Night Cycle

**Location**: `scriptbank/day night cycle/`

| Script | Description |
|--------|-------------|
| `daynight.fpi` | Full day/night cycle with ambience |
| `sun.fpi` | Sun position/rotation |
| `sunmoon.fpi` | Sun and moon combined |
| `sun_moon_combined.fpi` | Merged sun/moon entity |

## Water Scripts

**Location**: `scriptbank/waterscripts/`

| Script | Description |
|--------|-------------|
| `water.fpi` | Basic water setup |
| `raisewater.fpi` | Animated rising water |
| `metrotheaterwater.fpi` | Metro Theater themed water |

## Air & Oxygen

**Location**: `scriptbank/air and oxygen/`

| Script | Description |
|--------|-------------|
| `airsystem.fpi` | Air management system |
| `airsystem2.fpi` | Air system variant |
| `oxygen.fpi` | Oxygen pickup/management |

## Slow Motion

**Location**: `scriptbank/slowmotion/`

| Script | Description |
|--------|-------------|
| `SlowMotionBKey.fpi` | Toggle slow-mo with B key |
| `SlowMotionbkeytimed.fpi` | Timed slow-mo |

## Explosives

**Location**: `scriptbank/Explosives/`

| Script | Description |
|--------|-------------|
| `Explosive_5_seconds.fpi` | 5 second timed explosive |
| `Explosive_15_seconds.fpi` | 15 second timed explosive |
| `Explosive_30_seconds.fpi` | 30 second timed explosive |
| `Explosive_5_seconds_Notrigger.fpi` | 5s without trigger |
| `Plant_Bomb.fpi` | Bomb planting zone |
| `Planting Explosives/` | Sub-folder with more variants |
| `destroyscript.fpi` | Generic destroy on activate |
| `artileery destroy.fpi` | Artillery destruction |
| `Tigerdestroy.fpi` | Tank destruction |

## HUD Subdirectories

**Location**: `scriptbank/ammo huds/` — 4 ammo counter HUD scripts
**Location**: `scriptbank/health huds/` — 5 health bar HUD scripts
**Location**: `scriptbank/modern hud/` — 3 modern-style HUD scripts
**Location**: `scriptbank/npc health huds/` — NPC health bar display

## Mod-Specific Packs

| Directory | Mod |
|-----------|-----|
| `black ice mod scripts/` | Black Ice Mod (HD, 3rd person) |
| `Cod Health/` | Call of Duty style health system |
| `bond1/` | James Bond themed scripts |
| `BSP-WW2 Models/` | BSP WW2 model scripts |
| `Dark AI/` | Enhanced AI system |
| `Dark AI/ghost AI/` | Ghost/stealth AI behaviors |
| `Desert Storm Pack/` | Desert Storm themed |
| `dungeonpack/` | Dungeon themed |
| `Eckepack/` | Ecke content pack |
| `Hovercar/` | Hovercar vehicle scripts |
| `Kasseyus/` | Kasseyus content pack |
| `M3/` | M3 content pack |
| `nomadmod/` | Nomad Mod scripts |
| `Pavaris/` | Pavaris content pack |
| `rolfy/` | Rolfy content pack |
| `scificom/` | Sci-Fi Combat scripts |
| `The Scary Thinker/` | Horror pack scripts |
| `viral_outbreak/` | Zombie outbreak campaign |
| `viral_outbreak - DAI/` | Zombie outbreak with DarkAI |
| `xandy AI/` | X&Y AI system |
| `zombie_apocalypse/` | Zombie apocalypse pack |
| `zombie_apocalypse2/` | Zombie apocalypse 2 pack |
