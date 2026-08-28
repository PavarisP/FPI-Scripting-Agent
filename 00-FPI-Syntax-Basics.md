# FPI Syntax Basics

## File Structure

Every `.fpi` file follows this exact structure:

```
;Artificial Intelligence Script         ; Optional — standard header comment

;Header

desc          = Description Here         ; REQUIRED — shown in editor dropdown

;Triggers

:condition:action                        ; Transition rules (one per line)

;End of Script                           ; Optional footer
```

## The Trigger-Action Rule

Each line starting with `:` defines a single **state transition**:

```
:condition1,condition2,...:action1,action2,action3,...
```

- **Colon (`:`)** separates conditions (left) from actions (right)
- **Comma (`,`)** separates multiple conditions or multiple actions
- All conditions must be **true** for the rule to fire
- Actions execute **in order** left-to-right
- Only **one rule** fires per frame (the first matching one)

## State Machine Model

`state=N` is the fundamental control variable:

```fpi
:state=0:state=1                         ; Always: go from state 0 to state 1
:state=1,plrdistwithin=100:state=2       ; In state 1, if player within 100, go to state 2
:state=2:incframe=0                      ; In state 2, increment animation from frame 0
:state=2,frameatend=0:state=3,coloff     ; In state 2, when animation ends, go to state 3 + disable collision
```

**States are just integers** — use any numbers you like. Common conventions:
- `0` = init/setup
- `1-9` = active states
- `10+` = setup/waiting states (often used for HUD setup before interaction)

## Always-Active Rules (No State Condition)

Rules without a `state=` condition fire **every frame** as long as their conditions are met:

```fpi
:always:limblight=2                      ; Fire every frame unconditionally
:plrdistwithin=150:newjumpheight=100     ; Every frame player is within 150 units
:plrwithinzone=1:state=1,plraddhealth=1  ; Every frame player is inside zone
```

These are evaluated alongside state-based rules.

## Comments

```fpi
; This is a comment — anything after semicolon on a line
:state=0:state=1  ; Inline comment also works
```

## Special: Entity-Level Flags

Some flags are set outside the trigger system, on their own lines:

```fpi
::isaltammo                               ; Entity-level flag: alternative ammo type
:state=0:hudreset,...,isaltammo,state=10  ; Or set via action
```

## Script Header Field

```fpi
desc          = Player Proximity Door (Open and Close)
```

The `desc` field is **required** — it appears in the FPS Creator script dropdown. Keep it short and descriptive.

## Minimal Working Script

```fpi
;Artificial Intelligence Script

;Header

desc          = Do Nothing (Empty)

;Triggers

:state=0:state=1

;End of Script
```

## Common Syntax Errors to Avoid

- Missing `:` before conditions
- Spaces in variable names (use camelCase or underscores)
- Forgetting `state=N` in a state-based rule (rule won't match)
- Using `=` instead of `=` (e.g., `plrdistwithin=100` not `plrdistwithin==100`)
- Trailing commas on action lists

## Root Directory Configuration

Change this to your FPS Creator `Files\` directory:

```
Directory=C:\Program Files (x86)\The Game Creators\FPS Creator\Files
```

This is used by AI agents to know where to read/write FPI scripts and reference assets. The directory contains all the subdirectories documented in `24-FPSC-Installation-Reference.md`.
