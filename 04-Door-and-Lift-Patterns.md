# Door & Lift Patterns

## Player Proximity Door (Open & Close)

The most common door pattern. Opens when player approaches, closes when they walk away.

**File**: `door1.fpi`

```fpi
:state=0,plrdistwithin=120:state=4
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
:state=2,plrdistfurther=120:state=3,sound=$1,colon
:state=3:decframe=0
:state=3,frameatstart=0:state=0,setframe=0
:state=4,plrcanbeseen:state=1,setframe=0,sound=$0
:state=4,state=0
```

**Flow**: Idle (state 0/4) → player within 120 → open animation (state 1) → fully open + no collision (state 2) → player leaves → close animation (state 3) → back to idle.

## Auto Proximity Door (Any Entity)

Opens when ANY entity approaches (not just player).

**File**: `autodoor.fpi`

```fpi
:state=0,anywithin=75:activateifused=1,state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
:state=2,anyfurther=100:state=3,sound=$1,colon
:state=3:decframe=0
:state=3,frameatstart=0:state=0,setframe=0
```

## Key Door

Requires the player to have picked up a key.

**File**: `doorkey.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=60:hudshow=keydoorprompt,hudfadeout=keydoorprompt
:state=10,plrdistwithin=60,plrhaskey=1,plrusingaction=1:state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
:state=2,plrdistfurther=60:state=3,sound=$1,colon
:state=3:decframe=0
:state=3,frameatstart=0:state=10,setframe=0
```

**Key difference**: Uses `plrhaskey=1` condition. The HUD prompt shows a "locked door" image.

## Lockpick Door

Variable-based lockpicking mechanic with random re-locking.

**File**: `doorlockpick.fpi`

```fpi
; If global variable [G1] is zero (no lock pick), always fail
:state=0:globalvar=1
:state=0,varequal=0:localvar=1,setvar=1

; Progress through attempts (0, 10, 20, 30, 40, 50)
:state=0,plrdistwithin=120,plrusingaction=1,varequal=0:setvar=1,sound=$0
:state=0,plrdistwithin=120,plrusingaction=1,varequal=10:incvar=1,sound=$0
; ... (steps 20, 30, 40 same pattern)
:state=0,vargreater=60:state=1

; Door opens
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff,localvar=1,setvar=20

; Random re-lock while open
:state=2:localvar=1
:state=2,random=25,varless=21:decvar=1
:state=2:state=3
:state=3,varnotequal=0:state=2

; Door closes when lockpick effect fades
:state=3:localvar=1
:state=3,varequal=0:decframe=0
:state=3,frameatstart=0:state=0,setframe=0,colon
```

## Door (Open Initially)

Starts in the open position.

**File**: `dooropen.fpi`

```fpi
:state=0:state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
```

## Use Door (Push Open & Push Closed)

Player must press the use key to open and close.

**File**: `dooruse.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=60:hudshow=usedoorprompt,hudfadeout=usedoorprompt
:state=10,plrdistwithin=60,scancodekeypressed=33:activateifused=1,activate=2
:state=10,activated=2:state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
:state=2,plrdistwithin=60,scancodekeypressed=33:activate=0
:state=2,activated=0:state=3,sound=$1,colon
:state=2,activated=1:state=5,sound=$1,colon
:state=5:decframe=0
:state=3:decframe=0
:state=3,frameatstart=0:state=10,setframe=0
```

## Remote Controlled Door

Opens/closes when activated externally (via `activateifused` from a switch).

**File**: `doorremote.fpi`

```fpi
:state=0,activated=1:activateifused=1,state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
:state=2,activated=0:state=3,sound=$1,colon
:state=3:decframe=0
:state=3,frameatstart=0:state=0,setframe=0
```

## Airlock Door

Two-stage door (open, then close from other side).

**File**: `airlock_door.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=1
:state=1,plrdistwithin=100:hudshow=openprompt,hudfadeout=openprompt
:state=1,plrdistwithin=100,plrusingaction=1:state=10
:state=10,plrdistwithin=100,plrusingaction=1:activate=2,coloff
:state=10,activated=2:state=11,setframe=0,sound=audiobank\user\
:state=11:incframe=1
:state=11,frameatend=1:state=20
:state=20,plrdistwithin=100,plrusingaction=1:activate=3,colon
:state=20,activated=3:state=21,sound=audiobank\user\
:state=21:decframe=1
:state=21,frameatstart=1:state=0
```

## Lift / Elevator (Up & Down)

Auto lift that moves up and down, carrying the player.

**File**: `lift1.fpi`

```fpi
:state=0,plrhigher=10,plrdistwithin=50:state=6,coloff
:state=1:moveup=1
:state=1,raycastup=20 100:state=2
:state=2,plrdistfurther=55,playerassociated:state=3,unassociateplayer,colon
:state=3,plrhigher=10,plrdistwithin=50:state=7,coloff
:state=4:moveup=-1
:state=4,raycastup=20 0:state=5
:state=5,plrdistfurther=55,playerassociated:state=0,unassociateplayer,colon
:state=6,plrdistwithin=50:sound=...,state=1,associateplayer
:state=6,plrdistfurther=55,playerassociated:state=0,colon
:state=7,plrdistwithin=50:sound=...,state=4,associateplayer
:state=7,plrdistfurther=55,playerassociated:state=3,colon
:activated=1:state=11,activate=0
:state=11,plrhigher=100:state=21
:state=11:state=31
:state=21,raycastup=20 100:state=3
:state=21:state=1
:state=31,raycastup=20 0:state=0
:state=31:state=4
```

**Key concepts**:
- `associateplayer` / `unassociateplayer` — attach/detach player to the lift
- `moveup=N` — vertical movement (positive=up, negative=down)
- `raycastup=range height` — check for ceiling collision before moving
- `playerassociated` condition — verify player is riding
- `plrhigher=N` — player height check to enter/exit

## Transport / Teleporter

**File**: `transportifused.fpi`

```fpi
:state=0:rundecal=2,coloff
:state=0,plrhigher=10,plrdistwithin=50:state=1,plrsound=...,plrmoveifused
:state=1,plrdistfurther=55:state=0
```

**File**: `transporttoexit.fpi`

```fpi
:state=0:rundecal=2,coloff
:state=0,plrhigher=10,plrdistwithin=50:state=1,plrsound=...,plrmoveto=Teleporter OUT
:state=1,plrdistfurther=55:state=0
```

- `plrmoveifused` — teleports to entity named in IFUSED field
- `plrmoveto=Name` — teleports to exact entity name
- Both use decal effect 2 (loop face player) as a visual indicator

## Door Pattern Summary

| Pattern | Trigger | Key Feature |
|---------|---------|-------------|
| Proximity | `plrdistwithin` | Auto open/close |
| Key | `plrhaskey=1` | Requires key item |
| Lockpick | `vargreater=60` | Variable-based attempts |
| Use | `plrusingaction=1` / `scancodekeypressed` | Manual push open/close |
| Remote | `activated=1` | External activation |
| Open initially | Immediate | No close mechanism |
| Airlock | Two-stage | Open, enter, close from other side |

## Lift Pattern Summary

| Variant | Direction | Activation |
|---------|-----------|------------|
| `lift1.fpi` | Up first, then down | Player proximity |
| `lift2.fpi` | Down first, then up | Player proximity |
| `lift1SciFi.fpi` | Up first, ScFi theme | Player proximity |
| `lift1WW2.fpi` | Up first, WW2 theme | Player proximity |
