# FPSC Engine Overview

**FPS Creator** (version 3.4) is a complete game development tool for creating first-person shooters. The engine is built around a **component-based entity system** with a **state-machine scripting language** (FPI).

## Directory Structure

```
Files/
├── audiobank/           # Sound effects and music
│   ├── music/           # Background music tracks
│   ├── items/           # Item pickup sounds
│   ├── guns/            # Weapon fire sounds
│   ├── ww2/             # WW2 themed audio
│   ├── scifi/           # Sci-fi themed audio
│   └── ...
├── cubemaps/            # Environment cubemaps for reflections
├── databank/            # Core game data (HUD icons, radar, etc.)
├── EAI/                 # Enhanced AI hand maps
├── editors/             # Editor configuration
│   ├── advanced.ini     # Settings (64-bit patch, HDR)
│   └── currentversion.ini  # Engine version: 3.4
├── effectbank/          # HLSL shader effects (.fx files)
│   ├── postprocess/     # 42 post-processing shaders
│   ├── common/          # Common shader includes
│   ├── weapon_shaders/  # Weapon-specific shaders
│   └── ...
├── entitybank/          # 3D entity definitions (.fpe files)
│   ├── _markers/        # Trigger zones, lights, player start
│   ├── triggers/        # 80+ trigger entity types
│   ├── common/          # Invisible walls, pathfinding
│   ├── Characters/      # NPC character entities
│   ├── ww2/             # WW2 themed entities
│   ├── scifi/           # Sci-fi themed entities
│   └── ... (50+ packs)
├── gamecore/            # Core game assets
│   ├── backdrops/       # Menu backgrounds (8 themes)
│   ├── brass/           # Shell casing models
│   ├── bulletholes/     # Bullet hole decal textures
│   ├── debris/          # Explosion debris textures
│   ├── decals/          # 112 decal effect types
│   ├── flak/            # Flak/camera system
│   ├── guns/            # 21 weapon sets
│   ├── huds/            # HUD textures and layouts
│   ├── muzzleflash/     # 19 muzzle flash textures
│   └── text/            # Prompt text overlays
├── GUI-X9/              # Menu/GUI system (.fpi scripts)
│   ├── setuplevel.fpi   # In-game HUD + pause menu
│   ├── titlepage.fpi    # Main menu (103 lines)
│   ├── loadingpage.fpi  # Loading screen
│   ├── gameover.fpi     # Death screen
│   └── gamewon.fpi      # Victory screen
├── languagebank/        # Localization (6 languages)
│   └── english/         # English assets
│       ├── gamecore/    # Translated HUD/menu textures
│       └── ...
├── levelbank/           # Compiled levels (.zip)
├── mapbank/             # Level files (.fpm)
│   └── (50+ maps, including tutorials)
├── meshbank/            # 3D mesh data (.x files)
│   └── (29 theme packs)
├── prefabs/             # Pre-built room templates
│   └── (141 prefab files)
├── scriptbank/          # FPI scripts (the main focus)
│   └── (198+ scripts across many categories)
├── segments/            # Level geometry segments
│   └── (37 theme packs)
├── skybank/             # Skybox textures
│   └── (13 theme packs)
├── texturebank/         # Texture files (.dds)
│   └── (35 theme packs)
├── videobank/           # Cutscene videos
└── water/               # Water plane assets
```

## Entity System

### Entity Types

Entities in FPSC are defined by `.fpe` files in `entitybank/`. Common types:

| Type | Examples |
|------|----------|
| **Characters** | NPCs, enemies, allies |
| **Items** | Weapons, ammo, health, keys |
| **Triggers** | Zones, lights, post-effects, spawn points |
| **Markers** | Player start, trigger zones, light markers |
| **Doors** | Animated doors, key doors |
| **Props** | Furniture, scenery, vehicles |

### Entity Properties

Each entity has these key properties (set in the editor):

| Property | Description |
|----------|-------------|
| **Main AI** | Primary FPI script |
| **Appear** | Appearance/visibility FPI script |
| **Destroy** | Death/destroy FPI script |
| **IFUSED** | Target entity name for `activateifused` |
| **Spawn at Start** | Whether entity exists at level load |
| **Spawn Life** | Lifetime before auto-destroy |
| **Is Immobile** | Whether entity can move |
| **Physics Always On** | Enable physics for droppable items |
| **Script Params ($0, $1)** | Custom parameters passed to FPI scripts |
| **Explodable** | Can be destroyed by explosions |

## Trigger Entities (80+ types)

Located in `entitybank/triggers/`, these are invisible zone entities that apply engine-level effects:

| Trigger | Purpose |
|---------|---------|
| **Trigger Zone** | Generic zone — script handles logic |
| **Heal Zone** | Heals player inside |
| **Hurt Zone** | Damages player inside |
| **Sound Zone** | Plays sound when entered |
| **Music Zone** | Changes music |
| **Video Zone** | Plays cutscene video |
| **Win Zone / Win Zone2** | Level completion |
| **Post Effect** | Applies post-processing shader |
| **Fog** | Fog density control |
| **Water FX / Water On / Water Off** | Water system |
| **Skybox / Skyscroll** | Sky/environment |
| **Health / Health Regen** | Health management |
| **Blood / Mega Blood / Blood Hud** | Blood effects |
| **Flak Cam / Flak Light** | Camera systems |
| **Flashlight** | Player flashlight |
| **Gravity Control** | Gravity modification |
| **Head Bob / Gun Shake Cam** | Camera effects |
| **FOV** | Field of view control |
| **Slow Motion** | Time dilation |
| **Compass / Radar / Inventory** | HUD elements |
| **Damage Indicator** | Directional damage |
| **Spawn Ally / Spawn Enemy** | Dynamic spawning |
| **Weapon Wheel** | Weapon selection UI |
| **Quick Save** | Save point |
| **Slider Menu** | Settings menu |
| **New Scorch** | Bullet mark effect |
| **NPC Health** | NPC health bar |
| **Hud Overlay / Hud Anim** | Custom HUD |
| **Auto Swap** | Weapon auto-switch |
| **Level 2-10** | Chapter progression |
| **Chapter Complete** | Level completion trigger |
| **Ambient Light** | Environment lighting |
| **Marble Floor** | Floor material effect |
| **Eyehud Timer** | HUD timer element |

## Level File Format (.fpm)

Level files are binary `.fpm` files that contain:
- Entity placements and properties
- Segment/geometry data
- Lighting information
- Script assignments
- Waypoint paths

Compiled levels are stored as `.zip` in `levelbank/`.

## Prefab System

Prefabs (`.fpp` files in `prefabs/`) are pre-built room templates:

```
armoury large/small  → Weapon storage room
bunker large/small   → Bunker
cellar large/small   → Underground cellar
control room         → Sci-fi control room
detention            → Prison/detention
hallway              → Corridor
hangar bay           → Large hangar
laboratory           → Science lab
military             → Military room
radioroom            → Communications room
science lab          → Laboratory
staircase            → Stairwells (bunker/concrete/metal)
storage              → Storage room
study                → Office/study
chateau              → Chateau room
```

Each prefab includes:
- `.bmp` — preview image
- `.fpmb` — binary prefab data
- `.fpmo` — object data
- `.fpol` — overlay data
- `.fpp` — prefab placement file

## Shader/Effect System

**Location**: `effectbank/`

FPSC uses HLSL shader files (`.fx`) organized by capability:

| Directory | Description |
|-----------|-------------|
| `postprocess/` | 42 screen-space post-processing shaders |
| `common/` | Shared shader utilities |
| `bump/` | Bump/normal mapping |
| `phong_bump_specular/` | Phong lighting + specular |
| `physically based shading/` | PBR shaders |
| `cubemap/` | Environment reflection |
| `weapon_shaders/` | Weapon-specific effects |
| `ps_2_0/` | Pixel shader 2.0 |
| `ps_3_0/` | Pixel shader 3.0 |

## Localization System

**Location**: `languagebank/`

Supports 6 languages:
- English, French, German, Italian, Spanish, Deutsch

Localized assets include:
- Menu textures with translated text
- HUD element text
- Help wizard content
- Sound files

## Build System

The engine compiles levels from `.fpm` (editable) to `.zip` (runtime) format. The `advanced.ini` stores settings like 64-bit patch and HDR mode.

## Key Engine Features

1. **State-machine scripting** via FPI language
2. **Cross-entity activation** through IFUSED linking
3. **HLSL shader support** with 42 post-processing effects
4. **Multiple AI systems**: Legacy, DarkAI, EAI
5. **6-language localization**
6. **Prefab system** for rapid level building
7. **Waypoint system** for NPC patrol paths
8. **Physics**: ragdoll, gravity control, water simulation
9. **Dynamic spawning** via spawn points
10. **Full GUI system** with mouse-driven menus
11. **3rd person camera** (Black Ice Mod)
12. **Day/night cycle** with ambience control
13. **Weapon wheel** UI system
14. **BIMA HD scaling** for modern resolutions
