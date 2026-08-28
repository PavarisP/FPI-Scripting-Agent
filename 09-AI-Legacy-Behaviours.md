# Legacy AI Behaviours (`people/` directory)

The legacy AI system uses a state machine with conditions like `plrcanbeseen`, `plrdistwithin`, `shotdamage`, `random`, and `noiseheard`.

## Chase and Shoot

**File**: `people/chase.fpi`

```fpi
:nearactivatable=0:settarget,activatetarget=2
:losetarget=50:freeze,runfpidefault=1
:plralive=0:freeze,runfpidefault=1

:ifweapon=1,plrcanbeseen=46,plringunsight,rateoffire:settarget,useweapon,rundecal=6,choosestrafe
:ifweapon=0:reloadweapon,freeze,setframe=6,state=4,sound=audiobank\guns\reload.wav
:shotdamage=10:rotatetoplr,state=5

:state=0,plrdistfurther=100:state=1
:state=0,plrdistwithin=101:state=2,animate=1
:state=1,plrelevfurther=10,plrcanbeseen=46,plringunsight:freeze,settarget,animate=1,state=0
:state=1:followplr=1,animate=5
:state=1:state=0

:state=2:rotatetoplr,resethead
:state=2:freeze,state=0

:state=4:incframe=6
:state=4,frameatend=6:state=0

:state=5,random=1:state=6,setframe=4
:state=5:state=7,setframe=3
:state=6:incframe=4,strafe=90,rotatetoplr
:state=6,frameatend=4:state=1,animate=1
:state=7:incframe=3,strafe=-90,rotatetoplr
:state=7,frameatend=3:state=1,animate=1
```

**Behavior**: Patrols (state 0→1), chases when player seen (state 0→2→1), shoots (always-active), strafes when shot (state 5→6/7), reloads when empty.

## Look and Shoot (Static)

**File**: `people/shoot.fpi`

```fpi
:state=0,plrcanbeseen:state=1,settarget
:losetarget=10:runfpidefault=1
:state=0:runfpidefault=1
:state=1:rotatetoplr
:state=1,ifweapon=1,plrcanbeseen=46,plringunsight,rateoffire:useweapon,rundecal=6
:state=1,ifweapon=0:state=2,setframe=6
:state=2:incframe=6
:state=2,frameatend=6:state=1,reloadweapon,sound=audiobank\guns\reload.wav
```

**Behavior**: Stands still, rotates to face player, shoots when aimed, reloads when empty. Returns to default when target lost.

## Aggressive Sentry (Static)

**File**: `people/static.fpi`

```fpi
:state=0:animate=1
:state=0,plrcanbeseen:settarget,rotatetoplr,shootplr
:state=0,random=20:rotateheadrandom=35
:state=0,shotdamage=10:settarget,rotatetotarget,resethead
:state=0,noiseheard=5:rotateheadrandom=85
```

**Behavior**: Idle animation, shoots player directly (`shootplr`), looks toward noise, reacts when shot.

## Follow Waypoints

**File**: `people/follow.fpi`

```fpi
:waypointstate=0:animate=2,waypointstart
:waypointstate=3:animate=2,waypointnext
:waypointstate=4:animate=2,waypointrandom
:waypointstate=5:animate=2,waypointreverse
```

**Behavior**: Moves along placed waypoints. Handles all waypoint states (start, next, random, reverse).

## Duck for Cover and Shoot

**File**: `people/cover.fpi`

```fpi
:state=0,plrdistwithin=100:state=21
:state=0,plrcanbeseen:state=1,settarget
:state=0,random=50:animate=1
:state=0:runfpidefault=1

:state=4:state=5
:state=1:state=4,animate=31

:state=5:rotatetoplr
:state=5,plrdistwithin=100:state=11
:state=5,ifweapon=1,plrcanbeseen=46,plringunsight:useweapon,rundecal=6
:state=5,ifweapon=0:state=6,setframe=36
:state=5,shotdamage=3:state=11
:state=6:incframe=36
:state=6,frameatend=36:state=5,reloadweapon,sound=...
```

**Behavior**: Ducks into cover (animation 31 = crouch), peeks out to shoot, ducks back when shot. Alternates between cover (state 5) and hiding (state 11).

## Snipe

**File**: `people/snipe.fpi`

```fpi
:ifweapon=0:reloadweapon,state=1,animate=31,sound=...
:plrcanbeseen:settarget,rotatetotarget,resethead
:plrdistwithin=200:rotatetoplr,resethead

:state=0,random=50:rotateheadrandom=65
:state=0,shotdamage=1:state=7
:state=0,ifweapon=1,plrcanbeseen:state=2

; Delayed shot sequence: state 2→3→4→5 (useweapon)
:state=5:useweapon,rundecal=6,state=0
:state=4:state=5
:state=3:state=4
:state=2:state=3

; When shot, strafe away
:state=7,random=1:state=8
:state=7:settarget,rotatetotarget,state=1,animate=31
:state=8:state=9,setframe=3
:state=9:incframe=3,strafe=-90,rotatetoplr
:state=9,frameatend=3:state=1,animate=1
```

**Behavior**: Looks around, has a delayed shot sequence (4 frame delay), reloads while crouching, strafes when shot.

## Strafe Shoot

**File**: `people/strafe.fpi`

```fpi
:state=0,ifweapon=0:state=8,setframe=6
:plrcanbeseen:settarget,rotatetotarget,resethead
:plrdistwithin=200:rotatetoplr,resethead

:state=0,random=20:state=9
:state=0,shotdamage=1:rotatetoplr,state=7
:state=0,ifweapon=1,plrcanbeseen:state=2

:state=1:incframe=3,strafe=-90,rotatetoplr
:state=1,frameatend=3:state=0,animate=1

:state=5:useweapon,rundecal=6,state=0
:state=4:state=5
:state=3:state=4
:state=2:state=3

:state=6:incframe=4,strafe=90,rotatetoplr
:state=6,frameatend=4:state=0,animate=1
```

**Behavior**: Sidesteps while shooting, alternates strafe directions, random lateral movement.

## Hunt and Pace Waypoints

**File**: `people/pace.fpi`

```fpi
:waypointstate=0:animate=2,waypointstart
:waypointstate=3:animate=2,waypointnext
:waypointstate=4:animate=2,waypointrandom
:waypointstate=5:animate=2,waypointreverse
:nearactivatable=0:settarget,activatetarget=2,animate=1,state=0

:state=0:state=1
:state=1,plrcanbeseen:settarget,state=2
:state=1,random=20:rotateheadrandom=65
:state=1,random=60:rotatetoplr
:state=1,shotdamage=10:settarget,waypointstop,animate=1,rotatetotarget,resethead,state=4
:state=1,noiseheard=5:rotateheadrandom=85

:state=2:waypointstop,rotatetoplr,state=3,shootplr
:state=3:animate=1,waypointstart,state=0

:state=4,plrcanbeseen=46:settarget,state=2
:state=4,random=20:animate=2,waypointstart,state=1
```

**Behavior**: Patrols waypoints, stops and shoots when player seen, investigates noise, returns to patrol when threat gone.

## Homing In (Melee Enemy)

**File**: `people/home.fpi`

```fpi
:state=0:rotatetoplr
:state=1:rotatetoplr
:plrcanbeseen,plrdistfurther=70:settarget,state=1
:noiseheard=5:rotateheadrandom=85
:random=10:rotateheadrandom=45
:state=1:movetotarget,animate=2
:state=1,plrdistwithin=61:freeze,animate=1,state=0
:plrdistwithin=50,rateoffire:plraddhealth=-5
```

**Behavior**: Moves toward player, melee attacks when close (damages player directly via `plraddhealth=-5`).

## All Legacy AI Behaviours

| Script | Description |
|--------|-------------|
| `chase.fpi` | Chase and shoot, strafe when shot |
| `chase10.fpi` | Chase variant (DarkAI 1.0) |
| `shoot.fpi` | Stand and shoot |
| `shoot10.fpi` | Shoot variant |
| `static.fpi` | Aggressive sentry, shoots on sight |
| `cover.fpi` | Duck behind cover, peek and shoot |
| `cover10.fpi` | Cover variant |
| `snipe.fpi` | Long-range, delayed shot, strafe on hit |
| `snipe10.fpi` | Snipe variant |
| `strafe.fpi` | Sidestep while shooting |
| `strafe10.fpi` | Strafe variant |
| `pace.fpi` | Patrol waypoints, hunt player |
| `pace10.fpi` | Pace variant |
| `follow.fpi` | Follow waypoints (non-combat) |
| `home.fpi` | Melee homing enemy |
| `dead.fpi` | Start with reduced health (500 HP subtraction) |
| `coward.fpi` | Runs away from player |
| `throw.fpi` | Throws projectiles |
| `zombie*.fpi` | Zombie variants (cop, doctor, fat, nurse, surgeon) |
| `thug_flashlight_*.fpi` | Patrol with flashlight |
| `medicheal.fpi` | Heals nearby allies |
| `movefore.fpi` | Moves forward continuously |
| `restless.fpi` | Idle with random head movement |
| `passive.fpi` | Non-aggressive, ignores player |
