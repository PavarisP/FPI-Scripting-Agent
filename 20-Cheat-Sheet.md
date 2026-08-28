# FPI Scripting Cheat Sheet

## Syntax
```
:condition1,condition2:action1,action2,action3
; Comment
desc = Description
```

## State & Activation
| Condition | Action |
|-----------|--------|
| `state=N` | `state=N` |
| `activated=0/1/2/3` | `activate=0/1/2/3` |
| — | `activateifused=1/0` |
| — | `activateallinzone=1` |

## Player Conditions
| Condition | Description |
|-----------|-------------|
| `plrdistwithin=N` | Player within N units |
| `plrdistfurther=N` | Player beyond N units |
| `plrwithinzone=1/0` | Inside/outside zone |
| `plrhigher=N` | Player above N |
| `plrcanbeseen` | Line of sight |
| `plrusingaction=1` | Pressed Enter |
| `plrhaskey=1` | Has key item |
| `plrfacing=N` | Facing entity (degrees) |
| `scancodekeypressed=N` | Key pressed (33=F, 34=G, 35=H) |
| `keypressed=N 1/0` | Key held/released |
| `cantake` | Can pick up item |

## Player Actions
| Action | Description |
|--------|-------------|
| `playertake` | Give item to player |
| `playerdrop` | Drop held item |
| `plraddhealth=N` | Add/subtract health |
| `plrsound=path` | 2D sound in ears |
| `plrspeedmod=N` | Set speed |
| `plrmoveifused` | Teleport to IFUSED |
| `plrmoveto=Name` | Teleport to entity |
| `newjumpheight=N` | Set jump height |
| `associateplayer` | Attach to lift |
| `unassociateplayer` | Detach from lift |

## Animation
| Condition | Action |
|-----------|--------|
| `frameatend=0` | `animate=N` |
| `frameatstart=0` | `incframe=N` |
| `framebeyond=N M` | `decframe=N` |
| `alphafadeequal=N` | `setframe=N` |

## Visibility & Physics
| Action | Description |
|--------|-------------|
| `coloff` / `colon` | Collision off/on |
| `nobulletcol` | Bullets pass through |
| `setalphafade=N` | Set alpha (0-100) |
| `incalphafade=N` | Fade in |
| `decalphafade=N` | Fade out |
| `hide` / `hideshadow=N` | Hide/shadow |
| `nogravity=N` / `antigravity=N` | Gravity control |
| `water=N` / `waterfogdist=N` | Water setup |
| `floorlogic` | Stand on floors |

## Sound & Music
| Action | Description |
|--------|-------------|
| `sound=path` | 3D sound |
| `plrsound=path` | 2D sound |
| `loopsound=path` | Loop sound |
| `stopsound` | Stop sound |
| `soundscale=N` | Volume (0-100) |
| `music=path` | Play music |
| `musicoverride=path` | Override music |
| `musicvolume=N` | Music volume |

## Lights
| Action | Description |
|--------|-------------|
| `lighton` / `lightoff` | On/off |
| `lightintensity=N` | Brightness |
| `movelightred/green/blue=N` | Color |
| `movelightrange=N` | Range |

## Decals
| Action | Description |
|--------|-------------|
| `rundecal=N` | Run effect (0-8) |
| `spinrate=N` | Spin speed |
| `floatrate=N` | Float speed |

## AI (Legacy)
| Condition | Action |
|-----------|--------|
| `plrcanbeseen` | `settarget` |
| `shotdamage=N` | `rotatetoplr` |
| `noiseheard=N` | `useweapon` |
| `ifweapon=1/0` | `reloadweapon` |
| `rateoffire` | `followplr=N` |
| `plringunsight` | `movetotarget` |
| — | `shootplr` |
| — | `strafe=N` |
| — | `freeze` |

## AI (DarkAI)
| Condition | Action |
|-----------|--------|
| `aicanshoot=1/0` | `aisettarget` |
| `aitargetdistwithin=N` | `airotatetotarget` |
| `aitargetdistfurther=N` | `aimovetotarget` |
| `aiatcover=1/0` | `aimovetocover=N` |
| `aiheardsound=N` | `aistop` |
| `idle=1/0` | `aifollowplr=N` |
| `ducking=1/0` | `setaiactive=N` |
| — | `aiusemelee=N` |
| — | `aicallteam=N` |
| — | `addaiteam=N` |

## Variables
| Action | Description |
|--------|-------------|
| `dimvar=name` | Declare global |
| `setvar=name N` | Set to N |
| `addvar=name N` | Add N |
| `subvar=name N` | Subtract N |
| `incvar` / `decvar` | Increment/decrement |
| `localvar=N` | Select local slot |
| `globalvar=N` | Select G-slot |
| `dimlocalvar=name` | Named local var |
| `%varname` | Interpolate value |

## Variables (Conditions)
| Condition | Description |
|-----------|-------------|
| `varequal=name N` | == N |
| `varnotequal=name N` | != N |
| `vargreater=name N` | > N |
| `varless=name N` | < N |

## Timing & Random
| Condition | Action |
|-----------|--------|
| `random=N` | 1-in-N chance |
| `timergreater=N` | Timer > N ms |
| `etimergreater=N` | Global timer > N |
| `soundfinished=1` | Sound done |
| — | `timerstart` / `etimerstart` |

## GUI (Menu System)
| Action | Description |
|--------|-------------|
| `resetgui` | Reset GUI |
| `loadimage=name path` | Load image |
| `makehud=name norm over x y` | Create button |
| `showhud=name` / `hidehud=name` | Show/hide |
| `showcursor` / `hidecursor` | Cursor |
| `makechoice=name ...` | Dropdown |
| `makecheckbox=name ...` | Checkbox |
| `hudmouseup=name 1` | Click detection |
| `pausegame` / `resumegame` | Pause/resume |
| `loadgame` / `savegame` | Save/load |
| `quitgame` | Quit |
| `backdrop=path` | Background |

## Level Flow
| Action | Description |
|--------|-------------|
| `nextlevel=N` | Next level (1=instant, 2=fade) |
| `reset3` | Reset state |
| `passtosetup=name value` | Write setup.ini |
| `Crosshair=0` | Hide crosshair |
| `destroy` | Destroy entity |

## Post-Processing
| Action | Names |
|--------|-------|
| `setposteffect=name` | fog, rain, bloom, nightvision, underwater, filmgrain, filmnoir, gasmask, motionblur, pain, refract, surreal, television, tonemapping, western, bleachbypass, cellshading, depthoffield, none |

## HUD
| Action | Description |
|--------|-------------|
| `hudreset` | Reset HUD |
| `hudx=N` / `hudy=N` | Position |
| `hudimage=path` | Full image |
| `hudimagefine=path` | Alpha image |
| `hudname=name` | Reference name |
| `hudmake=display` | Overlay |
| `hudmake=internal` | Full screen |
| `hudshow=name` | Show |
| `hudfadeout=name` | Fade + destroy |
| `changehudalpha=name N` | Set alpha |

## Script Control
| Action | Description |
|--------|-------------|
| `runfpidefault=1` | Run main AI |
| `runfpi=file.fpi` | Chain to script |
| `alwaysactive=1` | Keep active at distance |

## Door/Lift Pattern
```
Door: setframe=0 → incframe → frameatend → coloff → decframe → frameatstart → setframe=0
Lift: moveup, raycastup, associateplayer, unassociateplayer
```

## Pickup Pattern
```
State 0: HUD setup → state 10
State 10: wait for plrdistwithin + condition → playertake, coloff, hudshow, hudfadeout
```

## Zone Pattern
```
Enter: state=0,plrwithinzone=1 → action, state=1
Exit:  state=1,plrwithinzone=0 → cleanup, state=0
Always-active (no state): plrwithinzone=1 → per-frame action
```

## Common Scan Codes
| Key | Code |
|-----|------|
| F | 33 |
| G | 34 |
| H | 35 |
| Pause | 197 |
| Escape | (escapekeypressed) |
| NumPad8 | 72 |
| NumPad2 | 80 |
