# FPI Scripting Language — Complete Reference

**FPS Creator Intelligence (.fpi)** scripts are plain-text state-machine files that control every entity, zone, pickup, door, light, NPC, and gameplay mechanic in FPS Creator levels.

This repository is a complete reference for the FPI scripting language, designed to be used as a knowledge base for AI coding agents. Optimized for [Kilo](https://kilo.ai) with `.kilo/` commands and agent config, but works with any AI tool — just point your agent at the markdown files.

## Reference Files

| # | File | Topic |
|---|------|-------|
| 00 | `00-FPI-Syntax-Basics.md` | File structure, trigger-action syntax, state machines, root directory config |
| 01 | `01-Conditions-Reference.md` | Complete list of all condition keywords |
| 02 | `02-Actions-Reference.md` | Complete list of all action keywords |
| 03 | `03-Variables-and-Params.md` | Global/local variables, parameter slots ($0, $1), interpolation |
| 04 | `04-Door-and-Lift-Patterns.md` | Door, lift, airlock, transport patterns |
| 05 | `05-Switch-and-Activation-Patterns.md` | Toggle, momentary, keyed switches; cross-entity activation |
| 06 | `06-Pickup-and-Item-Patterns.md` | Weapons, ammo, health, keys, quest items |
| 07 | `07-Zone-and-Trigger-Patterns.md` | Player-in-zone, hurt/heal zones, sound zones, level transitions |
| 08 | `08-Light-and-Effect-Patterns.md` | Light toggle, flicker, candle, move lights; decal/emission |
| 09 | `09-AI-Legacy-Behaviours.md` | The `people/` AI system: chase, shoot, strafe, cover, snipe, patrol |
| 10 | `10-AI-DarkAI-System.md` | DarkAI: teams, cover, melee, ally commands, full AI state machine |
| 11 | `11-Post-Processing-Effects.md` | Fog, rain, bloom, nightvision, underwater, filmgrain, etc. |
| 12 | `12-HUD-System.md` | HUD creation, health bars, ammo counters, prompts, fading |
| 13 | `13-Destroy-and-Corpse-Patterns.md` | Ragdoll, crumble, fade, activate-on-destroy |
| 14 | `14-Advanced-Scripts.md` | Sequence puzzles, day/night cycles, antigravity, explosives, water |
| 15 | `15-3rd-Person-and-Cutscenes.md` | Third person camera, cutscene controls, player forcing |
| 16 | `16-Appear-and-Disappear-Patterns.md` | Entity visibility, fading, spawning, no-collision variants |
| 17 | `17-Script-Composition-and-Chaining.md` | runfpidefault, runfpi, multi-script stacking |
| 18 | `18-Sound-and-Music-Patterns.md` | One-shot, loop, zone-based, global music override |
| 19 | `19-Special-Subdirectories.md` | Weapon wheel, mod packs, theme-specific scripts |
| 20 | `20-Cheat-Sheet.md` | Quick one-page reference of all keywords by category |
| 21 | `21-Engine-Overview.md` | FPSC directory structure, entity system, trigger types, prefabs |
| 22 | `22-Melee-Enemy-Patterns.md` | Melee NPC patterns: chase, attack approach, hit detection, animation frames |
| 23 | `23-DarkAI-Conversion-Guide.md` | How to convert legacy AI scripts to DarkAI |
| 24 | `24-FPSC-Installation-Reference.md` | Actual FPSC install structure, asset counts, path conventions |

## Setup

Before using the agent, configure your FPSC `Files\` directory path in `00-FPI-Syntax-Basics.md` — look for the "Root Directory Configuration" section at the bottom of the file and set it to your installation path (e.g. `C:\Program Files (x86)\The Game Creators\FPS Creator\Files`).

## How FPI Scripts Work (30-Second Summary)

```
; Comment
desc = Short description

:condition1,condition2:action1,action2,action3
```

1. Every line starting with `:` is a **rule**
2. Conditions before the `:` must ALL be true
3. Actions after the `:` execute in sequence
4. The first matching rule each frame fires
5. `state=N` is the core state variable — scripts are state machines
6. `$0`, `$1` are parameter slots filled in the FPSC entity editor
7. `activateifused=1` links entities together (IFUSED field in editor)

Use the default `code` agent to ask questions about any FPI scripting topic — it will read the reference files automatically.
