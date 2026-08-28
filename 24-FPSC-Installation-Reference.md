# FPSC Installation Reference

This file documents the FPSC `Files/` directory structure. The root `Files/` is where FPS Creator is installed — the drive path varies per user (e.g. `C:\Program Files\FPS Creator\Files`, `D:\Software\FPS Creator\Files`, etc.). All paths below are relative to `Files/`.

## Directory Structure & Asset Counts

```
Files/
├── audiobank/          (439 .wav files — sound effects + music)
├── cubemaps/           (environment cubemaps for reflections)
├── databank/           (31 files — core game data, HUD icons, radar)
├── EAI/                (Enhanced AI hand maps)
├── editors/            (editor config: advanced.ini, currentversion.ini)
├── effectbank/         (402 .fx shader files)
│   ├── postprocess/    (42 post-processing shaders)
│   ├── common/         (shared shader includes)
│   ├── bump/           (bump/normal mapping)
│   ├── phong_bump_specular/
│   ├── physically based shading/
│   ├── cubemap/
│   ├── weapon_shaders/
│   ├── ps_2_0/         (pixel shader 2.0)
│   └── ps_3_0/         (pixel shader 3.0)
├── entitybank/         (4,309 .fpe entity definition files)
│   ├── _markers/       (player start, trigger zones, light markers)
│   ├── triggers/       (80+ trigger entity types)
│   ├── common/         (invisible walls, pathfinding)
│   ├── Characters/     (NPC characters)
│   ├── ww2/
│   ├── scifi/
│   └── ... (50+ packs)
├── gamecore/           (core game assets)
│   ├── backdrops/      (menu backgrounds)
│   ├── brass/          (shell casing models)
│   ├── bulletholes/
│   ├── debris/
│   ├── decals/         (112 decal effect types)
│   ├── flak/           (camera system)
│   ├── guns/           (21 weapon sets)
│   ├── huds/           (HUD textures & layouts)
│   ├── muzzleflash/    (19 muzzle flash textures)
│   └── text/           (prompt text overlays)
├── GUI-X9/             (5 .fpi scripts — menu system)
│   ├── setuplevel.fpi  (in-game HUD + pause menu)
│   ├── titlepage.fpi   (main menu)
│   ├── loadingpage.fpi (loading screen)
│   ├── gameover.fpi    (death screen)
│   └── gamewon.fpi     (victory screen)
├── languagebank/       (localization — 6 languages)
├── levelbank/          (compiled levels .zip)
├── mapbank/            (222 .fpm level files)
├── meshbank/           (3,690 .x mesh files, 29 theme packs)
├── prefabs/            (34 .fpp prefab templates)
├── scriptbank/         (1,145 .fpi scripts)
├── segments/           (segment geometry: .bin, .bmp, .db, .dbo, .dds, .fps, .jpg, .png, .txt, .x)
├── skybank/            (118 .dds skybox textures, 13 theme packs)
├── texturebank/        (3,732 .dds texture files, 35 theme packs)
├── videobank/          (10 cutscene videos)
└── water/              (water plane assets)
```

## Scriptbank Categories

The `scriptbank/` directory contains 1,145 `.fpi` scripts organized into these categories:

### Core Scripts (root level, ~100 files)
Doors, lights, pickups, zones, switches, transports, HUD elements, post-effects, music, sound, and utility scripts.

### Subdirectories

| Directory | Description |
|-----------|-------------|
| `341/` | 341 Mod (Appear, Destroy, Items, Main, Shoot sub-folders) |
| `air and oxygen/` | Air/oxygen management systems |
| `ammo huds/` | Ammo counter HUD scripts |
| `behaviours/` | Additional AI behaviour scripts |
| `black ice mod scripts/` | Black Ice Mod (HD, 3rd person) |
| `bond1/` | James Bond themed scripts |
| `BSP-WW2 Models/` | BSP WW2 model scripts |
| `Cod Health/` | Call of Duty style health |
| `conjured/` | Multi-entity teleporter system |
| `culling scripts/` | Visibility culling |
| `Cutscene/` | Cutscene control scripts |
| `Dark AI/` | Enhanced AI system + ghost AI |
| `darkforce/` | Dark force/energy scripts |
| `day night cycle/` | Day/night cycle (daynight.fpi, sun.fpi, sunmoon.fpi) |
| `destroy/` | Ragdoll, crumble, glass, corpse scripts |
| `Desert Storm Pack/` | Desert Storm themed |
| `dungeonpack/` | Dungeon themed |
| `Eckepack/` | Ecke content pack |
| `Explosives/` | Timed explosives, bomb planting |
| `flakscripts/` | Flak/camera scripts |
| `ghost AI/` | Ghost/stealth AI |
| `ghost lol/` | Humorous ghost scripts |
| `health huds/` | Health bar HUD scripts |
| `Horror/` | Horror themed scripts |
| `Hovercar/` | Hovercar vehicle scripts |
| `Kasseyus/` | Kasseyus content pack |
| `M3/` | M3 content pack |
| `metro theater/` | Metro Theater themed |
| `Model pack 6/` | Model pack scripts |
| `modern hud/` | Modern-style HUD scripts |
| `nomadmod/` | Nomad Mod scripts |
| `npc health huds/` | NPC health bar displays |
| `Obj/` | Object scripts |
| `office/` | Office themed scripts |
| `Pavaris/` | Pavaris content pack |
| `people/` | Legacy AI behaviours (chase, shoot, strafe, cover, snipe, pace, etc.) |
| `posteffects/` | 26 post-processing zone scripts |
| `rolfy/` | Rolfy content pack |
| `Scene Scripts/` | Scene/cinematic scripts |
| `scificom/` | Sci-Fi Combat scripts |
| `screenbloodscript/` | Screen blood overlay |
| `slowmotion/` | Slow motion (B key toggle) |
| `speciallogic/` | Sequence switch puzzle system |
| `The Scary Thinker/` | Horror pack scripts |
| `TheStoryteller01/` | Storyteller system |
| `Training Level/` | Tutorial scripts |
| `user/` | User-created scripts |
| `viral_outbreak/` | Zombie outbreak campaign |
| `viral_outbreak - DAI/` | Zombie outbreak with DarkAI |
| `warfarecom/` | Warfare combat scripts |
| `waterscripts/` | Water systems |
| `weaponwheel/` | Weapon wheel UI system |
| `xandy AI/` | X&Y AI system |
| `zombie_apocalypse/` | Zombie apocalypse pack |
| `zombie_apocalypse2/` | Zombie apocalypse 2 pack |

## Key Post-Processing Shaders (effectbank/postprocess/)

42 shader `.fx` files: `bleachbypass`, `bloom`, `blur`, `camera`, `cellshading`, `cinematic bloom`, `color`, `depthoffield`, `fakehdr`, `filmgrain`, `filmnoir`, `filmreel`, `filmsepia`, `fog`, `gasmask`, `hdr_camera_high_key`, `hdr_camera_low_contrast`, `hdr_camera_low_key`, `HDR-false-color`, `monochrome`, `motionblur`, `motionsickness`, `multi`, `nightvision`, `nomadmod`, `none`, `pain`, `rain`, `refract`, `surreal`, `television`, `tonemapping`, `western`.

## Asset Path Conventions

When writing scripts, use these base paths:

| Asset Type | Base Path | Example |
|------------|-----------|---------|
| Sound | `audiobank\` | `audiobank\guns\reload.wav` |
| Music | `audiobank\music\` | `audiobank\music\level1.ogg` |
| HUD Image | `gamecore\huds\` | `gamecore\huds\fader.tga` |
| HUD Text | `gamecore\text\` | `gamecore\text\pressentertouse.tga` |
| Decal | `gamecore\decals\` | `gamecore\decals\sparkle.dds` |
| Video | `videobank\` | `videobank\intro.bik` |
| Entity | `entitybank\` | `entitybank\Modern Day 2\Items\dialtone.wav` |

## Engine Version

- **Engine**: FPS Creator 3.4
- **Config**: `editors\advanced.ini` (64-bit patch, HDR mode)
- **Key mod**: Black Ice Mod v11+ (3rd person camera, HD scaling)
