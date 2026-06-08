# Backlog — Omarchy-style "batteries-included" promotions

This directory tracks proposed changes that make `reidots` a more opinionated,
batteries-included base — taking the **generic tooling** out of my personal
`dotfiles-dmsniri` repo and shipping it as a reiconium default, the way
[Omarchy](https://omarchy.org) bundles helper scripts as part of the system.

These files are **planning notes only** — `backlog` is in `.chezmoiignore`, so nothing
here is ever deployed to `$HOME`.

## How to use this backlog

Each item is **atomic and independent** (except where it lists `Depends on:`). Pick the
ones you want, skip the rest. An item describes only the **reidots-side change** — the
mechanism, debranded and stripped of personal data. The matching personal *content*
(menus, host lists, secrets, keybinds) stays in `dotfiles-dmsniri` and is out of scope
here by design.

## Shared conventions (decided in item 01, assumed by the rest)

- **Debrand `rl-*` → `z-*`.** Matches reiconium's existing namespace (`zjust`, `zmotd`,
  `/usr/share/zirconium`). The `rl-` prefix stays reserved for personal scripts, so the
  prefix itself signals "distro tool vs mine".
- **Tools live at `private_dot_local/bin/executable_z-*`** → chezmoi deploys to
  `~/.local/bin` (on PATH), consistent with the existing `private_dot_local/` tree.
- **The image must provide the runtime deps.** reidots only ships the script; the
  packages it calls (`jq`, `sshfs`, `wtype`, …) are a **reiconium image** concern. Each
  item lists them under *Image prerequisite* — that package-manifest work happens in the
  `reiconium` repo, not here, and gates the script actually working on a clean install.
- **Fork-churn caveat.** reidots is a fork of `zirconium-dev/zdots`; every file added here
  is a permanent divergence the sync flow (`reiconium/FORK.md`) carries past upstream.
  Acceptable for config-coupled tools; for anything you'd rather not carry, the
  alternative home is the reiconium image itself (`/usr/share/zirconium/bin`).

## Items

| # | Item | Kind | Depends on |
|---|------|------|------------|
| 01 | Script delivery & naming convention | enabler | — |
| 02 | `z-launch-or-focus` | script | 01 |
| 03 | `z-webapp-install` | script | 01 |
| 04 | `z-tui-install` | script | 01, 09 |
| 05 | `z-niri-column-split` | script | 01 |
| 06 | `z-paste-over-text` | script | 01 |
| 07 | `z-copy-md-link` (needs rework) | script | 01 |
| 08 | `z-mount-rainier` (tool only) | script | 01 |
| 09 | `floating.tui` window-rule | niri config | — |
| 10 | screencast-target indicator window-rule | niri config | — |
| 11 | leader-menu keybind (Dank Lader) | niri config, optional | — |
