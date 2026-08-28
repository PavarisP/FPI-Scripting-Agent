# HUD System

FPSC has two HUD types: **display** (overlay) and **internal** (full-screen). HUDs are created and managed through FPI script actions.

## HUD Types

| Type | `hudmake` | `hudtype` | Description |
|------|-----------|-----------|-------------|
| Overlay | `hudmake=display` | (not set) | Floating overlay with transparency, named elements |
| Full-screen | `hudmake=internal` | `hudtype=3` | Full-screen image (e.g., faders, damage overlays) |
| Text | `hudmake=display` | `hudtype=1` | Text-based HUD elements |
| Internal Image | `hudmake=internal` | `hudtype=2` | Colored overlay (e.g., red damage vignette) |
| Zoom | `hudmake=internal` | `hudtype=4` | Scope/zoom overlay |

## HUD Creation Pattern

### Overlay (Display) HUD

```fpi
:state=0:hudreset,hudx=50,hudy=90,hudimagefine=gamecore\text\pickedupanitem.tga,hudname=itemprompt,hudhide=1,hudmake=display,state=10
```

1. `hudreset` — start fresh
2. `hudx=50, hudy=90` — position (0-100 percentage)
3. `hudimagefine=path` — image with transparency
4. `hudname=itemprompt` — name to reference later
5. `hudhide=1` — start hidden
6. `hudmake=display` — create as overlay

Then show/fade it:
```fpi
:state=10,plrdistwithin=40:state=1,playertake,...,hudshow=itemprompt,hudfadeout=itemprompt
```

### Full-Screen (Internal) HUD

```fpi
:state=0:hudreset,hudx=50,hudy=50,hudsizex=1024,hudsizey=768,hudimage=gamecore\huds\fader.tga,hudhide=1,hudtype=3,hudmake=internal
```

- `hudimage=path` — no transparency (legacy)
- `hudtype=3` — full image type
- `hudsizex/hudsizey` — fixed pixel size (or use `hudglobalx/hudglobaly` for HD)

### Text HUD

```fpi
:state=0:hudreset,hudx=50,hudy=10,hudsize=32,hudfont=verdana,hudtext=Press Numpad Arrow Up & Down Key,hudname=keys,hudtype=1,hudmake=display
```

- `hudfont=name` — font face
- `hudtext=string` — text content
- `hudsize=N` — font size
- `hudtype=1` — text type

## HUD Actions Reference

| Action | Description |
|--------|-------------|
| `hudreset` | Clear HUD state for a new element |
| `hudx=N` | X position (0-100 = screen percentage) |
| `hudy=N` | Y position (0-100) |
| `hudsize=N` | Text size |
| `hudsizex=N` | Width in pixels (legacy) |
| `hudsizey=N` | Height in pixels (legacy) |
| `hudglobalx` | HD dynamic X scaling (BIMA mod) |
| `hudglobaly` | HD dynamic Y scaling (BIMA mod) |
| `hudfont=name` | Font name (e.g., `verdana`, `arial`) |
| `hudtext=string` | Text content |
| `hudimage=path` | Full-screen image (no alpha) |
| `hudimagefine=path` | Overlay image (with alpha transparency) |
| `hudname=name` | Assign a reference name |
| `hudtype=N` | 1=text, 2=colored overlay, 3=image, 4=zoom |
| `hudhide=N` | 1=start hidden |
| `hudmake=display` | Create overlay HUD |
| `hudmake=internal` | Create internal HUD |
| `hudshow=name` | Show a hidden HUD |
| `hudfadeout=name` | Fade out and destroy |
| `changehudalpha=name N` | Set alpha (0-255) |
| `hudred=N` | Red tint (0-255) |
| `hudgreen=N` | Green tint (0-255) |
| `hudblue=N` | Blue tint (0-255) |

## Advanced HUD System (GUI-X9)

The `GUI-X9/` directory contains the full GUI system used by the game's menus:

### GUI System Actions

| Action | Description |
|--------|-------------|
| `resetgui` | Reset all GUI state |
| `loadimage=name path` | Load an image resource |
| `makehud=name normal over x y` | Create interactive button (normal + hover images) |
| `showhud=name` | Show a HUD element |
| `hidehud=name` | Hide a HUD element |
| `hideall` | Hide all HUD elements |
| `showcursor` | Show the mouse cursor |
| `hidecursor` | Hide the mouse cursor |
| `setcursor=name` | Set cursor image |
| `makechoice=name back none slider x y` | Create dropdown choice |
| `addchoicevalue=name value` | Add option to choice |
| `setchoicevalue=name value` | Set current choice |
| `makecheckbox=name unsel sel x y` | Create checkbox |
| `setcheckboxchecked=name 1/0` | Check/uncheck |
| `makesvar=name default setupkey` | Create settings variable |
| `readsetupline=name setupkey` | Read from setup.ini |
| `setsvarvalue=name value` | Set settings variable |
| `setsvartogui=var checkbox` | Save checkbox state to variable |
| `savesvars` | Save all settings variables |
| `savesetupvars` | Save all setup variables |
| `hudmouseup=name 1` | Mouse click on HUD element |
| `escapekeypressed=1` | Escape key condition |
| `pausegame` | Pause the game |
| `resumegame` | Resume the game |
| `quicksavegame` | Quick save |
| `quickloadgame` | Quick load |
| `loadgame` | Open load game dialog |
| `savegame` | Open save game dialog |
| `continuegame` | Continue (used for exit) |
| `quitgame` | Quit the game |
| `destroy` | Destroy this GUI script |
| `reset` | Reset GUI state |
| `backdrop=path` | Set background image |
| `sky=path` | Set skybox |
| `swgreater=name N` | Stopwatch greater than condition |
| `startsw=name N` | Start stopwatch |
| `makesw=name N` | Create stopwatch |

### GUI System Flow

```
titlepage.fpi → [New Game] → setuplevel.fpi → [game loop]
                                  → loadingpage.fpi
                                  → gameover.fpi (on death)
                                  → gamewon.fpi (on completion)
```

## HUD Numeric Displays

```fpi
:state=0:loadimage=NUM1 gamecore\huds\numeric1.png,loadimage=AMTitle gamecore\huds\ammo.png
:state=0:makehud=LNUM NUM1 NUM1 4 8,sethudnumeric=LNUM 1,sethudvalue=LNUM lives
:state=0:makehud=HNUM NUM1 NUM1 14 8,sethudnumeric=HNUM 1,sethudvalue=HNUM health
:state=0:makehud=ANUM NUM1 NUM1 88 12,sethudnumeric=ANUM 1,sethudvalue=ANUM ammo
```

- `sethudnumeric=name N` — set number of digits
- `sethudvalue=name value` — set value (e.g., `health`, `lives`, `ammo`)

## BIMA HD Scaling

The Black Ice Mod introduced `hudglobalx` and `hudglobaly` to replace fixed `hudsizex/hudsizey`:

```fpi
; Legacy (fixed resolution):
:state=0:hudreset,hudx=50,hudy=50,hudsizex=1024,hudsizey=768,...

; BIMA HD (dynamic scaling):
:state=0:hudreset,hudx=50,hudy=50,hudglobalx,hudglobaly,...
```

## Fade-Out Transition Pattern

```fpi
:state=0:hudreset,hudx=50,hudy=50,hudimagefine=gamecore\huds\fadein\blank.png,hudname=fadeout,hudhide=1,hudmake=display
:state=0,plrwithinzone=1:etimerstart,hudshow=fadeout,state=1

:state=1,etimergreater=100:changehudalpha=fadeout 20,state=2
:state=2,etimergreater=200:changehudalpha=fadeout 40,state=3
; ... continues in 20-alpha steps every 100ms ...
:state=13:nextlevel=2,reset3
```

## Pre-built HUD Scripts

Located in `scriptbank/ammo huds/` and `scriptbank/health huds/`:

| Script | Type |
|--------|------|
| `AutoWeap_Ammo_horizontal.fpi` | Horizontal auto-weapon ammo |
| `Hand_Autoweapon_Combo.fpi` | Handgun + auto combo |
| `Handgun_AmmoCounter.fpi` | Handgun ammo counter |
| `horizontal_healthbar.fpi` | Horizontal health bar |
| `vertical_healthbar.fpi` | Vertical health bar |
| `vertical_healthbar_hud.fpi` | Vertical health bar HUD |
| `horizontal_health_lives.fpi` | Horizontal health + lives |
| `hud_anim.fpi` | Animated HUD element |
