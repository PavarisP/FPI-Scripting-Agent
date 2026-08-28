# Light & Effect Patterns

## Light Toggle (On by Default)

**File**: `light1.fpi`

```fpi
:state=0:lightintensity=200,state=1
:state=1,activated=0:state=2,lighton
:state=2,activated=1:state=1,lightoff
```

- State 0: sets initial intensity
- Activated=0 (from external switch) → turn light ON
- Activated=1 → turn light OFF

## Light Toggle (Off by Default)

**File**: `lightoff.fpi`

```fpi
:state=0:state=2
:state=1,activated=1:state=2,lighton
:state=2,activated=0:state=1,lightoff
```

Light starts OFF. Activated=1 turns it ON.

## Light Toggle (Reverse Logic)

**File**: `lightoffreverse.fpi`

```fpi
:state=0:state=2
:state=1,activated=0:state=2,lighton
:state=2,activated=1:state=1,lightoff
```

Reverse behavior: activated=0 = light ON, activated=1 = light OFF.

## Light Flicker

**File**: `light2.fpi`

```fpi
:state=0:lightintensity=200,state=1
:state=1,activated=1:state=2
:state=1,random=10:state=2
:state=1:lighton,state=0
:state=2:lightoff,state=0
```

- Normal state: light ON, 10% chance per frame to flicker OFF
- Next frame: light back ON
- `activated=1` externally forces it OFF

## Candle Flicker (Smooth)

**File**: `lightcandle.fpi`

```fpi
:state=0:localvar=0,lighton
:varequal=0,timergreater=50:lightintensity=85,timerstart,incvar=1
:varequal=1,timergreater=50:lightintensity=80,timerstart,incvar=1
:varequal=2,timergreater=50:lightintensity=75,timerstart,incvar=1
; ... cycles through intensities: 85, 80, 75, 70, 85, 80, 75, 80, 75, 80, 75, 85, 80, 75, 80, 90, 85, 80, 75, 70, 90, reset
```

Uses a local variable cycling through 20 steps, changing intensity every 50ms for natural-looking candle flicker.

## Super Gravity Light

**File**: `lightsupergravity.fpi`

```fpi
; When player within 150 units of heavy gravity field, reduce jump height
:plrdistwithin=150:newjumpheight=100
:plrdistfurther=150:newjumpheight=50

; Fade light in and out using LIGHTINTENSITY
:state=0:localvar=0,incvar=1,lighton
:varequal=0:lightintensity=0
:varequal=1:lightintensity=10
; ... steps up 0→100 in increments of 10 ...
:varequal=20:lightintensity=0,setvar=0
```

Combines player gravity modification with pulsing light animation.

## Flame Effect

**File**: `flame.fpi`

```fpi
:state=0:rundecal=7,lighton
:state=0,random=2:lightintensity=99
:state=0,random=3:lightintensity=93
:state=0,random=4:lightintensity=85
```

Decal mode 7 = flame effect. Random light intensity flicker for realistic fire.

## Emission (Toggle Decal)

**File**: `emission.fpi`

```fpi
:state=0:state=1
:state=1,activated=0:state=2,rundecal=2
:state=2,activated=1:state=1,rundecal=-1
```

Toggles a looping decal effect on/off via activation. Decal mode 2 = loop face player.

## Decal Runner

**File**: `decal.fpi`

```fpi
:state=0:rundecal=2
```

Starts decal mode 2 (loop face player). Used as a sub-script or standalone.

## Limb Light

**File**: `limblight.fpi`

```fpi
:always:limblight=2
:state=0:movelightred=225,movelightgreen=225,movelightblue=255,movelightrange=10,movelightoff
:state=0:state=1
:state=1:animate=1
```

Attaches a colored dynamic light to the entity's limbs. Always-on update via `:always`.

## Dynamic Light Colors

| Action | Range |
|--------|-------|
| `movelightred=N` | 0-255 |
| `movelightgreen=N` | 0-255 |
| `movelightblue=N` | 0-255 |
| `movelightrange=N` | Units of light reach |

## Spin & Float Effects

```fpi
:state=10:rundecal=5,spinrate=4,floatrate=10
```

- `spinrate=N` — entity spins at N speed
- `floatrate=N` — entity bobs up and down
- Used on glowing pickups for visual attraction

## Light Pattern Summary

| Script | Behavior |
|--------|----------|
| `light1.fpi` | On by default, toggle via activation |
| `lightoff.fpi` | Off by default, toggle via activation |
| `lightoffreverse.fpi` | Off by default, reverse toggle |
| `light2.fpi` | Flicker effect |
| `lightcandle.fpi` | Smooth candle flicker (20-step cycle) |
| `lightsupergravity.fpi` | Pulsing light + gravity field |
| `flame.fpi` | Fire + random flicker |
| `emission.fpi` | Decal on/off toggle |
| `limblight.fpi` | Colored limb-attached dynamic light |
| `lightmove.fpi` | Animated moving light (uses animate=1) |
