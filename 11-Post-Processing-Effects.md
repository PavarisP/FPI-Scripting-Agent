# Post-Processing Effects

Post-processing effects are applied via trigger zones using the `setposteffect` action. Each effect has a corresponding `.fx` shader file in `effectbank/postprocess/`.

## Common Post-Process Script Pattern

All post-effect scripts follow the same pattern:

```fpi
:state=0,plrwithinzone=1:state=1
:state=1:setposteffect=effectname,state=3

:state=3,plrwithinzone=1:state=4
:state=3,plrwithinzone=0:state=0

:state=4:state=3
```

- **Enter zone** → apply effect
- **Leave zone** → remove effect (state returns to 0)
- State 3/4 loop keeps the effect active while inside

## Available Post-Processing Effects

| Effect Name | Shader File | Description |
|-------------|-------------|-------------|
| `bleachbypass` | `bleachbypass.fx` | High-contrast film look |
| `bloom` | `bloom.fx` | Glowing bright areas |
| `cellshading` | `cellshading.fx` | Cartoon/toon shading |
| `depthoffield` | `depthoffield.fx` | Blur far/near objects |
| `filmgrain` | `filmgrain.fx` | Grainy film texture |
| `filmnoir` | `filmnoir.fx` | Black & white noir |
| `filmreel` | `filmreel.fx` | Old film reel scratches |
| `filmsepia` | `filmsepia.fx` | Sepia tone |
| `fog` | `fog.fx` | Dense fog overlay |
| `gasmask` | `gasmask.fx` | Gas mask vignette |
| `motionblur` | `motionblur.fx` | Blur during movement |
| `motionsickness` | `motionsickness.fx` | Disorienting wobble |
| `multi` | `multi.fx` | Combined effects (used by underwater) |
| `nightvision` | `nightvision.fx` | Green night vision |
| `none` | `none.fx` | Clears all effects |
| `pain` | `pain.fx` | Red pain overlay |
| `rain` | `rain.fx` | Rain droplets on screen |
| `refract` | `refract.fx` | Heat distortion/refraction |
| `surreal` | `surreal.fx` | Dream-like color shift |
| `television` | `television.fx` | Scanlines/TV effect |
| `tonemapping` | `tonemapping.fx` | HDR tone mapping |
| `underwater` | `multi.fx` | Blue tint + blur (uses multi shader) |
| `western` | `western.fx` | Western/desert color grade |

## Toggle Versions

Some effects have toggle scripts (press a key to turn on/off):

| Script | Effect | Key |
|--------|--------|-----|
| `gasmask-toggle.fpi` | Gas mask | Toggle key |
| `nightvision-toggle.fpi` | Night vision | Toggle key |

## Example: Night Vision Zone

```fpi
:state=0,plrwithinzone=1:ambience=30,state=1
:state=1:setposteffect=nightvision,state=3

:state=3,plrwithinzone=1:state=4
:state=3,plrwithinzone=0:state=0

:state=4:state=3
```

Also adjusts `ambience=30` (darkens the environment) so night vision is visible.

## Full List of Post-Process Scripts

Located in `scriptbank/posteffects/`:

`bleachbypass.fpi`, `bloom.fpi`, `cellshading.fpi`, `depthoffield.fpi`, `filmgrain.fpi`, `filmnoir.fpi`, `filmreel.fpi`, `filmsepia.fpi`, `fog.fpi`, `gasmask.fpi`, `gasmask-toggle.fpi`, `motionblur.fpi`, `motionsickness.fpi`, `nightvision.fpi`, `nightvision-toggle.fpi`, `nomadmod.fpi`, `none.fpi`, `pain.fpi`, `rain.fpi`, `refract.fpi`, `surreal.fpi`, `television.fpi`, `tonemapping.fpi`, `underwater.fpi`, `western.fpi`

## Global Settings (setuplevel.fpi)

```fpi
:state=0:fog=0,fogred=0,foggreen=0,fogblue=0
:state=0:ambience=25,ambiencered=255,ambiencegreen=255,ambienceblue=255
```

Level-wide fog and ambient settings can also be configured here.
