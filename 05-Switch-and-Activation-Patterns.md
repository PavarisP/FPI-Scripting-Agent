# Switch & Activation Patterns

Switches use **cross-entity activation** — a switch activates another entity (door, light, trap, etc.) via the IFUSED field.

## Basic Toggle Switch

**File**: `switch.fpi`

```fpi
:state=0:hudreset,hudx=50,hudy=90,hudimagefine=gamecore\text\pressentertouse.tga,hudname=useswitchprompt,hudhide=1,hudmake=display,state=10
:plrdistwithin=50:hudshow=useswitchprompt,hudfadeout=useswitchprompt

:state=10,plrdistwithin=50,plrusingaction=1:state=1,plrsound=$0,activateifused=1,alttexture=1
:state=1,plrusingaction=0:state=2
:state=2,plrdistwithin=50,plrusingaction=1:state=3,plrsound=$1,activateifused=0,alttexture=0
:state=3,plrusingaction=0:state=10
```

**Flow**: Idle → press Enter → activate target + switch texture on → release Enter → press Enter again → deactivate target + switch texture off → release → idle.

## Toggle Switch (No Animation)

**File**: `switch2.fpi` — same as `switch.fpi` but uses different distance thresholds (25 for HUD, 50 for activation).

## Momentary Switch (Animated Lever)

**File**: `switch3.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:plrdistwithin=100:hudshow=useswitchprompt,hudfadeout=useswitchprompt

:state=10,plrdistwithin=100,plrusingaction=1:state=1,setframe=0,plrsound=$0,activateifused=1
:state=1:incframe=0
:state=1,frameatend=0:state=2
:state=2,plrusingaction=0:state=3
:state=3,plrdistwithin=100,plrusingaction=1:state=4,plrsound=$1,activateifused=0
:state=4:decframe=0
:state=4,frameatstart=0:state=5,setframe=0
:state=5,plrusingaction=0:state=10
```

**Difference**: Has an **animated lever** — plays the incframe/decframe animation instead of just toggling texture.

## Instance Switch (No Toggle)

**File**: `switch1.fpi` — activates once, then waits for animation to reset. Only toggles in one direction.

```fpi
:state=10,plrdistwithin=100,plrusingaction=1:state=1,setframe=0,plrsound=$0,activateifused=1
:state=1:incframe=0
:state=1,frameatend=0:state=2
:state=2,plrusingaction=0:state=4
:state=4:decframe=0
:state=4,frameatstart=0:state=5,setframe=0
:state=5:state=10
```

## Key-Required Switch

**File**: `switch2key.fpi` / `switch3key.fpi`

Same as toggle/key switch but adds `plrhaskey=1` condition:

```fpi
:state=10,plrdistwithin=50,plrhaskey=1,plrusingaction=1:state=1,plrsound=$0,activateifused=1,alttexture=1
```

## Sequence Switch Puzzle

**File**: `speciallogic/Sequence-switch.fpi`

A multi-switch puzzle where switches must be pressed in a specific order:

```fpi
:state=0:dimvar=allsreset,setvar=allsreset 0,dimlocalvar=hasbeenreset,alwaysactive=1,hudreset,...,hudmake=display,state=10

:state=10,plrdistwithin=100,pickobject=1,plrusingaction=1,varequal=nextsnum %sid:state=4,plrsound=$0,alttexture=1
:state=10,plrdistwithin=100,pickobject=1,plrusingaction=1,varnotequal=nextsnum %sid:setvar=resetswitches 1,state=3

:state=4,varnotequal=nextsnum %scount:addvar=nextsnum 1,state=3
:state=4,varequal=nextsnum %scount:activateifused=1,state=5
```

**How it works**:
- Each switch entity has a unique `%sid` (auto-assigned ID)
- `%scount` = total number of switches in the sequence (set manually per entity)
- `nextsnum` = next expected switch number
- `pickobject=1` — player must be looking at the switch
- If `nextsnum == %sid`, correct — advance `nextsnum`
- If wrong, trigger `resetswitches` to reset all
- When `nextsnum > %scount`, all switches pressed — activate the target

**Appear scripts for sequence** (`speciallogic/appear1-id1.fpi` through `appear1-id4.fpi`):

```fpi
:state=0:dimlocalvar=sid,setvar=sid 1,dimvar=nextsnum,dimvar=resetswitches,dimvar=scount,addvar=scount 1
:state=0,varequal=nextsnum 0:setvar=nextsnum 1
:state=0:setalphafade=100,runfpidefault=1
```

Each ID script initializes the global puzzle state and assigns its `sid`.

## Remote Switch (Activation Without Interaction)

Entities can be activated without a switch using:
- `zoneactivate.fpi` — player walks into a zone → activates IFUSED entity
- `plrinzoneactivateused.fpi` — player in zone, activate IFUSED
- `controlspawn.fpi` — activation controls spawn on/off

## Activation Value System

The `activate=N` action can pass **different values** (0, 1, 2, 3) that scripts can check with `activated=N`:

| Value | Usage |
|-------|-------|
| `activate=1` / `activated=1` | Standard on/activate |
| `activate=0` / `activated=0` | Standard off/deactivate |
| `activate=2` | Secondary activation (e.g., door open) |
| `activate=3` | Tertiary activation (e.g., door close from other side) |

**Example from airlock**: `activate=2` starts door open, `activate=3` starts door close.

## Activation Helper Scripts

| Script | What It Does |
|--------|--------------|
| `zoneactivate.fpi` | When player enters zone, activate IFUSED |
| `zoneanyactivate.fpi` | When ANY entity enters zone, activate IFUSED |
| `zoneanykeyactivate.fpi` | When any key object enters zone, activate IFUSED |
| `plrinzoneactivateused.fpi` | Player in zone → activate IFUSED, with exit handling |
| `plrinzondeeactivateused.fpi` | Player in zone → deactivate IFUSED |
| `controlspawn.fpi` | Activation toggles spawn on/off |

## Switch Pattern Summary

| Variant | Behavior |
|---------|----------|
| `switch.fpi` | Toggle on/off, alt texture |
| `switch1.fpi` | Momentary (animates then resets) |
| `switch2.fpi` | Toggle on/off, closer range |
| `switch3.fpi` | Animated lever toggle |
| `switch2key.fpi` | Toggle + requires key |
| `switch3key.fpi` | Animated lever + requires key |
| `Sequence-switch.fpi` | Order-based puzzle sequence |
