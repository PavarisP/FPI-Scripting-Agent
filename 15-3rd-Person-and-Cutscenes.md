# 3rd Person & Cutscenes

## Third Person Camera

**Requires**: Black Ice Mod v11+

**Setup**:
1. Place a starter marker in the level
2. Add a character entity (set `isimmobile=yes`)
3. Apply the 3rd person script to the character
4. Configure position with `ThirdPerson` and `ThirdPersonHeight`

### Third Person Commands

| Action | Description |
|--------|-------------|
| `ThirdPerson=X Y` | Activate 3rd person: X units back, Y units left |
| `ThirdPersonHeight=X` | Camera height offset (Y axis) |
| `swapweapon` | Swap 3rd person character weapon model |
| `climb=1` | Hide weapon, play climb animation (for ladders) |
| `climb=0` | Show weapon after climbing |

### Example

```fpi
:state=0:ThirdPerson=80 0,ThirdPersonHeight=70,state=1
```

Camera sits 80 units behind, 0 units left, 70 units above the character.

### Ladder Climbing

Place a ladder + trigger zone with a climb script:

```fpi
:plrwithinzone=1:climb=1
:plrwithinzone=0:climb=0
```

## Cutscene Scripts

Located in `scriptbank/Cutscene/`:

| Script | Description |
|--------|-------------|
| `force to look.fpi` | Force player to look in a specific direction |
| `forceplrlookatobject.fpi` | Force player to look at a specific entity |
| `plrinzoneactivatedufisedthendeactivatedifused.fpi` | Zone: activate target, then deactivate |
| `plrinzonemoveto.fpi` | Move player to a position when entering zone |
| `plrlookat.fpi` | Make player look at an entity smoothly |

## Camera Movement

**File**: `cameramove.fpi`

```fpi
:state=0:animate=1
```

Plays a camera animation path.

## Player Speed Modifier

**File**: `setplrspeedto100.fpi`

```fpi
:state=0:plrspeedmod=40,state=1
:state=1,plrwithinzone=1:activateifused=1
```

Sets player speed at level start. Can also trigger actions when player reaches a zone.

## Story / Video Zones

**File**: `storyinzone.fpi`

```fpi
:state=0,plrwithinzone=1:state=1,sound=$0,video=$1,stopsound=$0,activateifused=1
```

Plays a video (`$1` from `videobank/`) with sound when player enters the zone. Stops sound when video ends.

## Forced Player Look

Force player camera to look at an entity:

```fpi
:state=0,plrwithinzone=1:state=1,plrlookat=EntityName
:state=1,plrwithinzone=0:state=0
```

## Teleport / Transport

**File**: `transportifused.fpi` — teleports player to IFUSED entity
**File**: `transporttoexit.fpi` — teleports player to entity named "Teleporter OUT"
