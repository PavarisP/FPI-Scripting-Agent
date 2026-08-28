# Advanced Scripts

## Sequence Switch Puzzle

**File**: `speciallogic/Sequence-switch.fpi`

A multi-switch puzzle requiring switches to be pressed in the correct order.

**How it works**:
1. Each switch entity has a unique `%sid` (auto-assigned entity ID)
2. `%scount` = total switches in the sequence (set manually per entity)
3. `nextsnum` = the next expected switch number in the sequence
4. `pickobject=1` — player must be looking directly at the switch
5. If `nextsnum == %sid`, the correct switch was pressed → advance `nextsnum`
6. If wrong switch pressed → set `resetswitches=1` to reset all
7. When `nextsnum > %scount`, all switches pressed correctly → activate IFUSED target

**Appear scripts** (`speciallogic/appear1-id1.fpi` through `appear1-id4.fpi`):

```fpi
:state=0:dimlocalvar=sid,setvar=sid 1,dimvar=nextsnum,dimvar=resetswitches,dimvar=scount,addvar=scount 1
:state=0,varequal=nextsnum 0:setvar=nextsnum 1
:state=0:setalphafade=100,runfpidefault=1
```

Each ID script initializes the puzzle state and sets its `sid` (1, 2, 3, or 4).

## Day/Night Cycle

**File**: `day night cycle/daynight.fpi`

```fpi
:state=0:coloff,setalphafade=100
:state=0:dimvar=amb,setvar=amb 0
:always:ambience=%amb
:state=0:state=2

; Sunrise — fade out skybox overlay
:varequal=eax -14,alphafadeequal=100:decalphafade=0
:alphafadeequal=0:setalphafade=0

; Sunrise — increase ambience from 0 to 100
:state=2,varequal=eax -2:state=3
:state=3:etimerstart,state=4
:state=4,varequal=amb 100:state=5
:state=4,etimergreater=50:addvar=amb 1,state=3

; Sunset — fade in skybox overlay
:state=5,varequal=eax -188:alttexture=1,state=6
:state=6,alphafadeequal=0:incalphafade=100
:state=6,alphafadeequal=100:state=7,setalphafade=100

; Sunset — decrease ambience from 100 to 0
:state=7:etimerstart,state=8
:state=8,varequal=amb 0:state=9
:state=8,etimergreater=10:subvar=amb 1,state=7

; Reset with F key
:state=9,scancodekeypressed=33:setvar=amb 0,state=2
```

**Key concepts**:
- `eax` = sun position angle (engine-built-in variable)
- `amb` = ambience level (0-100)
- Skybox overlay entity fades in/out to simulate night
- `alttexture=1` switches to night sky texture
- Sun position at -14 = sunrise, -188 = sunset

## Antigravity

**File**: `antigravity.fpi`

```fpi
:state=0:dimvar=setgrav,setvar=setgrav 0
:state=0:hudreset,...,hudmake=display
:state=0:state=1

:always:fpgcrawtextsize=24,fpgcrawtextx=50,fpgcrawtexty=20,fpgcrawtext=%setgrav

:state=1,keypressed=72 1:state=2
:state=2,keypressed=72 0:state=4
:state=1,keypressed=80 1:state=3
:state=3,keypressed=80 0:state=5

:state=4:addvar=setgrav 1,antigravity=%setgrav,state=1
:state=5:subvar=setgrav 1,antigravity=%setgrav,state=1
```

- NumPad8 (key 72) = increase gravity
- NumPad2 (key 80) = decrease gravity
- `antigravity=%setgrav` — variable interpolation to set the force
- Debug text shows current value

## Explosives

**File**: `Explosives/Explosive_5_seconds.fpi`

```fpi
:state=0,activated=0:state=0
:state=0,activated=1:alttexture=1,etimerstart,loopsound=$0,soundscale=25,state=1
:state=1,etimergreater=5000:subhealth=500
```

1. Idle until activated
2. When activated: switch to armed texture, start timer + beeping sound
3. After 5000ms: subhealth=500 (explodes)

**File**: `Explosives/Plant_Bomb.fpi`

```fpi
:state=0:plrwithinzone=0:state=0
:state=0,plrwithinzone=1:state=2
:state=2:fpgcrawtextsize=24,fpgcrawtextfont=verdana,...,fpgcrawtext=Press [F] to plant the explosive
:state=2,scancodekeypressed=33:sound=$0,soundscale=15,activateifused=1,state=3
:state=3,soundfinished=1:destroy
:state=2,plrwithinzone=0:state=0
```

- Shows "Press F to plant" text when player is in zone
- F key (scan code 33) → activates IFUSED explosive entity → destroys the zone

## Water System

**File**: `waterscripts/water.fpi`

```fpi
:state=0:water=1,waterfogdist=500,waterred=210,watergreen=230,waterheightofzone=0,state=1
```

**File**: `waterscripts/raisewater.fpi` — animated rising water.

**File**: `waterscripts/metrotheaterwater.fpi` — themed water for Metro Theater maps.

## Air & Oxygen System

**File**: `air and oxygen/airsystem.fpi`, `airsystem2.fpi`, `oxygen.fpi`

Manages breathable air and oxygen levels, typically used in space/underwater levels.

## Slow Motion

**File**: `slowmotion/SlowMotionBKey.fpi`, `SlowMotionbkeytimed.fpi`

Press a key to toggle slow-motion effect. Timed variant auto-expires.

## Control Spawn

**File**: `controlspawn.fpi`

```fpi
:state=0,activated=1:spawnon,state=1
:state=1,activated=0:spawnoff,state=0
```

Toggles a spawn point on/off via activation. Useful for wave-based enemy spawning.

## Combination Safe

**File**: `combination safe.fpi`

```fpi
:state=0:hudreset,hudx=50,hudy=90,hudimagefine=gamecore\huds\safehud.tga,hudname=combinationsafe,hudhide=1,hudmake=display,state=10
:state=10,plrdistwithin=80:hudshow=combinationsafe,hudfadeout=combinationsafe
:state=10,plrdistwithin=80,plrusingaction=1:activate=2
:state=10,activated=2:state=1,setframe=0,sound=$0
:state=1:incframe=0
:state=1,frameatend=0:state=2,coloff
```

Shows a safe HUD overlay. Player presses Enter to "dial" the combination, door opens when correct combo is entered.

## Payphone

**File**: `payphone.fpi`

```fpi
:state=0,plrdistwithin=60:state=1,sound=entitybank\Modern Day 2\Items\dialtone.wav,timerstart
:state=1,timergreater=1000:state=0
```

Plays dial tone for 1 second when player approaches.

## NPC Drop Item

**File**: `NPC drop item.txt` (doc)

Two options for NPC-dropped items:
1. **Ammo Spawn Player Take**: NPC dies → ammo spawns → player presses Enter to pick up
2. **Ammo Spawn Give to Player**: NPC dies → ammo spawns → auto-given to player

Setup:
- NPC uses "Destroy and Activate" AI script
- NPC's IFUSED field = name of ammo entity
- Ammo entity: "Spawn at Start = No"
- Ammo script: `ammospawned_plrtake.fpi` or `ammospawned_givetoplr.fpi`

## Health Prison

**File**: `healthprison.fpi`

```fpi
:state=0,plrdistwithin=100,plrdistfurther=0:subhealth=1,plraddhealth=1
:state=0,random=50,plrdistwithin=100,plrdistfurther=0:subhealth=20,plraddhealth=20
:state=0,plrdistwithin=300,plrdistfurther=100:sethealth=100
:state=0,plrdistwithin=400,plrdistfurther=300:addhealth=1,plrsubhealth=1
```

Entity and player are linked in health — damaging one hurts the other. Close range = shared damage, far range = healing.

## LightHouse Spin

**File**: `LightHouseSpin.fpi`

```fpi
:state=0:spinrate=4,
```

Continuous rotation — used for lighthouse beacon lights.

## Hide Crosshair

**File**: `Hide Crosshair.fpi`

```fpi
:state=0:Crosshair=0
```

Hides the player's crosshair (for cutscenes, HUD-less gameplay).
