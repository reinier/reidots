# How the niri config in this repo reaches niri

Niri itself only ever reads **one** file: `~/.config/niri/config.kdl`. It has no
search path into `/usr/share` or anywhere else. Nothing in this repo is picked up
by niri "automatically" — it gets there via chezmoi plus an absolute-path include,
in two steps:

1. **Chezmoi puts the entrypoint in the home dir.**
   `dot_config/niri/config.kdl` is applied to `~/.config/niri/config.kdl` by the
   reiconium systemd user units (`chezmoi-init.service` on first graphical login,
   `chezmoi-update.service` thereafter). Never copy or edit it by hand — it's
   chezmoi-managed and will be overwritten.

2. **The entrypoint includes the shipped defaults in place.**
   `config.kdl` is just three includes:
   - `include "dms.kdl"` — DMS imports, chezmoi-applied next to it
   - `include "/usr/share/zirconium/zdots/system/niri/zirconium.kdl"` — the
     system-wide defaults, read **in place** from where the reidots submodule
     lands at image build. `system/` is in `.chezmoiignore`, so chezmoi never
     copies it into the home dir.
   - `include optional=true "local.kdl"` — per-user override hook

   `zirconium.kdl` then pulls in the real content
   (`includes/{binds,dms-base,input,layout,misc,shadow,window-rules}.kdl`) and
   finally `include optional=true "/etc/niri/local.kdl"` as the system-wide
   override hook.

## Division of labor

| File | Lives at runtime | How it gets there | What goes in it |
|---|---|---|---|
| `config.kdl`, `dms.kdl` | `~/.config/niri/` | chezmoi apply | nothing — entrypoints only |
| `system/niri/includes/*.kdl` | `/usr/share/zirconium/zdots/system/niri/` | image build (submodule) | shipped defaults — edit these in reidots |
| `~/.config/niri/local.kdl` | home dir | created by hand | personal overrides (never tracked here) |
| `/etc/niri/local.kdl` | system | optional, by hand | machine-wide overrides |

## Caveat

This only works automatically **on reiconium**, where the systemd units run
chezmoi and the submodule sits at `/usr/share/zirconium/zdots`. A standalone
clone elsewhere leaves the absolute include dangling — you'd have to place the
repo at that path or run `chezmoi apply -S <repo>` yourself and adjust the
include. On the actual machine: nothing to do; tweaks belong in
`~/.config/niri/local.kdl`.
