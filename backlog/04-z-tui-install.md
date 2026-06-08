# 04 — `z-tui-install`

**Status:** proposed · **Kind:** script · **Depends on:** 01, 09

Create an Applications-menu launcher for a TUI that opens in a floating terminal and
auto-closes when the TUI exits. Companion to `z-webapp-install`; analog of Omarchy's
`omarchy-tui-install`.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-tui-install`
- **Target:** `private_dot_local/bin/executable_z-tui-install`

## What / why

Generic launcher generator. Autodetects a terminal and emits the right per-terminal
`--app-id`/exec syntax. Defaults the app-id to `floating.tui` so a single niri window-rule
floats every launcher it creates (see item 09).

## Changes needed

- Rename `rl-` → `z-` in help text / examples.
- The terminal autodetect list is ghostty-first; **foot** is already in the list and is
  reiconium's terminal, so it works unchanged. Optionally reorder to prefer `foot` (or
  honor `$TERMINAL`) since reiconium dropped ghostty.
- Drop ghostty-specific caveats from the help text if foot is made the default (cosmetic).

## Image prerequisite (reiconium)

- `foot` (already shipped), `desktop-file-utils`.

## Depends on

- **Item 09** (`floating.tui` window-rule) — without it the launchers open tiled, not
  floating. Ship them together.

## Acceptance criteria

- [ ] `z-tui-install "btop" btop` creates a launcher that opens btop floating and closes
      on exit.
- [ ] On reiconium (no ghostty) it picks `foot` and produces a valid `--app-id`.
