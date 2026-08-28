# Melee Enemy Patterns

Melee enemies chase the player and attack at close range using animation frames to time damage. This covers the common patterns found across bond1 creatures, zombies, and other melee NPCs.

## Core Melee Flow

```
Patrol (waypoints / idle)
  → Detect player (distance + LOS, or shot)
  → Intro animation (optional — roar, reveal, etc.)
  → Choose attack type (random)
  → Chase / close distance
  → Attack animation (damage at specific frame %)
  → Recovery / back to attack choice
```

## Detection

Most melee enemies detect the player within 200-300 units with line-of-sight:

```fpi
; Legacy — player only
:state=0,plrdistwithin=300,plrcanbeseen:state=9,...
:state=0,shotdamage=1:state=9,...

; DarkAI — any enemy team
:state=1,aitargetdistwithin=300,aicanshoot=1:state=9,aistop
:state=1,shotdamage=1:state=9,aistop,aisettarget
```

## Intro Animation

A common pattern is playing an intro/reveal animation before chasing:

```fpi
:state=9:setframe=22,sound=audiobank\monster\intro.wav,state=10
:state=10:incframe=22,rotatetoplr
:state=10,frameatend=22:state=5
```

The entity plays a full animation cycle (e.g., roar, stand up) before pursuing.

## Chase / Close Distance

Two approaches:

### Legacy — explicit run/move commands
```fpi
; If target is far, run toward them
:state=1,plrdistfurther=80:runfore=2,animate=5,loopsound=...
; If target is close, stop and attack
:state=1,plrdistwithin=80:stopsound,setframe=22,state=7
```

### DarkAI — automatic movement
```fpi
:state=50:aisettarget,aimovetotarget,loopsound=...
:state=50,aitargetdistwithin=80:stopsound,state=55
:state=55:aisettarget,airotatetotarget
:state=55,aitargetdistwithin=70,aicanshoot=1:state=191,aistop
```

## Attack Animation & Damage Timing

Damage is dealt at a specific percentage through the attack animation using `framebeyond`:

```fpi
; Attack 1 — 20 damage at 80% through frame 22
:state=7,framebeyond=22 80,plrdistwithin=90:plraddhealth=-20,state=8
:state=7:incframe=22,rotatetoplr
:state=7,frameatend=22:state=5
:state=8:incframe=22                    ; Recovery — no damage
:state=8,frameatend=22:state=5

; Attack 2 — 10 damage at 60% through frame 23
:state=2,framebeyond=23 60,plrdistwithin=90:plraddhealth=-10,state=3
:state=2:incframe=23,rotatetoplr
:state=2,frameatend=23:state=5
:state=3:incframe=23
:state=3,frameatend=23:state=5
```

**Pattern**:
- `framebeyond=FRAME PERCENT` — check if frame is past PERCENT% of its duration
- When hit connects: transition to a "recovery" state (same animation, no damage)
- The recovery state finishes the animation, then returns to attack choice
- This prevents double-damage from the same swing

## DarkAI Melee State Machine

When using DarkAI, melee uses states 190-294:

```fpi
; Route to attack 1 or 2
:state=190,varequal=0:state=191
:state=190,varequal=1:state=291
:state=190,varequal=911:state=191

; Attack 1 (default)
:state=191:aisettarget,airotatetotarget
:state=191,aitargetdistwithin=70,aicanshoot=1:state=192,aistop
:state=191,aitargetdistfurther=70:state=50        ; Too far — chase again
:state=191,aicanshoot=0:state=1                    ; Lost target — patrol
:state=192:setaiactive=0,state=193,setframe=22     ; Disable movement
:state=193:incframe=22,airotatetotarget
:state=193,framebeyond=22 80,aitargetdistwithin=90:plraddhealth=-20,state=194
:state=193,frameatend=22:animate=1,state=5,setaiactive=1
:state=194,frameatend=22:animate=1,state=5,setaiactive=1

; Attack 2 (escort/ally variant uses state 291-294)
:state=291:aisettarget,airotatetotarget
:state=291,aitargetdistwithin=70,aicanshoot=1:state=292,aistop
:state=291,aitargetdistfurther=70:state=60
:state=291,aicanshoot=0:state=1
:state=292:setaiactive=0,state=293,setframe=23
:state=293:incframe=23,airotatetotarget
:state=293,framebeyond=23 60,aitargetdistwithin=90:plraddhealth=-10,state=294
:state=293,frameatend=23:animate=1,state=5,setaiactive=1
:state=294,frameatend=23:animate=1,state=5,setaiactive=1
```

**Key DarkAI melee rules**:
- `setaiactive=0` during attack — prevents movement during swing
- `setaiactive=1` on recovery — re-enables AI
- `aisettarget` + `airotatetotarget` every frame during windup
- `aitargetdistfurther=70` → go back to chase if target moved away
- `aicanshoot=0` → return to patrol if target lost

## Multiple Attacks Pattern

Choose randomly between attacks:

```fpi
; Legacy
:state=5,random=2:state=1     ; 50% attack 1
:state=5:state=6              ; 50% attack 2 (falls through)

; DarkAI — branch from state 5
:state=5,random=2:state=50    ; Attack 1 chase
:state=5:state=60             ; Attack 2 chase
```

## Passive / Reactive Enemies

Some melee enemies don't actively hunt — they react only when provoked:

```fpi
; Passive — feeding/idle until shot or approached
:state=0:animate=25                            ; Feed animation
:state=0,shotdamage=1:state=9,...              ; React to being shot
:state=0,plrdistwithin=100:state=9,...         ; React to close player
```

## Chase Sound Loop

Melee enemies often play a looping sound while chasing:

```fpi
:state=1,plrdistfurther=80:runfore=2,loopsound=audiobank\monster\chase_loop.wav
:state=1,plrdistwithin=80:stopsound,...        ; Stop loop when in range
```

For DarkAI:
```fpi
:state=50:aisettarget,aimovetotarget,loopsound=audiobank\monster\chase_loop.wav
:state=50,aitargetdistwithin=80:stopsound,state=55
```

## Animation Frame Conventions

There is no universal standard, but common conventions seen across packs:

| Frame | Common Use |
|-------|------------|
| 1 | Idle |
| 2 | Walk |
| 5 | Run |
| 6 | Reload / special |
| 8 | Melee swing (DarkAI standard) |
| 22-25 | Bond1 creatures (intro, attack1, attack2, death, feed) |
| 31 | Crouch idle |
| 32 | Crouch walk |
| 36 | Crouch reload |
| 92-96 | AxeBrute / larger creatures |
| 93-95 | Lobotomy attacks |

Always check the entity's animation set in the model viewer before assigning frames.

## Sound Slots for Melee Enemies

Common sound parameter convention:

| Slot | Use |
|------|-----|
| `$0` | Intro / aggro sound |
| `$1` | Attack hit sound |
| `$2` | Death sound |
| `$3` | Chase loop sound |

Hardcoded paths (as seen in bond1 scripts) are also common when the sounds are pack-specific.

## Complete Template: Melee Enemy (Legacy)

```fpi
desc          = Melee Enemy Template

; Waypoint patrol
:waypointstate=0:animate=2,waypointstart
:waypointstate=3:animate=2,waypointnext
:waypointstate=4:animate=2,waypointrandom
:waypointstate=5:animate=2,waypointreverse

; Detection
:state=0,plrdistwithin=250,plrcanbeseen:waypointstop,state=9,rotatetoplr,setframe=22,sound=$0
:state=0,shotdamage=1:waypointstop,state=9,rotatetoplr,setframe=22,sound=$0

; Intro
:state=9:waypointstop,incframe=22
:state=9,frameatend=22:state=5

; Attack choice
:state=5,random=2:state=1
:state=5:state=6

; Attack 1 approach
:state=1,plrdistfurther=80:waypointstop,rotatetoplr,runfore=2,animate=5,loopsound=$3
:state=1,plrdistwithin=80:stopsound,waypointstop,rotatetoplr,setframe=22,state=7,sound=$1

; Attack 1 hit
:state=7,framebeyond=22 80,plrdistwithin=90:plraddhealth=-20,state=8
:state=7:incframe=22,rotatetoplr
:state=7,frameatend=22:state=5
:state=8:incframe=22
:state=8,frameatend=22:state=5

; Attack 2 approach
:state=6,plrdistfurther=80:waypointstop,rotatetoplr,runfore=2,animate=5,loopsound=$3
:state=6,plrdistwithin=80:stopsound,waypointstop,rotatetoplr,setframe=23,state=2,sound=$2

; Attack 2 hit
:state=2,framebeyond=23 60,plrdistwithin=90:plraddhealth=-10,state=3
:state=2:incframe=23,rotatetoplr
:state=2,frameatend=23:state=5
:state=3:incframe=23
:state=3,frameatend=23:state=5
```

## Complete Template: Melee Enemy (DarkAI)

```fpi
desc          = Melee Enemy — DarkAI

; Animation map
:ducking=0,strafingleft=1:animationnormal,animate=2
:ducking=0,strafingright=1:animationnormal,animate=2
:ducking=0,movingforwards=1:animationnormal,animate=2
:ducking=0,runningforwards=1:animationnormal,animate=5
:ducking=0,movingbackwards=1:animationreverse,animate=2
:ducking=0,idle=1:animate=1
:ducking=1,movingforwards=1:animate=2
:ducking=1,idle=1:animate=1

; Init
:always:localvar=1
:state=0:setvar=0,setaiactive=1,alwaysactive=1,reloadweapon,state=1

; Waypoint patrol
:waypointstate=0:animate=2,waypointstart
:waypointstate=3:animate=2,waypointnext
:waypointstate=4:animate=2,waypointrandom
:waypointstate=5:animate=2,waypointreverse

; Target acquisition
:nearactivatable=0:settarget,activatetarget=2

; Detect target
:state=1,aitargetdistwithin=250,aicanshoot=1:state=9,aistop
:state=1,shotdamage=1:state=9,aistop,aisettarget

; Intro
:state=9:aistop,setframe=22,sound=$0,state=10
:state=10:incframe=22,airotatetotarget
:state=10,frameatend=22:state=5

; Attack choice
:state=5,random=2:state=50
:state=5:state=60

; Chase
:state=50:aisettarget,aimovetotarget,loopsound=$3
:state=50,aitargetdistwithin=80:stopsound,state=55
:state=55:aisettarget,airotatetotarget
:state=55,aitargetdistwithin=70,aicanshoot=1:state=191,aistop
:state=55,aitargetdistfurther=80:state=50
:state=55,aicanshoot=0:state=1

:state=60:aisettarget,aimovetotarget,loopsound=$3
:state=60,aitargetdistwithin=80:stopsound,state=65
:state=65:aisettarget,airotatetotarget
:state=65,aitargetdistwithin=70,aicanshoot=1:state=291,aistop
:state=65,aitargetdistfurther=80:state=60
:state=65,aicanshoot=0:state=1

; Melee attack 1
:state=191:aisettarget,airotatetotarget
:state=191,aitargetdistwithin=70,aicanshoot=1:state=192,aistop
:state=191,aitargetdistfurther=70:state=50
:state=191,aicanshoot=0:state=1
:state=192:setaiactive=0,state=193,setframe=22
:state=193:incframe=22,airotatetotarget
:state=193,framebeyond=22 80,aitargetdistwithin=90:plraddhealth=-20,state=194,sound=$1
:state=193,frameatend=22:animate=1,state=5,setaiactive=1
:state=194,frameatend=22:animate=1,state=5,setaiactive=1

; Melee attack 2
:state=291:aisettarget,airotatetotarget
:state=291,aitargetdistwithin=70,aicanshoot=1:state=292,aistop
:state=291,aitargetdistfurther=70:state=60
:state=291,aicanshoot=0:state=1
:state=292:setaiactive=0,state=293,setframe=23
:state=293:incframe=23,airotatetotarget
:state=293,framebeyond=23 60,aitargetdistwithin=90:plraddhealth=-10,state=294,sound=$2
:state=293,frameatend=23:animate=1,state=5,setaiactive=1
:state=294,frameatend=23:animate=1,state=5,setaiactive=1

; Enemy team — idle random movement
:state=1,aiteam=2,aiaction=0,aicanshot=0,random=30:aimoverandom
```

## Zombie-Specific Patterns

Zombies in `zombie_apocalypse/` and `zombie_apocalypse2/` follow the same melee patterns with additional variations:

| Pattern | Scripts |
|---------|---------|
| Standard walker | `walkingzombie.fpi`, `zombie1.fpi`-`zombie4.fpi` |
| Runner | `runningzombie1.fpi`-`runningzombie3.fpi` |
| Crawler | `zombiecrawler.fpi`, `zombiedragger.fpi` |
| Horde/mob | `mobzombie1.fpi`-`mobzombie8.fpi` |
| Wall climber | `wallzombie.fpi` |
| DarkAI zombie | `zombie1darkai.fpi` |
| Shroudling | `shroudlingmelee.fpi` |
