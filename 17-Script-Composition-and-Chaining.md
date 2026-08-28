# Script Composition & Chaining

## How Multiple Scripts Work Together

Entities in FPSC can have **multiple scripts** assigned. They are layered:

```
1. Appear script (runs first, sets up visibility)
2. Main AI script (runs after, controls behavior)
3. Destroy script (runs when entity dies)
```

## runfpidefault=1

The most common composition pattern. An appear script calls `runfpidefault=1` to hand control to the entity's **main/default AI script**:

```fpi
:state=0:setalphafade=100,runfpidefault=1
```

This is how **appear scripts** work — they do initial setup (fade in, set collision, etc.) then yield to the main AI.

## runfpi=filename.fpi

Chains to a **specific** FPI script by filename:

```fpi
:state=3,frameatstart=0:state=0,setframe=0,runfpi=dooruse.fpi
```

Used in `doorremotethendooruse.fpi` — after the remote-controlled door sequence completes, it chains to `dooruse.fpi` for manual push-open behavior.

## Default Script

**File**: `default.fpi`

```fpi
:state=0:state=1
```

Minimal script with `nobulletcol` variant:

```fpi
:state=0:state=1,nobulletcol
```

Used as the default AI for entities that don't need custom behavior.

## AI Behaviour + Waypoint Follow

Some NPC scripts combine waypoint following with combat:

```fpi
:waypointstate=0:animate=2,waypointstart
:waypointstate=3:animate=2,waypointnext
:state=1,plrcanbeseen:settarget,state=2
```

The NPC patrols waypoints normally but switches to combat when it sees the player.

## Appear + Sequence Puzzle

The sequence puzzle system uses **specialized appear scripts** that also initialize puzzle variables:

```fpi
:state=0:dimlocalvar=sid,setvar=sid 1,dimvar=nextsnum,dimvar=resetswitches,dimvar=scount,addvar=scount 1
:state=0,varequal=nextsnum 0:setvar=nextsnum 1
:state=0:setalphafade=100,runfpidefault=1
```

These scripts do double duty: they set up puzzle state AND make the entity visible.

## Script Layering Example (NPC)

For a full NPC setup:

| Slot | Script | Purpose |
|------|--------|---------|
| Appear | `appear1.fpi` | Fade in, enable default AI |
| Main AI | `people/chase.fpi` | Chase and shoot behavior |
| Destroy | `destroy/ragdollcorpsefadeandactivate.fpi` | Ragdoll on death, fade, activate IFUSED |

The appear script runs `runfpidefault=1` which invokes the main AI. When health reaches 0, the destroy script takes over.

## Spawn-on-Activation Entities

For entities that don't exist at level start:

| Slot | Script | Purpose |
|------|--------|---------|
| Appear | `appearspawn.fpi` | Wait for activation, then fade in |
| Main AI | `people/chase.fpi` | Combat behavior after appearing |

## Multi-Script Activation Flow

```
Entity activated (activated=1)
  → appearspawn.fpi: state=1 (spawnon, fade in)
  → appearspawn.fpi: state=4 (runfpidefault=1)
  → chase.fpi: state=0 (starts patrolling/seeking player)
  → chase.fpi: player detected (combat begins)
  → Entity killed (subhealth=0)
  → ragdollcorpsefadeandactivate.fpi: state=0 (ragdoll, activate IFUSED)
```

## alwaysactive=1

```fpi
:state=0:dimvar=allsreset,setvar=allsreset 0,...,alwaysactive=1
```

Keeps the script running even when the entity is far from the player. Used for persistent systems like day/night cycles and sequence puzzles.
