# Variables & Parameters

## Parameter Slots ($0, $1, $2)

Scripts use `$0`, `$1`, `$2` as **fill-in-the-blank slots** set in the FPSC entity editor:

```fpi
:state=0,plrwithinzone=1:state=1,sound=$0,video=$1,stopsound=$0
```

- `$0` = first parameter (typically a sound path)
- `$1` = second parameter (typically a second sound or video path)
- `$2` = third parameter (rarely used)

**Editor setup**: In the entity properties, the script's parameter fields correspond to these slots.

## Global Variables

Declared and used across all entities in the level:

```fpi
:state=0:dimvar=collecteditemA,setvar=collecteditemA 0

:state=10,plrdistwithin=40:state=1,playertake,setvar=collecteditemA 1
```

### Variable Actions

| Action | Example | Description |
|--------|---------|-------------|
| `dimvar=name` | `dimvar=setgrav` | Declare a global variable |
| `setvar=name N` | `setvar=nextsnum 1` | Set variable to value N |
| `addvar=name N` | `addvar=amb 1` | Add N to variable |
| `subvar=name N` | `subvar=amb 1` | Subtract N from variable |

### Variable Conditions

| Condition | Example | Description |
|-----------|---------|-------------|
| `varequal=name N` | `varequal=collecteditemA 1` | Variable == N |
| `varnotequal=name N` | `varnotequal=nextsnum %sid` | Variable != N |
| `vargreater=name N` | `vargreater=allsreset %scount` | Variable > N |
| `varless=name N` | `varless=setgrav 1` | Variable < N |

### Variable Interpolation

Use `%varname` to inject the current value of a variable into an action parameter:

```fpi
:state=4:addvar=setgrav 1,antigravity=%setgrav,state=1
:state=5:subvar=setgrav 1,antigravity=%setgrav,state=1
```

## Local Variables (Per-Entity)

Entity-specific variables using `localvar=N` (slots 1-4):

```fpi
:state=0:localvar=1,setvar=0     ; Select local var slot 1, set to 0
:state=0,plrdistwithin=120,plrusingaction=1,varequal=0:setvar=1,sound=$0
:state=0,plrdistwithin=120,plrusingaction=1,varequal=10:incvar=1,sound=$0
```

**Pattern**: `localvar=N` selects the slot, then `setvar`, `addvar`, `incvar`, `decvar`, `varequal`, etc. operate on it.

## Entity Global Variables (G1-G4)

Four per-entity persistent variable slots:

```fpi
:state=0:globalvar=1              ; Access slot G1
:state=0,plrdistwithin=120,plrusingaction=1,varequal=0:setvar=1  ; G1 == 0 -> set to 1
:state=0,plrdistwithin=120,plrusingaction=1,varequal=10:incvar=1 ; G1 == 10 -> increment
```

Used in scripts like `doorlockpick.fpi` for per-entity state tracking.

## Named Local Variables

Declared with `dimlocalvar`:

```fpi
:state=0:dimlocalvar=sid,setvar=sid 4
```

These are entity-specific but accessed by name rather than slot number.

## Special Variables

| Variable | Purpose |
|----------|---------|
| `%sid` | The entity's own ID number (auto-assigned) |
| `%scount` | Total count of entities in a sequence (set manually) |
| `nextsnum` | Next expected switch number in a sequence puzzle |
| `resetswitches` | Flag to reset all switches in a sequence |
| `allsreset` | Counter of how many switches have been reset |
| `hasbeenreset` | Whether this specific entity has been reset |
| `collecteditemA` | Whether the player has collected "item A" (quest item) |
| `setgrav` | Current antigravity force value |
| `amb` | Current ambient/ambience level (day/night cycle) |
| `eax` | Sun position angle (day/night cycle) |

## Example: Sequence Puzzle Variables

From `speciallogic/Sequence-switch.fpi`:

```fpi
:state=0:dimvar=allsreset,setvar=allsreset 0,dimlocalvar=hasbeenreset,alwaysactive=1

:state=10,plrdistwithin=100,pickobject=1,plrusingaction=1,varequal=nextsnum %sid:state=4,alttexture=1
:state=10,plrdistwithin=100,pickobject=1,plrusingaction=1,varnotequal=nextsnum %sid:setvar=resetswitches 1,state=3

:state=4,varnotequal=nextsnum %scount:addvar=nextsnum 1,state=3
:state=4,varequal=nextsnum %scount:activateifused=1,state=5
```

This checks if the player pressed the correct switch in sequence by comparing `nextsnum` with the entity's `%sid`.

## Best Practices

1. **Always `dimvar` before use** — declare a global variable before setting it
2. **Initialize in state 0** — setup variables during the first frame
3. **Use interpolation sparingly** — `%varname` is powerful but makes scripts harder to read
4. **`localvar=N` selects slot** — you must call `localvar=N` before using `setvar`/`incvar` on that slot
5. **`globalvar=N` selects G-slot** — similar pattern for entity global slots
