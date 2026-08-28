# Conditions Reference

Conditions go **before** the colon in a rule. All must be true for the rule to fire.

## State & Activation

| Condition | Description |
|-----------|-------------|
| `state=N` | Entity is in state N |
| `activated=0` | Entity has been deactivated (via `activate=0`) |
| `activated=1` | Entity has been activated (via `activate=1`) |
| `activated=2` | Entity has received activation value 2 |
| `activated=3` | Entity has received activation value 3 |

## Player Proximity & Position

| Condition | Description |
|-----------|-------------|
| `plrdistwithin=N` | Player is within N units of the entity |
| `plrdistfurther=N` | Player is further than N units from the entity |
| `plrhigher=N` | Player is at least N units above the entity |
| `plrwithinzone=1` | Player is inside the trigger zone volume |
| `plrwithinzone=0` | Player is outside the trigger zone volume |

## Player State

| Condition | Description |
|-----------|-------------|
| `plrusingaction=1` | Player pressed the use/action key (Enter) |
| `plrusingaction=0` | Player released the use/action key |
| `plrcanbeseen` | Player is in the entity's line of sight |
| `plrcannotbeseen` | Player is NOT in the entity's line of sight |
| `plrfacing=N` | Player is facing within N degrees of the entity |
| `plrhaskey=1` | Player has a key item (picked up via pickupkey.fpi) |
| `plralive=0` | Player is dead |
| `plrweaponidle` | Player's current weapon is in idle state |
| `plrcurrentweapon=path` | Player has a specific weapon equipped (e.g., `modernday\law`) |

## General Entity Proximity

| Condition | Description |
|-----------|-------------|
| `anywithin=N` | Any entity (NPC/enemy) is within N units |
| `anyfurther=N` | All entities are further than N units |
| `anywithinzone=1` | Any entity is inside the zone volume |
| `anywithinzone=0` | No entities inside the zone volume |
| `anykeywithinzone=1` | A "key" type entity is inside the zone |

## Animation

| Condition | Description |
|-----------|-------------|
| `frameatend=0` | Animation has reached its last frame |
| `frameatstart=0` | Animation has rewound to its first frame |
| `framebeyond=N M` | Frame N has played beyond M% (0-100) of its duration |
| `alphafadeequal=N` | Entity's alpha fade has reached value N (0-100) |

## Timing & Random

| Condition | Description |
|-----------|-------------|
| `random=N` | 1-in-N chance each frame (e.g., `random=10` = 10% per frame) |
| `timergreater=N` | Entity-local timer exceeded N milliseconds |
| `etimergreater=N` | Global elapsed timer exceeded N milliseconds |
| `soundfinished=1` | The currently playing sound on this entity has finished |

## Key Presses

| Condition | Description |
|-----------|-------------|
| `scancodekeypressed=N` | Specific key pressed (scan code). Common values: 33=F, 34=G, 35=H, 72=NumPad8, 80=NumPad2, 203=Left, 205=Right |
| `keypressed=N 1` | Key N is currently held down |
| `keypressed=N 0` | Key N was released |

## Player-Item Interaction

| Condition | Description |
|-----------|-------------|
| `cantake` | Player has room to pick up the item (not full on ammo/weapon) |

## Weapon & Combat

| Condition | Description |
|-----------|-------------|
| `ifweapon=1` | Entity has ammo (weapon loaded) |
| `ifweapon=0` | Entity is out of ammo |
| `rateoffire` | Enough time has passed since last shot (fire rate cooldown) |
| `shotdamage=N` | Entity took at least N damage this frame from a shot |
| `hasweapon=1` | Entity is carrying a weapon |

## AI-Specific (Legacy)

| Condition | Description |
|-----------|-------------|
| `noiseheard=N` | Entity heard a noise within N units |
| `nearactivatable=0` | No activatable target nearby |
| `losetarget=N` | Entity lost sight of target N frames ago |
| `plringunsight` | Player is in the entity's weapon sights |
| `plrelevfurther=N` | Player is more than N units vertically from entity |

## AI-Specific (DarkAI)

| Condition | Description |
|-----------|-------------|
| `aicanshoot=1` | AI can see and has line-of-sight to target |
| `aicanshoot=0` | AI cannot see target |
| `aitargetdistwithin=N` | AI target is within N units |
| `aitargetdistfurther=N` | AI target is further than N units |
| `aiatcover=0` | AI is not at a cover point |
| `aiatcover=1` | AI has reached a cover point |
| `aiheardsound=N` | AI heard a sound within N units |
| `aiaction=0` | AI has no current action |
| `aicalled=N` | AI received a team call signal within N units |
| `ducking=1` | AI is in crouching state |
| `ducking=0` | AI is standing |
| `idle=1` | AI is not moving (standing still) |
| `idle=0` | AI is moving |
| `strafingleft=1` | AI is strafing left |
| `strafingright=1` | AI is strafing right |
| `movingforwards=1` | AI is walking forward |
| `runningforwards=1` | AI is running forward |
| `movingbackwards=1` | AI is walking backward |

## Variables

| Condition | Description |
|-----------|-------------|
| `varequal=name N` | Global variable `name` equals N |
| `varnotequal=name N` | Global variable `name` does NOT equal N |
| `vargreater=name N` | Global variable `name` is greater than N |
| `varless=name N` | Global variable `name` is less than N |

## Local Variables

| Condition | Description |
|-----------|-------------|
| `localvar=N` | Access local variable slot N (1-4). Used with `setvar`, `addvar`, etc. |
| `varequal=N` (with localvar) | Check if current local var equals N |
| `varnotequal=N` | Check if current local var does NOT equal N |

## Player Association (Lifts)

| Condition | Description |
|-----------|-------------|
| `playerassociated` | Player is currently riding/associated with this entity (via `associateplayer`) |

## Physics

| Condition | Description |
|-----------|-------------|
| `pickobject=1` | Player crosshair is pointing at the entity (for switches) |
