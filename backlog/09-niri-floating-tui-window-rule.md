# 09 — `floating.tui` window-rule

**Status:** proposed · **Kind:** niri config · **Depends on:** —

Add a base window-rule that floats any window with app-id `floating.tui`, so launchers
created by `z-tui-install` (item 04) open as tidy floating terminals.

- **Source:** the rule documented in `rl-tui-install`'s help + `rl-misc.kdl`
- **Target:** `system/niri/includes/window-rules.kdl`

## What / why

`z-tui-install` stamps every launcher with app-id `floating.tui` and relies on a single
compositor rule to float them. Shipping that rule in the base makes the tool work out of
the box for every user.

## Change

Add to `system/niri/includes/window-rules.kdl`:

```kdl
window-rule {
    match app-id=r#"^floating\.tui$"#
    open-floating true
    default-column-width { fixed 1000; }
    default-window-height { fixed 650; }
}
```

(Pick the fixed size to taste; the source repo used 800×650 / 1000×650.)

## Acceptance criteria

- [ ] A window with app-id `floating.tui` opens floating at the configured size.
- [ ] `z-tui-install`-generated launchers float without any user-side niri edit.
