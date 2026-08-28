# Sound & Music Patterns

## Sound Types

| Action | Description |
|--------|-------------|
| `sound=path` | 3D sound at entity position (attenuates with distance) |
| `plrsound=path` | 2D sound in player's ears (no distance attenuation) |
| `loopsound=path` | Continuous looping sound |
| `stopsound` | Stop the current sound |
| `music=path` | Play a music track (pass `0` to stop) |
| `musicoverride=path` | Override global music with local track |
| `musicvolume=N` | Set music volume (0-100) |
| `soundscale=N` | Set sound volume scale (0-100) |

## Parameter Slots

Sounds are typically passed via `$0` and `$1`:

```fpi
:state=1,plrdistwithin=50:state=4,plrsound=$0
:state=2,plrdistfurther=120:state=3,sound=$1,colon
```

## One-Shot Sound

**File**: `soundinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,sound=$0,activateifused=1
```

Plays once when player enters the zone.

## Looping Sound

**File**: `loopsound.fpi`

```fpi
:state=0:loopsound=$0,soundscale=25
```

Starts a looping sound at reduced volume. Runs continuously.

## Looping Sound in Zone

**File**: `soundloopinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,loopsound=$0
:state=1,plrwithinzone=0:state=0,stopsound
```

Loop plays while player is inside the zone, stops when they leave.

## Repeat Sound

**File**: `repeatsound.fpi`

```fpi
:state=0:loopsound=$0,setalphafade=100,runfpidefault=1
```

A looping ambient sound combined with visibility.

## Player Sound in Zone

**File**: `plrsoundinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,plrsound=$0,activateifused=1
```

Plays a 2D sound directly in the player's ears (no distance falloff).

## Music at Level Start

**File**: `changemusicatstart.fpi`

```fpi
:state=0:music=$0,musicvolume=70,soundscale=25
```

Sets the background music when the level loads.

## Music in Zone (While Inside)

**File**: `musicinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,music=$0
:state=1,plrwithinzone=0:state=0,music=0
```

Plays music while inside the zone, stops when leaving.

## Music Override Zone

**File**: `musicchangeinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,musicoverride=$0,musicvolume=50,soundscale=25
:state=1,plrwithinzone=0:state=0
```

Overrides the global music track while in the zone, restores original on exit.

## Music Change (No Restore)

**File**: `musicchangeinzoneandcontinuetoplay.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,music=$0,musicvolume=70,soundscale=50
```

Changes music permanently — doesn't restore when leaving.

## Sound Finished Condition

```fpi
:state=2,soundfinished=1:state=3
```

Waits for the current sound to finish playing before transitioning.

## Single-Sound Zone System

**File**: `plrinzoneonesound.fpi`

```fpi
; Other zones — stop when this one is active
:state=0,plrwithinzone=0,activated=1:stopsound=$0,activate=0

; This zone — play sound, suppress others
:state=0,plrwithinzone=1:activateifused=1,activate=0,sound=$0,state=1
:state=1,plrwithinzone=0:state=0
```

Ensures only one zone's sound plays at a time (all zones must share the same name).

## Sound Pattern Summary

| Script | Type | Loops | Zone-Based | Distance |
|--------|------|-------|------------|----------|
| `soundinzone.fpi` | 3D | No | Yes | Yes |
| `plrsoundinzone.fpi` | 2D | No | Yes | No |
| `soundloopinzone.fpi` | 3D | Yes | Yes (on/off) | Yes |
| `loopsound.fpi` | 3D | Yes | No | Yes |
| `repeatsound.fpi` | 3D | Yes | No | Yes |
| `musicinzone.fpi` | Music | Yes | Yes (on/off) | No |
| `musicchangeinzone.fpi` | Music | Yes | Yes (override) | No |
| `changemusicatstart.fpi` | Music | Yes | No | No |
| `plrinzoneonesound.fpi` | 3D | No | Exclusive | Yes |
