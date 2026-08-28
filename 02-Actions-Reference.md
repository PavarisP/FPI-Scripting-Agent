# Actions Reference

Actions go **after** the colon in a rule. They execute left-to-right when the rule fires.

## State Management

| Action | Description |
|--------|-------------|
| `state=N` | Transition to state N |
| `activate=0` | Send deactivation signal |
| `activate=1` | Send activation signal (value 1) |
| `activate=2` | Send activation value 2 |
| `activate=3` | Send activation value 3 |
| `activateifused=1` | Activate the entity named in the "IFUSED" field |
| `activateifused=0` | Deactivate the entity named in the "IFUSED" field |
| `activateallinzone=1` | Activate all entities inside this zone |

## Animation

| Action | Description |
|--------|-------------|
| `animate=N` | Play animation N from the entity's animation set |
| `incframe=N` | Increment animation frame starting at frame N |
| `decframe=N` | Decrement (rewind) animation frame starting at frame N |
| `setframe=N` | Jump immediately to frame N |
| `animationnormal` | Play animation at normal speed/direction |
| `animationreverse` | Play animation in reverse |

## Visibility & Collision

| Action | Description |
|--------|-------------|
| `coloff` | Disable collision (entity becomes pass-through) |
| `colon` | Enable collision |
| `nobulletcol` | Disable bullet collision only (bullets pass through) |
| `hide` | Hide the entity |
| `hideshadow=N` | Hide (1) or show (0) the entity's shadow |
| `setalphafade=N` | Set alpha transparency (0=invisible, 100=fully visible) |
| `incalphafade=N` | Fade in by N units per frame |
| `decalphafade=N` | Fade out by N units per frame |
| `setalphafade=100` | Make fully visible (common in appear scripts) |
| `runfpidefault=1` | Run the entity's default/main AI script |

## Player Interaction

| Action | Description |
|--------|-------------|
| `playertake` | Player picks up the item (adds to inventory) |
| `playerdrop` | Player drops the currently held item |
| `plraddhealth=N` | Add N health to player (negative = damage) |
| `plrsound=path` | Play a sound for the player (not affected by distance) |
| `plrspeedmod=N` | Modify player movement speed |
| `plrmoveifused` | Teleport player to the entity named in IFUSED |
| `plrmoveto=Name` | Teleport player to entity with exact name "Name" |
| `newjumpheight=N` | Set player's jump height (e.g., for gravity fields) |
| `plrsubhealth=N` | Subtract N health from player |
| `associateplayer` | Attach player to this entity (lifts) |
| `unassociateplayer` | Detach player from this entity |

## Sound & Music

| Action | Description |
|--------|-------------|
| `sound=path` | Play a 3D sound at this entity's position |
| `plrsound=path` | Play a 2D sound directly in player's ears |
| `loopsound=path` | Start looping a sound |
| `stopsound` | Stop the current sound |
| `soundscale=N` | Set sound volume scale (0-100) |
| `music=path` | Play music track (pass `0` to stop) |
| `musicoverride=path` | Override the global music with local level music |
| `musicvolume=N` | Set music volume (0-100) |

## HUD

| Action | Description |
|--------|-------------|
| `hudreset` | Reset HUD state for a new HUD element |
| `hudx=N` | Set HUD X position (0-100 = percentage of screen) |
| `hudy=N` | Set HUD Y position (0-100) |
| `hudsize=N` | Set HUD text size |
| `hudsizex=N` | Set HUD width in pixels (legacy) |
| `hudsizey=N` | Set HUD height in pixels (legacy) |
| `hudglobalx` | Use dynamic HD scaling for X (BIMA mod) |
| `hudglobaly` | Use dynamic HD scaling for Y (BIMA mod) |
| `hudfont=name` | Set HUD font (e.g., `verdana`, `arial`) |
| `hudtext=string` | Set HUD text content |
| `hudimage=path` | Set full-screen HUD image |
| `hudimagefine=path` | Set overlay HUD image (with transparency) |
| `hudname=name` | Assign a name to the HUD for later reference |
| `hudtype=N` | Set HUD type (1=text, 3=image) |
| `hudhide=N` | Start hidden (1=hidden) |
| `hudmake=display` | Create an overlay (display) HUD |
| `hudmake=internal` | Create a full-screen internal HUD |
| `hudshow=name` | Show a previously created HUD by name |
| `hudfadeout=name` | Fade out and destroy a HUD by name |
| `changehudalpha=name N` | Change HUD alpha to value N (0-255) |

## Decals & Visual FX

| Action | Description |
|--------|-------------|
| `rundecal=N` | Run a decal/particle effect. Modes: 0=once face player, 1=once keep angle, 2=loop face player, 3=loop keep angle, 4=once face up, 5=loop face up, 6=character gun decal, 7=flame, 8=spawn |
| `rundecal=-1` | Stop the current decal loop |
| `spinrate=N` | Set auto-rotation speed |
| `floatrate=N` | Set floating/bobbing speed |

## Physics & Environment

| Action | Description |
|--------|-------------|
| `nogravity=N` | Set no-gravity mode (1=on) |
| `antigravity=N` | Set antigravity value (N = upward force) |
| `water=1` | Enable water mode |
| `waterfogdist=N` | Set water fog distance |
| `waterred=N` | Water fog red channel (0-255) |
| `watergreen=N` | Water fog green channel (0-255) |
| `waterblue=N` | Water fog blue channel (0-255) |
| `waterheightofzone=N` | Set water surface height |
| `ambience=N` | Set ambient light level |
| `floorlogic` | Enable floor logic (entity stands on floors) |

## Lights

| Action | Description |
|--------|-------------|
| `lighton` | Turn the light on |
| `lightoff` | Turn the light off |
| `lightintensity=N` | Set light intensity (0-255) |
| `movelightred=N` | Set dynamic light red (0-255) |
| `movelightgreen=N` | Set dynamic light green (0-255) |
| `movelightblue=N` | Set dynamic light blue (0-255) |
| `movelightrange=N` | Set dynamic light range |
| `movelightoff` | Turn off dynamic light |
| `limblight=N` | Set limb light mode (2=anim1) |

## AI Actions (Legacy)

| Action | Description |
|--------|-------------|
| `settarget` | Set the nearest enemy as target |
| `rotatetoplr` | Rotate entity to face the player |
| `rotatetotarget` | Rotate entity to face its current target |
| `useweapon` | Fire the entity's weapon at its target |
| `reloadweapon` | Reload the entity's weapon |
| `shootplr` | Shoot at the player directly |
| `followplr=N` | Follow the player (N = follow distance?) |
| `movetotarget` | Move toward the current target |
| `choosestrafe` | Randomly choose a strafe direction |
| `strafe=N` | Strafe at angle N (positive=right, negative=left) |
| `freeze` | Stop all movement |
| `resethead` | Reset head rotation to neutral |
| `rotateheadrandom=N` | Randomly rotate head up to N degrees |
| `lookatplr=N` | Look at player with N% intensity |
| `talk=$N` | Play speech sound from parameter slot N |

## AI Actions (DarkAI)

| Action | Description |
|--------|-------------|
| `aisettarget` | Set the nearest enemy as AI target |
| `airotatetotarget` | Rotate toward AI target |
| `airotatetosound` | Rotate toward the sound source |
| `aifollowplr=N` | Set follow-player mode (1=follow, 0=stop) |
| `aimovetotarget` | Move toward AI target |
| `aimovetocover=N` | Move to nearest cover point (N=0 default) |
| `aimoverandom` | Move to a random position |
| `aistop` | Stop all AI movement |
| `setaiactive=N` | Enable (1) or disable (0) AI activity |
| `aicallteam=N` | Call for team backup within N units |
| `airespondtocall` | Respond to a team call signal |
| `aiusemelee=N` | Execute melee attack (1=on) |
| `aisetmeleedamage=N` | Set melee damage amount |
| `aiusefullaim=1` | Enable full aiming system |

## AI Teams

| Action | Description |
|--------|-------------|
| `addaiteam=N` | Add entity to AI team N (1=ally, 2=enemy) |
| `aiaddenemy=N M` | Add N enemies of type M to the level |

## Variables

| Action | Description |
|--------|-------------|
| `dimvar=name` | Declare/initialize a global variable |
| `setvar=name N` | Set global variable `name` to value N |
| `addvar=name N` | Add N to global variable `name` |
| `subvar=name N` | Subtract N from global variable `name` |
| `incvar` | Increment current local variable by 1 |
| `decvar` | Decrement current local variable by 1 |
| `dimlocalvar=name` | Declare a local variable (per-entity) |
| `globalvar=N` | Access entity global variable slot N (G1-G4) |

## Movement & Lifts

| Action | Description |
|--------|-------------|
| `moveup=N` | Move lift/platform up by N units |
| `raycastup=range height` | Raycast upward: if blocked, don't move; params = horizontal range, target height |
| `norotate=N` | Disable rotation during waypoint follow (for platforms) |

## Spawning

| Action | Description |
|--------|-------------|
| `spawnon` | Enable spawning for this spawn point |
| `spawnoff` | Disable spawning for this spawn point |

## Destruction & Health

| Action | Description |
|--------|-------------|
| `destroy` | Destroy this entity immediately |
| `subhealth=N` | Subtract N health from this entity |
| `sethealth=N` | Set this entity's health to exactly N |
| `addhealth=N` | Add N health to this entity |
| `suspend` | Suspend entity physics (for ragdoll transition) |
| `ragdoll` | Enable ragdoll physics |
| `emitforce=N` | Emit a force at N strength (for crumble) |
| `setforcedamage=N` | Set force damage value |

## Level Flow

| Action | Description |
|--------|-------------|
| `nextlevel=N` | Go to next level (N = transition type: 1=instant, 2=fade) |
| `reset3` | Reset level state (used with nextlevel) |
| `passtosetup=name value` | Pass a value to the setup.ini (e.g., `levelcompleted 1`) |
| `Crosshair=0` | Hide the crosshair |

## Post-Processing Effects

| Action | Description |
|--------|-------------|
| `setposteffect=name` | Apply a post-processing effect. Names: `fog`, `rain`, `bloom`, `nightvision`, `underwater`, `filmgrain`, `filmnoir`, `filmreel`, `filmsepia`, `gasmask`, `motionblur`, `motionsickness`, `pain`, `refract`, `surreal`, `television`, `tonemapping`, `western`, `bleachbypass`, `cellshading`, `depthoffield`, `none` |

## Script Control

| Action | Description |
|--------|-------------|
| `runfpidefault=1` | Run the entity's default/main AI script |
| `runfpi=filename.fpi` | Execute another FPI script on this entity |
| `alwaysactive=1` | Keep this entity active even when far from player |

## Debug

| Action | Description |
|--------|-------------|
| `fpgcrawtext=string` | Display text on screen (debug overlay) |
| `fpgcrawtextsize=N` | Set debug text size |
| `fpgcrawtextfont=name` | Set debug text font |
| `fpgcrawtextx=N` | Set debug text X position (0-100) |
| `fpgcrawtexty=N` | Set debug text Y position (0-100) |
| `fpgcrawtextr=N` | Debug text red channel (0-255) |
| `fpgcrawtextg=N` | Debug text green channel (0-255) |
| `fpgcrawtextb=N` | Debug text blue channel (0-255) |

## 3rd Person

| Action | Description |
|--------|-------------|
| `ThirdPerson=X Y` | Activate third person camera: X units back, Y units left |
| `ThirdPersonHeight=X` | Set third person camera height offset |
| `swapweapon` | Swap 3rd person character weapon model |
| `climb=1` | Hide weapon, play climb animation (for ladders) |
| `climb=0` | Show weapon after climbing |

## Timer

| Action | Description |
|--------|-------------|
| `timerstart` | Start the entity's local timer |
| `etimerstart` | Start the global elapsed timer |

## Alternate Textures

| Action | Description |
|--------|-------------|
| `alttexture=N` | Switch to alternate texture set (0=default, 1=alt) |
