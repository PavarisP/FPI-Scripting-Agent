# DarkAI Conversion Guide

How to convert legacy AI scripts to DarkAI. This guide uses the bond1 dogman as a real-world example.

## Why Convert to DarkAI?

| Feature | Legacy AI | DarkAI |
|---------|-----------|--------|
| Target detection | `plrdistwithin` + `plrcanbeseen` (player only) | `aitargetdistwithin` + `aicanshoot` (any team) |
| Targeting | `settarget`, `rotatetoplr` | `aisettarget`, `airotatetotarget` |
| Movement | `runfore=N`, `movefore=N`, `followplr=N` | `aimovetotarget`, `aimovetocover=N` |
| Animation | Manual `animate=N` per state | Auto-detects `idle`, `runningforwards`, `ducking` etc. |
| Teams | None | `aiteam=1` (ally), `aiteam=2` (enemy) |
| Cover system | Manual | `aimovetocover`, `aiatcover` |
| Sound response | `noiseheard=N` | `aiheardsound=N`, `aimovetosound` |
| Ally commands | None | G to recruit, H to dismiss |

## Step-by-Step: Dogman Example

### Step 1: Add the Animation Map

DarkAI needs to know which animations to play for each movement state. Replace manual `animate=N` calls with this map:

```fpi
:ducking=0,strafingleft=1:animationnormal,animate=2
:ducking=0,strafingright=1:animationnormal,animate=2
:ducking=0,movingforwards=1:animationnormal,animate=2
:ducking=0,runningforwards=1:animationnormal,animate=5
:ducking=0,movingbackwards=1:animationreverse,animate=2
:ducking=0,idle=1:animate=25

;If no crouch anims, fall back to standing:
:ducking=1,movingforwards=1:animate=2
:ducking=1,idle=1:animate=25
```

### Step 2: Replace Commands

| Legacy | DarkAI |
|--------|--------|
| `settarget` | `nearactivatable=0:settarget,activatetarget=2` (always-active) |
| `rotatetoplr` | `airotatetotarget` |
| `runfore=N` / `movefore=N` | `aimovetotarget` |
| `shootplr` | `useweapon` + `rundecal=6` (for ranged) |
| `followplr=N` | `aifollowplr=N` |

### Step 3: Replace Detection

```fpi
; Legacy — player only
:state=0,plrdistwithin=300,plrcanbeseen:...

; DarkAI — detects any enemy team
:state=1,aitargetdistwithin=300,aicanshoot=1:state=9,aistop
```

### Step 4: Add Init + Team Assignment

```fpi
:always:localvar=1
:state=0:setvar=0,setaiactive=1,alwaysactive=1,reloadweapon,state=1

; Enemy team — idle random movement
:state=1,aiteam=2,aiaction=0,aicanshot=0,random=30:aimoverandom
```

### Step 5: Convert Melee to DarkAI State Machine

**Original**:
```fpi
:state=1,plrdistfurther=80:waypointstop,rotatetoplr,runfore=2,loopsound=...
:state=1,plrdistwithin=80:stopsound,waypointstop,rotatetoplr,setframe=22,state=7
:state=7,framebeyond=22 80,plrdistwithin=90:plraddhealth=-20,state=8
:state=7:incframe=22,rotatetoplr
:state=7,frameatend=22:state=5
```

**DarkAI**:
```fpi
:state=50:aisettarget,aimovetotarget,loopsound=...
:state=50,aitargetdistwithin=80:stopsound,state=55
:state=55:aisettarget,airotatetotarget
:state=55,aitargetdistwithin=70,aicanshoot=1:state=191,aistop
:state=55,aitargetdistfurther=80:state=50

:state=191:aisettarget,airotatetotarget
:state=191,aitargetdistwithin=70,aicanshoot=1:state=192,aistop
:state=192:setaiactive=0,state=193,setframe=22
:state=193:incframe=22,airotatetotarget
:state=193,framebeyond=22 80,aitargetdistwithin=90:plraddhealth=-20,state=194
:state=193,frameatend=22:animate=25,state=5,setaiactive=1
:state=194,frameatend=22:animate=25,state=5,setaiactive=1
```

### Step 6: Setup Appear Script

The DarkAI appear scripts add the entity to a team:

**Enemy** — `Dark AI/appear-enemy-team2.fpi`:
```fpi
:state=0:state=1,setalphafade=0,animate=1
:state=1,alphafadeequal=0:incalphafade=100
:state=1,alphafadeequal=100:state=2,addaiteam=2,aiaddenemy=1 2,runfpidefault=1
```

**Ally** — `Dark AI/appear-ally-team1.fpi`:
```fpi
:state=0:state=1,setalphafade=0,animate=1
:state=1,alphafadeequal=0:incalphafade=100
:state=1,alphafadeequal=100:state=2,addaiteam=1,runfpidefault=1
```

## Common Pitfalls

1. **DarkAI needs `aisettarget` before `airotatetotarget`/`aimovetotarget`** — always call `aisettarget` first
2. **`nearactivatable=0:settarget,activatetarget=2`** must be always-active (no state condition) to continuously acquire targets
3. **Animation map must cover all states** — `ducking=1` states too, even if the creature can't crouch (fall back to standing)
4. **`aicanshoot=1` requires LOS** — make sure the target isn't behind geometry
5. **`reloadweapon` in init** — harmless for melee creatures, required for DarkAI init
6. **`setaiactive=1/0`** — disable during attack animations so the creature doesn't move mid-swing

## Quick Reference: Command Mapping

| Purpose | Legacy | DarkAI |
|---------|--------|--------|
| Acquire target | `settarget` | `nearactivatable=0:settarget,activatetarget=2` |
| Face target | `rotatetoplr` | `airotatetotarget` |
| Move to target | `runfore=N`, `movefore=N` | `aimovetotarget` |
| Move to cover | (manual) | `aimovetocover=N` |
| Stop moving | `freeze` | `aistop` |
| Stop AI | (manual) | `setaiactive=0` |
| Check target distance | `plrdistwithin=N` | `aitargetdistwithin=N` |
| Check target visible | `plrcanbeseen` | `aicanshoot=1` |
| Check idle | (manual) | `idle=1` |
| Check crouching | (manual) | `ducking=1` |
| Respond to sound | `noiseheard=N` | `aiheardsound=N` |
| Move to sound | (manual) | `aimovetosound` |
| Call for backup | (manual) | `aicallteam=N` |
| Add to team | (manual) | `addaiteam=N` |
