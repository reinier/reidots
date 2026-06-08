# 10 — Screencast-target indicator window-rule

**Status:** proposed · **Kind:** niri config · **Depends on:** —

Color the border/focus-ring/shadow/tab-indicator red for any window currently being
screen-cast, as a generic privacy cue.

- **Source:** the `is-window-cast-target=true` rule in `rl-misc.kdl`
- **Target:** `system/niri/includes/window-rules.kdl`

## What / why

Fully generic, no personal data — a sensible safety default for an opinionated image:
you can always see at a glance which window is being shared.

## Change

Add to `system/niri/includes/window-rules.kdl`:

```kdl
window-rule {
    match is-window-cast-target=true
    focus-ring   { active-color "#f38ba8"; inactive-color "#7d0d2d"; }
    border       { inactive-color "#7d0d2d"; }
    shadow       { color "#7d0d2d70"; }
    tab-indicator { active-color "#f38ba8"; inactive-color "#7d0d2d"; }
}
```

Consider sourcing the colors from the matugen palette instead of hardcoded Catppuccin
values, to stay consistent with the dynamic theme — optional refinement.

## Acceptance criteria

- [ ] A window being cast shows the red indicator; normal windows are unaffected.
