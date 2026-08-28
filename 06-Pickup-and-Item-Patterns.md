# Pickup & Item Patterns

## Basic Pickup Item

**File**: `pickup1.fpi`

```fpi
:state=0:hudreset,hudx=50,hudy=90,hudimagefine=gamecore\text\pickedupanitem.tga,hudname=itemprompt,hudhide=1,hudmake=display,state=10
:state=10,plrdistwithin=40:state=1,playertake,coloff,plrsound=audiobank\misc\ping.wav,hudshow=itemprompt,hudfadeout=itemprompt
:state=1:rundecal=5
```

**Pattern**: Setup HUD in state 0 → wait in state 10 → player within 40 units → pick up, disable collision, play sound, show HUD fadeout → run decal effect 5 (sparkle) then done.

## Weapon Pickup

**File**: `weapon.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=40:state=1,playertake,coloff,hideshadow=1,rundecal=-1,plrsound=...,hudshow=weaponprompt,hudfadeout=weaponprompt
:state=3,plrdistfurther=65:state=10
```

**Key differences**: `hideshadow=1`, stops decal with `rundecal=-1`, includes a drop mechanic via commented-out `playerdrop`.

## Ammo Pickup

**File**: `ammo.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=40,cantake:state=1,playertake,coloff,hideshadow=1,plrsound=...,hudshow=ammoprompt,hudfadeout=ammoprompt
```

**Key difference**: Uses `cantake` condition — only collectible if player has room for more ammo.

## Health Pickup

**File**: `pickuphealth.fpi`

```fpi
:state=0,cantake,plrdistwithin=80:state=1,playertake,plraddhealth=50,hideshadow=1,plrsound=audiobank\items\healthup.wav
```

**Simple**: No HUD prompt needed. `cantake` ensures player isn't at full health. `plraddhealth=50` adds health.

## Health Station (Reusable)

**File**: `healthuse.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=60:hudshow=useprompt,hudfadeout=useprompt
:state=10,plrdistwithin=100,plrusingaction=1:state=1,plraddhealth=2,plrsound=...
:state=1,plrusingaction=0:state=0
```

**Reusable**: Player presses Enter to heal 2 HP each time. Returns to state 0 when they release the key.

## Key Pickup

**File**: `pickupkey.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10
:state=10,plrdistwithin=40:activateifused=1,state=1,playertake,coloff,plrsound=...,hudshow=keyprompt,hudfadeout=keyprompt
:state=1:rundecal=5
```

**Key difference**: `activateifused=1` activates the door/entity linked in the IFUSED field when picked up.

## Pickup and Drop (H Key)

**File**: `pickupcandrop.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,state=10,coloff
:state=10,plrdistwithin=40:state=1,playertake,plrsound=$0,hudshow=itemprompt,hudfadeout=itemprompt
:state=1,scancodekeypressed=35:plrsound=$1,playerdrop
:state=2,plrdistfurther=45:state=10
```

**Note**: `playerdrop` auto-increments the state. Player presses H (scan code 35) to drop the item. Requires "Physics Always On" on the entity.

## Slippy Pickup (Random Drop)

**File**: `pickupslippy.fpi`

```fpi
:state=1,random=200:plrsound=$1,playerdrop
```

Item randomly drops from the player's hands (1 in 200 chance per frame).

## Quest Item Pickup

**File**: `pickupitemA.fpi`

```fpi
:state=0:hudreset,...,hudmake=display,dimvar=collecteditemA,setvar=collecteditemA 0,state=10
:state=10,plrdistwithin=40:state=1,playertake,coloff,plrsound=...,hudshow=itemprompt,hudfadeout=itemprompt,setvar=collecteditemA 1
```

**Sets a global variable** `collecteditemA = 1` when picked up. Other scripts (e.g., NPC dialog) can check `varequal=collecteditemA 1`.

## Lockpicks Pickup

**File**: `pickuplockpicks.fpi`

```fpi
:state=0,plrdistwithin=40:state=1,playertake,coloff,plrsound=...
:state=1:rundecal=5,globalvar=1,setvar=1
```

Sets entity global variable G1 to 1, which the lockpick door script checks.

## Glowing Pickups

**File**: `weaponglow.fpi`, `ammoglow.fpi`, `pickuphealthglow.fpi`

Add spinning and floating effects:

```fpi
:state=10:rundecal=5,spinrate=4,floatrate=10
```

- `spinrate=N` — auto-rotate
- `floatrate=N` — bobbing up/down
- `rundecal=5` — sparkle effect loop

## NPC-Dropped Ammo

**File**: `ammospawned_givetoplr.fpi` (auto-give)

```fpi
:state=0:state=1
:state=1,cantake:playertake,coloff,plrsound=...
```

**File**: `ammospawned_plrtake.fpi` (press Enter to take)

```fpi
:state=0:hudreset,hudx=50,hudy=90,hudsize=36,hudtext=Press Enter To Pickup Ammo,hudname=press,hudmake=display,state=10,linktoplr
:state=10,plrdistwithin=40,cantake:state=3
:state=3,plrusingaction=1:playertake,coloff,plrsound=...,hudfadeout=press
```

## Start-With-Weapon

**File**: `start2weapons.fpi`

```fpi
:state=0:playertake,coloff,hideshadow=1
```

Place this on a weapon entity set to "Spawn At Start = Yes" to give the player a weapon when the level begins.

## Pickup Pattern Summary

| Pattern | Conditions | Actions | Notes |
|---------|------------|---------|-------|
| Basic item | `plrdistwithin=40` | `playertake,coloff` | Simple pickup |
| Weapon | `plrdistwithin=40` | `playertake,coloff,hideshadow=1,rundecal=-1` | Stops decal |
| Ammo | `plrdistwithin=40,cantake` | `playertake,coloff` | Checks capacity |
| Health | `cantake,plrdistwithin=80` | `playertake,plraddhealth=50` | No HUD needed |
| Health station | `plrusingaction=1` | `plraddhealth=2` | Reusable, per-use |
| Key | `plrdistwithin=40` | `activateifused=1,playertake` | Also activates linked door |
| Quest item | `plrdistwithin=40` | `playertake,setvar=collecteditemA 1` | Sets global flag |
| Droppable | `scancodekeypressed=35` | `playerdrop` | H key to drop |
| Start weapon | (none) | `playertake` | Level start auto-give |
