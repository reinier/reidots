# CLAUDE.md — `reidots`

Orientation for Claude Code working in this repo.

## What this is

`reidots` is the **chezmoi-managed dotfiles source** for my atomic Fedora desktop image
**`reiconium`** (a bootc/ostree fork of **zirconium**, niri + DankMaterialShell). This repo
is a fork of upstream **`github.com/zirconium-dev/zdots`** and carries my divergences. I pull
relevant upstream changes *in*, but **never PR back upstream** — the flow is one-directional.

It is **not** applied on this machine directly. Inside `reiconium` it is wired in as a git
submodule:

```
reiconium/.gitmodules:
  mkosi.extra/usr/share/zirconium/zdots  →  github.com/reinier/reidots.git
```

So at image-build time this whole repo lands on the OS at **`/usr/share/zirconium/zdots`**,
and two systemd **user** units (shipped by reiconium) apply it as the chezmoi source:

- `chezmoi-init.service` (on `graphical-session-pre.target`, once if missing):
  `chezmoi apply -S /usr/share/zirconium/zdots --config ~/.config/zirconium/chezmoi/chezmoi.toml`
- `chezmoi-update.service`: same, with `--keep-going` and auto-answering `s` to prompts.

> Naming stays `zirconium` on purpose (`/usr/share/zirconium`, `~/.config/zirconium`, the
> `zirconium.kdl` include). Only reiconium's *published image name* differs. Don't rename
> these to "reiconium".

## chezmoi conventions (how to read filenames)

This is a chezmoi **source directory**, so filenames are encoded, not literal:

| Source name                | Applies to                          |
|----------------------------|-------------------------------------|
| `dot_config/`              | `~/.config/`                        |
| `private_dot_local/`       | `~/.local/` (perms kept private)    |
| `dot_cache/`               | `~/.cache/`                         |
| `*.tmpl`                   | rendered with chezmoi template vars (e.g. `{{.chezmoi.homeDir}}`) |
| `symlink_*`                | becomes a symlink; file content is the link target |
| `.chezmoiignore`           | paths chezmoi must **not** apply    |

Examples here: `dot_config/foot/foot.ini.tmpl` (templated include path),
`dot_config/symlink_vesktop.tmpl` (symlinks `~/.config/vesktop` → the Flatpak config dir),
`dot_config/fastfetch/symlink_config.jsonc` → `/usr/share/zirconium/fastfetch.jsonc`.

## Layout

- `dot_config/` — per-user app config applied to `~/.config`: `niri/`, `DankMaterialShell/`,
  `foot/`, `gtk-3.0/`, `gtk-4.0/`, `qt6ct/`, `dgop/`, `fastfetch/`.
- `private_dot_local/` — `~/.local`: KDE color-schemes, DMS session state.
- `dot_cache/` — seeded `~/.cache` (DMS color cache).
- `system/` — **NOT applied by chezmoi** (it's in `.chezmoiignore`). It's the system-wide
  niri config, consumed *in place* from `/usr/share/zirconium/zdots/system/` via absolute
  `include`s. See below.
- `LICENSE`, `CLAUDE.md` — both chezmoi-ignored (dev/planning, never deployed).

## Conventions for shipped tooling

Helper scripts that belong in the base ("batteries included", Omarchy-style) ship as
**reiconium image executables** — `/usr/bin/ember-<name>`, added via `mkosi.extra/usr/bin/`
in the **reiconium** repo — **not** as chezmoi-delivered dotfiles here. That's how
`zjust`/`zmotd` already ship (executables in `/usr/bin`; only *data* under
`/usr/share/zirconium`), and it avoids permanent `zdots` fork divergence. They're debranded
`rl-*` → `ember-*` for the Emberdark rebrand; `rl-*` stays reserved for personal scripts in
`dotfiles-dmsniri`, so the prefix signals "distro tool vs mine". Promoting a tool means its
runtime deps must be baked into the reiconium image. The running list of candidate promotions
lives in **reiconium's** backlog (`reiconium/backlog/`, items `0014`–`0024`) — not here.

## niri config flow (important)

niri config is layered across user + system + DMS, so know which file to touch:

```
~/.config/niri/config.kdl   (dot_config/niri/config.kdl — "DO NOT EDIT", chezmoi-managed)
  ├─ include "dms.kdl"                                  → dot_config/niri/dms.kdl (DMS imports)
  ├─ include "/usr/share/zirconium/zdots/system/niri/zirconium.kdl"   → system/niri/zirconium.kdl
  │     └─ include "includes/{binds,dms-base,input,layout,misc,shadow,window-rules}.kdl"
  │     └─ include optional "/etc/niri/local.kdl"       (system-wide override hook)
  └─ include optional "local.kdl"                       (per-user override hook)
```

- **Default keybinds, input, layout, window-rules** live in `system/niri/includes/*.kdl` —
  edit these to change shipped defaults.
- `config.kdl` and `dms.kdl` are entrypoints — don't put real config there.
- User-facing overrides belong in `~/.config/niri/local.kdl` (never tracked here).

## Generated / tool-managed files — don't hand-edit

These are checked in as defaults but get overwritten at runtime by matugen / DMS; treat
them as generated:

- matugen color outputs: `foot/dank-colors.ini`, `gtk-{3,4}.0/dank-colors.css`,
  `qt6ct/colors/matugen.conf`, `dgop/colors.json`, `niri/dms/colors.kdl`,
  `dot_cache/DankMaterialShell/dms-colors.json`,
  `private_dot_local/share/color-schemes/DankMatugen*.colors`.
- DMS state/settings: `DankMaterialShell/settings.json`, `DankMaterialShell/clsettings.json`,
  `private_dot_local/state/DankMaterialShell/session.json`.

Theme is driven by DMS matugen (`matugenScheme: scheme-fidelity`, dynamic from wallpaper).

## Working here

- Edits and PRs happen in this repo; it then flows into reiconium by bumping the submodule.
- To test a change the way the image does it:
  `chezmoi apply -n -v -S . --config /tmp/cz/chezmoi.toml` (`-n` dry-run; drop it to apply).
- Don't `cd`-and-commit into the parent `~/dev/conium` umbrella — it's not a repo. See
  `~/dev/conium/CLAUDE.md` and `reiconium/FORK.md` for the broader fork/sync context.
