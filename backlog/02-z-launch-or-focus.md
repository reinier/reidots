# 02 — `z-launch-or-focus`

**Status:** proposed · **Kind:** script · **Depends on:** 01

Focus a window matching an app-id (and optionally a title) regex, or launch a command if
none is open. The building block other "launch or raise" keybinds compose on.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-launch-or-focus`
- **Target:** `private_dot_local/bin/executable_z-launch-or-focus`

## What / why

Pure mechanism over `niri msg --json windows` + `jq`. No personal data. A natural base
primitive for an opinionated niri image.

## Changes needed

- Rename `rl-` → `z-` in the shebang-comment usage block.
- Otherwise ship as-is — it's already generic.

## Image prerequisite (reiconium)

- `niri`, `jq` (both already core to the niri/DMS image — verify present).

## Acceptance criteria

- [ ] `z-launch-or-focus <app-id>` focuses an open match; launches the command otherwise.
- [ ] No `rl-`/personal references remain.
