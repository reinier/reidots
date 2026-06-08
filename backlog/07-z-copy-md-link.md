# 07 — `z-copy-md-link` (needs rework before promotion)

**Status:** proposed · **Kind:** script · **Depends on:** 01

Copy the focused browser tab as a markdown link `[title](url)`.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-copy-md-link`
- **Target:** `private_dot_local/bin/executable_z-copy-md-link`

## What / why

Useful generic action — **but** the current implementation is Chromium-Flatpak-specific
and not yet image-clean. Promote only after generalizing.

## Changes needed (this is the work, not a copy-paste)

- Remove the hardcoded `flatpak run org.chromium.Chromium` focus call and the
  `~/.local/bin/launch-or-focus` reference (old un-prefixed name). Either drop the focus
  step or make it call `z-launch-or-focus` with a configurable app-id.
- Stop stripping the literal `" - Chromium"` title suffix — make the browser/title
  handling browser-agnostic (or env-configurable).
- Rename `rl-` → `z-`.

## Image prerequisite (reiconium)

- `niri`, `jq`, `wtype` (synthesizes Ctrl+L/Ctrl+C), `wl-clipboard`, `libnotify`.
- Note `wtype`-driving-the-address-bar is fragile; consider whether this belongs in the
  base at all, or stays personal until a cleaner approach exists.

## Acceptance criteria

- [ ] No hardcoded browser binary or app-id; works with reiconium's default browser.
- [ ] No reference to the old `launch-or-focus` name.
- [ ] Decision recorded: promote to reidots, or keep personal (acceptable outcome).
