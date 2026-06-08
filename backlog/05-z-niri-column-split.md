# 05 — `z-niri-column-split`

**Status:** proposed · **Kind:** script · **Depends on:** 01

Resize the focused column and the one to its right to a preset split (e.g. 50/50 or
65/35) in a single keypress.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-niri-split-50`,
  `…/executable_rl-niri-split-66-34`
- **Target:** one parameterized `private_dot_local/bin/executable_z-niri-column-split`

## What / why

Generic niri layout helper. The two source scripts are byte-identical except for the two
width percentages — promote them as **one** script that takes the split as args, then bind
convenient presets in niri.

## Changes needed

- Collapse the two scripts into `z-niri-column-split <left%> <right%>`
  (e.g. `z-niri-column-split 50 50`, `z-niri-column-split 65 35`).
- Fix the copied-wrong comment header (the `-50` script claims "66% / 34%").
- Rename `rl-` → `z-`.
- Presets get wired as keybinds in **personal** niri config (`rl-binds`), not here — this
  item ships only the mechanism. (Optionally add default binds via item 11's pattern.)

## Image prerequisite (reiconium)

- `niri`, `jq`.

## Acceptance criteria

- [ ] `z-niri-column-split 65 35` sets focused column to 65%, right column to 35%, returns
      focus to the original window.
- [ ] Errors cleanly ("no column to the right") when there's no neighbor.
