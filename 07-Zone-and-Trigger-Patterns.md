# Zone & Trigger Patterns

Zones are invisible trigger volumes. When the player (or another entity) enters, the zone fires its script.

## Basic Player-In-Zone

**File**: `plrinzone.fpi`

```fpi
:state=0,plrwithinzone=1:activateallinzone=1,sound=$0,state=1
```

Activates ALL entities inside the zone and plays a sound. One-shot (goes to state 1 and stops).

## Zone Activate (IFUSED Target)

**File**: `zoneactivate.fpi`

```fpi
:state=0,plrwithinzone=1:activateifused=1,state=1,sound=$0
```

Activates only the entity named in the zone's IFUSED field.

## Zone Activate (Any Entity)

**File**: `zoneanyactivate.fpi`

```fpi
:state=0,anywithinzone=1:activateifused=1,state=1,sound=$0
:state=0,anywithinzone=0:activateifused=0,state=0
```

Activates when ANY entity (NPC, enemy, etc.) enters the zone, not just the player.

## Zone Activate (Key Object)

**File**: `zoneanykeyactivate.fpi`

```fpi
:state=0,anykeywithinzone=1:activateifused=1,state=1,sound=$0
```

Activates when any entity tagged as a "key" enters the zone.

## Player In Zone — Activate with Exit

**File**: `plrinzoneactivateused.fpi`

```fpi
:state=0,plrwithinzone=1:activateifused=1,sound=$0,state=1
:state=1,plrwithinzone=0:state=0
```

Turns on when player enters, turns off when player leaves.

## Player In Zone — Deactivate

**File**: `plrinzondeeactivateused.fpi`

```fpi
:state=0,plrwithinzone=1:activateifused=0,sound=$0,state=1
:state=1,plrwithinzone=0:state=0
```

Deactivates IFUSED entity while player is in the zone.

## Player In Zone — One-Shot Activate Then Destroy

**File**: `plrinzoneactivateusedocedestroy.fpi`

```fpi
:state=0,plrwithinzone=1:activateifused=1,sound=$0,destroy
```

Activates once and destroys itself.

## Player Hurt In Zone

**File**: `plrhurtinzone.fpi`

```fpi
:plrwithinzone=1:state=1,plraddhealth=-1
```

Damages the player by 1 HP every frame while inside the zone (no state condition = always-active).

## Player Heal In Zone

**File**: `plrhealinzone.fpi`

```fpi
:plrwithinzone=1:state=1,plraddhealth=1
```

Heals the player by 1 HP every frame while inside.

## Timed Hurt In Zone

**File**: `plrhurtinzone_timed.fpi` — similar but may include timer checks for periodic damage.

## Level Transition (Win Zone)

**File**: `winzone2.fpi`

```fpi
:state=0,plrwithinzone=1:nextlevel=1,sound=$0,reset3
```

Player walks in → advance to next level. `reset3` resets level state.

## Level Transition (Fade Out)

**File**: `nextlevel2.fpi`

```fpi
:state=0:hudreset,hudx=50,hudy=50,hudimagefine=gamecore\huds\fadein\blank.png,hudname=fadeout,hudhide=1,hudmake=display
:state=0,plrwithinzone=1:etimerstart,hudshow=fadeout,state=1,sound=$0

:state=1,etimergreater=100:changehudalpha=fadeout 20,state=2
:state=2,etimergreater=200:changehudalpha=fadeout 40,state=3
; ... increments alpha by 20 every 100ms ...
:state=12,etimergreater=1200:changehudalpha=fadeout 225,state=13
:state=13:nextlevel=2,reset3
```

**Smooth fade**: Uses `etimergreater` (global timer) and `changehudalpha` to gradually fade to black, then transitions level.

## Story Zone

**File**: `storyinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,sound=$0,video=$1,stopsound=$0,activateifused=1
```

Plays a video (`$1`) and sound (`$0`) when player enters the zone.

## Sound Zone

**File**: `soundinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,sound=$0,activateifused=1
```

Plays a 3D sound at the zone's position once.

## Sound Loop Zone

**File**: `soundloopinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,loopsound=$0
:state=1,plrwithinzone=0:state=0,stopsound
```

Plays a looping sound while player is inside the zone, stops when they leave.

## Music Change Zone

**File**: `musicchangeinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,musicoverride=$0,musicvolume=50,soundscale=25
:state=1,plrwithinzone=0:state=0
```

Overrides global music while in zone, restores when leaving.

## Single Sound Zone (Exclusive)

**File**: `plrinzoneonesound.fpi`

```fpi
; Other zones — stop when this one is active
:state=0,plrwithinzone=0,activated=1:stopsound=$0,activate=0

; This zone — play sound, suppress others
:state=0,plrwithinzone=1:activateifused=1,activate=0,sound=$0,state=1
:state=1,plrwithinzone=0:state=0
```

Ensures only one sound plays at a time across zones with the same name.

## Chapter Completion Zone

**File**: `plrinzone2.fpi`

```fpi
:state=0,plrwithinzone=1:passtosetup=levelcompleted 1,activateallinzone=1,sound=$0,state=1
```

Writes `levelcompleted=1` to the setup.ini for campaign progression.

## Zone Activation + Destroy

**File**: `plrinzondeactivateuse0docedestroy.fpi`

```fpi
:state=0,plrwithinzone=1:activateifused=0,sound=$0,destroy
```

Deactivates IFUSED target and destroys itself (one-shot deactivation zone).

## Zone Pattern Summary

| Pattern | Condition | Action | Persistence |
|---------|-----------|--------|-------------|
| One-shot activate | `plrwithinzone=1` | `activateifused=1,state=1` | Fires once |
| Toggle activate | `plrwithinzone=1/0` | `activateifused=1/0` | Tracks entry/exit |
| Hurt zone | `plrwithinzone=1` (no state) | `plraddhealth=-1` | Every frame |
| Heal zone | `plrwithinzone=1` (no state) | `plraddhealth=1` | Every frame |
| Sound zone | `plrwithinzone=1` | `sound=$0` | One-shot |
| Sound loop zone | `plrwithinzone=1/0` | `loopsound` / `stopsound` | Entry/exit |
| Music zone | `plrwithinzone=1/0` | `musicoverride` / restore | Entry/exit |
| Win zone | `plrwithinzone=1` | `nextlevel=N` | One-shot |
| Story zone | `plrwithinzone=1` | `video=$1` | One-shot |
| Any entity | `anywithinzone=1` | `activateifused=1` | Any NPC/enemy |
| Key object | `anykeywithinzone=1` | `activateifused=1` | Key entity only |
