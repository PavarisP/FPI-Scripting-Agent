# Appear & Disappear Patterns

## Instant On

**File**: `appear1.fpi`

```fpi
:state=0:setalphafade=100,runfpidefault=1
```

Most basic appear script. Makes entity fully visible and runs the default AI.

## Instant Off

**File**: `disappear1.fpi`

```fpi
:state=0:destroy
```

Destroys the entity immediately.

## Instant On + No Collision

**File**: `appear1_nocol.fpi`

```fpi
:state=0:coloff,setalphafade=100,runfpidefault=1
```

Makes entity visible but pass-through (no collision).

## Instant On + No Floor Logic

**File**: `appearnofloorlogic.fpi`

```fpi
:state=0:setalphafade=100,floorlogic,runfpidefault=1
```

Enables floor logic (entity stands on floors).

## Instant On + Headshot

**File**: `appearwithheadshot.fpi`

```fpi
:state=0:setalphafade=100,headshot=1,runfpidefault=1
```

Allows headshot damage multiplier on this entity.

## Appear No Collision (Full)

**File**: `appear_nocollision.fpi`

```fpi
:state=0:coloff,nobulletcol,runfpidefault=1
```

No collision AND no bullet collision.

## Appear No Bullet Collision

**File**: `appear_nobulletcollisiononly.fpi`

```fpi
:state=0:colon,nobulletcol,runfpidefault=1
```

Has collision but bullets pass through.

## Appear with Decal

**File**: `appear1_spawndecal.fpi`

```fpi
:state=0:rundecal=8,setalphafade=100,runfpidefault=1
```

Appears with a spawn decal/particle effect (mode 8).

## Appear with Antigravity

**File**: `appearantigravity.fpi`

Appears with antigravity physics enabled.

## Appear No Gravity

**File**: `appearnogravity.fpi`

```fpi
:state=0:nogravity=1
```

Entity floats with no gravity.

## Appear No Gravity 2

**File**: `appearnogravity2.fpi`

Alternate no-gravity variant.

## Appear and Change Music

**File**: `appearchangemusic.fpi`

```fpi
:state=0:musicoverride=$0,runfpidefault=1
```

Overrides level music when entity appears.

## Appear by Activation

**File**: `appearspawn.fpi`

```fpi
:state=0,activated=1:state=1,spawnon
:state=1:state=2,setalphafade=0,setframe=0
:state=2:incframe=0,incalphafade=100
:state=2,frameatend=0:state=3
:state=3,alphafadeequal=100:state=4,runfpidefault=1
```

Entity starts invisible. When activated: fades in while playing its appear animation, then runs default AI.

## Disappear Activate

**File**: `disappearactivate.fpi`

Disappears and activates IFUSED entity.

## Disappear with Blood

**File**: `disappear1_doblood.fpi`, `disappear1_doblood2.fpi`

Disappears with a blood decal effect.

## Script Layering

Appear scripts are designed to **stack on top of** main AI scripts:

```
Entity's scripts:
  1. appear1.fpi          → handles visibility setup
  2. (main AI)            → handles combat/movement
```

The appear script calls `runfpidefault=1` to hand control to the main AI script after setup.

## Appear Script Summary

| Script | Collision | Bullet Col | Visual Effect | Special |
|--------|-----------|------------|---------------|---------|
| `appear1.fpi` | On | On | None | Basic |
| `appear1_nocol.fpi` | Off | Off | None | Pass-through |
| `appear_nocollision.fpi` | Off | Off | None | Full no col |
| `appear_nobulletcollisiononly.fpi` | On | Off | None | Bullets pass |
| `appearnofloorlogic.fpi` | On | On | None | Floor logic |
| `appearwithheadshot.fpi` | On | On | None | Headshot |
| `appear1_spawndecal.fpi` | On | On | Spawn decal | Entrance effect |
| `appearspawn.fpi` | On | On | Fade in | Activation-triggered |
| `appearnogravity.fpi` | On | On | None | Zero gravity |
| `appearantigravity.fpi` | On | On | None | Anti-gravity |
| `appearchangemusic.fpi` | On | On | None | Music override |
| `default_nobulletcol.fpi` | On | Off | None | Bullet col only |

## Disappear Script Summary

| Script | Effect | Activates |
|--------|--------|-----------|
| `disappear1.fpi` | Instant destroy | No |
| `disappear2.fpi` | Destroy variant | No |
| `disappearactivate.fpi` | Destroy | Yes (IFUSED) |
| `disappear1_doblood.fpi` | Blood decal + destroy | No |
| `disappear1_doblood2.fpi` | Blood decal variant | No |
