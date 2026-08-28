# Destroy & Corpse Patterns

## Basic Destroy

**File**: `disappear1.fpi`

```fpi
:state=0:destroy
```

Destroys the entity immediately.

## Destroy and Activate

**File**: `destroy/destroyandactivate.fpi`

```fpi
:state=0:state=1,plrsound=$0,activateifused=1
:state=1:state=2,destroy
```

Plays a sound, activates IFUSED entity, then destroys itself.

## Ragdoll Corpse

**File**: `destroy/ragdollcorpse.fpi`

```fpi
:state=0:state=1,suspend,coloff,ragdoll
```

Suspends physics, disables collision, enables ragdoll. Used as a death script.

## Ragdoll Corpse Fade

**File**: `destroy/ragdollcorpsefade.fpi`

Ragdolls then fades out over time. Variants: `ragdollcorpsefadeandactivate.fpi` (also activates IFUSED).

## Ragdoll Corpse Dynamic

**File**: `destroy/ragdollcorpsedynamic.fpi`

Ragdoll with dynamic physics (can be pushed). Variants: `ragdollcorpsedynamicandactivate.fpi`, `ragdollcorpsedynamicfade.fpi`, `ragdollcorpsedynamicfadeandactivate.fpi`.

## Crumble (Destructible Wall)

**File**: `destroy/crumble.fpi`

```fpi
:state=1:hide,state=2
:state=2,soundfinished=1:destroy
:state=0:emitforce=1,setforcedamage=20,activateifused=1,sound=audiobank\smash\concrete.wav,rundecal=0,state=1
```

Activated → emits force, plays concrete smash sound, runs decal, hides, then destroys when sound finishes.

## Glass Break

**File**: `destroy/glasssmashsmall.fpi`, `destroy/glasssmashmedium.fpi`, `destroy/glasssmashlarge.fpi`

Three sizes of glass break effect. Used on windows.

## Window

**File**: `destroy/window.fpi`

Full window destruction with glass shards.

## Flammable

**File**: `destroy/flamable.fpi`

Entity catches fire and burns before being destroyed.

## Keep Alive

**File**: `destroy/keepalive.fpi`

Prevents entity from being destroyed (immortal).

## Leave Corpse

**File**: `destroy/leavecorpse.fpi`

Leaves a static corpse mesh behind.

## Corpse Options Summary

| Script | Ragdoll | Dynamic | Fade | Activate IFUSED |
|--------|---------|---------|------|-----------------|
| `ragdollcorpse.fpi` | Yes | No | No | No |
| `ragdollcorpsefade.fpi` | Yes | No | Yes | No |
| `ragdollcorpsefadeandactivate.fpi` | Yes | No | Yes | Yes |
| `ragdollcorpsedynamic.fpi` | Yes | Yes | No | No |
| `ragdollcorpsedynamicandactivate.fpi` | Yes | Yes | No | Yes |
| `ragdollcorpsedynamicfade.fpi` | Yes | Yes | Yes | No |
| `ragdollcorpsedynamicfadeandactivate.fpi` | Yes | Yes | Yes | Yes |

## Destroy via Sub-Health

Any entity can be destroyed by reducing its health:

```fpi
:state=1,etimergreater=5000:subhealth=500
```

Used in explosive scripts — when the timer expires, the entity's health drops below zero, triggering the engine's built-in destroy behavior.

## Explosive Patterns

See `scriptbank/Explosives/`:

| Script | Behavior |
|--------|----------|
| `Explosive_5_seconds.fpi` | Activated → loop sound → 5 sec countdown → subhealth=500 (explode) |
| `Explosive_15_seconds.fpi` | Same, 15 second timer |
| `Explosive_30_seconds.fpi` | Same, 30 second timer |
| `Plant_Bomb.fpi` | Player presses F in zone → activates IFUSED explosive → destroy zone |
