# DarkAI System

**Requires FPSC v1.17+**. A more sophisticated AI system with team-based combat, cover seeking, melee, ammo management, and ally commands.

## Main DarkAI Script

**File**: `Dark AI/main-weapon.fpi` (194 lines — the most complex script in the scriptbank)

### Initialization

```fpi
:always:localvar=1
:state=0:aiusefullaim=1,setvar=0
```

Sets up full aiming and default behavior mode (0=default, 1=escort/ally, 911=responder).

### Animation System

```fpi
; Walking
:ducking=0,strafingleft=1:animationnormal,animate=3
:ducking=0,strafingright=1:animationnormal,animate=4
:ducking=0,movingforwards=1:animationnormal,animate=2
:ducking=0,runningforwards=1:animationnormal,animate=5
:ducking=0,movingbackwards=1:animationreverse,animate=2
:ducking=0,idle=1:animate=1

; Crouching
:ducking=1,movingforwards=1:animate=32
:ducking=1,idle=1:animate=31
```

DarkAI detects movement state automatically and selects the correct animation. This is the only place where `animationreverse` is used to play walk animation backwards.

### Combat State Machine

**State 1 — Ready (default active state)**:
```fpi
:state=1,ifweapon=1,aicanshoot=1,aitargetdistfurther=70,idle=1,random=1:aisettarget,useweapon,rundecal=6
:state=1,ifweapon=1,aicanshoot=1,aitargetdistfurther=70,idle=0,rateoffire:aisettarget,useweapon,rundecal=6
```
Shoots at target when in range. Random 1-in-4 fire when idle, rate-limited when moving.

**Closing in (state 54→55)**:
```fpi
:state=1,varequal=0,aicanshoot=1,aitargetdistfurther=70,aitargetdistwithin=250:state=54,aistop
:state=1,varequal=1,aicanshoot=1,aitargetdistfurther=70,aitargetdistwithin=150:state=54,aistop

:state=54,idle=1:animate=1,state=55
:state=55:aisettarget,aimovetotarget
:state=55,aitargetdistwithin=60:state=191
```
Moves toward target. Default behavior closes at 250 units, escort at 150, responder at 100.

**Close range (state 2)**:
```fpi
:state=2,ifweapon=1,aicanshoot=1,random=3:aifollowplr=0,state=60,aistop,animate=1
:state=2,ifweapon=0,random=5:aifollowplr=0,state=60,aistop,animate=1
:state=2,ifweapon=1,random=1:airotatetotarget,state=190
:state=2,ifweapon=0:airotatetotarget,state=190
:state=2,aitargetdistfurther=100:state=1
```
If enemy within 70 units: 1-in-4 chance to fall back, 1-in-1 chance to melee.

### Responding to Sounds (state 30)

```fpi
:state=30:aisettarget,airotatetotarget
:state=30,varequal=1,aiheardsound=3000:airotatetosound
:state=30,varequal=0,aiheardsound=3000:aimovetosound
:state=30,healthless=50,aicanshoot=1:aistop,state=1,aicallteam=2000
:state=30,aicanshoot=1:aisettarget,useweapon,rundecal=6,state=1
:state=30,etimergreater=4000,idle=1:state=1
```
Escort (var=1) only rotates toward sound. Default (var=0) moves to investigate. Calls for backup if injured.

### Fall Back to Cover (state 60→67)

```fpi
:state=60:state=67,etimerstart
:state=67:airotatetotarget
:state=67,aiatcover=0:aimovetocover=0,aisettarget,airotatetotarget
:state=67,ifweapon=1,aicanshoot=1,rateoffire:aisettarget,useweapon,rundecal=6
:state=67,etimergreater=1000,aitargetdistwithin=70,aicanshoot=1,ratoffire:aistop,state=190
:state=67,etimergreater=1000,aicanshoot=0:state=1
:state=67,etimergreater=1000,ifweapon=0:freeze,setaiactive=0,state=25
```
Moves to nearest cover, returns fire while retreating, melees if cornered, reloads if out of ammo.

### Reloading (states 25-28 standing, 45-48 crouched)

```fpi
:state=25:freeze,setframe=6,state=26
:state=26:freeze,incframe=6
:state=26,framebeyond=6 60:reloadweapon
:state=26,frameatend=6:sound=...,state=27
:state=27:state=1,setaiactive=1
```

### Melee Attack (states 190-294)

```fpi
:state=191:aisettarget,airotatetotarget
:state=191,aitargetdistwithin=70,aicanshoot=1:state=192,aistop
:state=192:setaiactive=0,state=193,setframe=8
:state=193:incframe=8,airotatetotarget
:state=193,framebeyond=8 60,aitargetdistwithin=70:aisetmeleedamage=10,aiusemelee=1,state=194,sound=...
:state=194,frameatend=8:animate=1,state=1,setaiactive=1
```
Plays melee animation (frame 8), applies damage at 60% through the animation, then returns to combat.

### Ally / Escort System

```fpi
; Recruit — Press G (scan code 34)
:aiteam=1,varequal=0,plrdistwithin=70,plrfacing=10,etimergreater=200,scancodekeypressed=34 1:etimerstart,setvar=1,sound=...,aifollowplr=1

; Dismiss — Press H (scan code 35)
:aiteam=1,varequal=1,plrdistwithin=70,plrfacing=10,etimergreater=200,scancodekeypressed=35 1:etimerstart,setvar=0,sound=...,aifollowplr=0

; Auto-follow if too far
:aiteam=1,varequal=1,plrdistfurther=200:aifollowplr=1
```

- G key = recruit ally
- H key = dismiss ally
- Ally follows player when in escort mode
- Shows on-screen text prompts

### Responder System

```fpi
:varequal=0,aiaction=0,aicalled=2000:airespondtocall,setvar=911
:varequal=911,aicanshoot=1:setvar=0
:varequal=911,idle=1:setvar=0
:varequal=911,shotdamage=1:setvar=0
```

Free NPCs respond to team calls, switch to responder mode (911), then return to default when engaged or idle.

## DarkAI Appear Scripts

**Enemy spawn** — `Dark AI/appear-enemy-team2.fpi`:
```fpi
:state=0:state=1,setalphafade=0,animate=1
:state=1,alphafadeequal=0:incalphafade=100
:state=1,alphafadeequal=100:state=2,addaiteam=2,aiaddenemy=1 2,runfpidefault=1
```
Fades in, adds to team 2 (enemy), adds 1 enemy of type 2 to the level, then runs the default AI.

**Ally spawn** — `Dark AI/appear-ally-team1.fpi`:
```fpi
:state=0:state=1,setalphafade=0,animate=1
:state=1,alphafadeequal=0:incalphafade=100
:state=1,alphafadeequal=100:state=2,addaiteam=1,runfpidefault=1
```
Fades in, adds to team 1 (ally), runs default AI.

## DarkAI Commands Reference

| Command | Description |
|---------|-------------|
| `aisettarget` | Set nearest enemy as target |
| `airotatetotarget` | Rotate toward target |
| `airotatetosound` | Rotate toward sound source |
| `aimovetotarget` | Move toward target |
| `aimovetocover=N` | Move to nearest cover |
| `aimoverandom` | Move to random position |
| `aistop` | Stop movement |
| `aifollowplr=N` | Follow player (1=on, 0=off) |
| `setaiactive=N` | Enable/disable AI |
| `aicallteam=N` | Call for backup within N units |
| `airespondtocall` | Respond to team call |
| `aiusemelee=N` | Execute melee |
| `aisetmeleedamage=N` | Set melee damage |
| `aiusefullaim=1` | Enable full aiming |
| `addaiteam=N` | Add to team N |
| `aiaddenemy=N M` | Add N enemies of type M |

## DarkAI Conditions Reference

| Condition | Description |
|-----------|-------------|
| `aicanshoot=1/0` | Can/cannot see target |
| `aitargetdistwithin=N` | Target within N units |
| `aitargetdistfurther=N` | Target beyond N units |
| `aiatcover=1/0` | At/not at cover |
| `aiheardsound=N` | Heard sound within N |
| `aiaction=0` | No current action |
| `aicalled=N` | Received team call |
| `ducking=1/0` | Crouching/standing |
| `idle=1/0` | Standing still/moving |
| `healthless=N` | Health below N |

## DarkAI Variants

| Script | Description |
|--------|-------------|
| `main-weapon.fpi` | Full combat: shoot, melee, cover, reload, ally system |
| `main-weapon auto follow.fpi` | Auto-follows player |
| `main-weapon-holdposition.fpi` | Holds position, doesn't chase |
| `main-weapon - Copy.fpi` | Variant copy |
