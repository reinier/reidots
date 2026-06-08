# 01 — Script delivery & naming convention

**Status:** proposed · **Kind:** enabler · **Depends on:** —

Establishes *where* promoted tools live in reidots and *how* they're named. Every script
item below assumes this is settled.

## Decision

- **Location:** `private_dot_local/bin/executable_z-<name>` → chezmoi deploys to
  `~/.local/bin/z-<name>`, executable, on PATH. Reuses the existing `private_dot_local/`
  subtree, so `.local`'s directory attributes stay consistent (avoids the private/non-
  private clash that a parallel `dot_local/` would introduce).
- **Naming:** drop the personal `rl-` prefix, adopt `z-` to match `zjust`/`zmotd`/
  `/usr/share/zirconium`. `rl-` remains reserved for personal scripts in `dotfiles-dmsniri`.
- **Internal references:** any script that calls a sibling by hardcoded path/name must use
  the new `z-` name (e.g. `z-copy-md-link` calling `z-launch-or-focus`, not the old
  `~/.local/bin/launch-or-focus`).

## Alternative (note, not chosen here)

Tools you'd rather not carry as fork divergence can instead ship in the **reiconium image**
under `/usr/share/zirconium/bin` (PATH-added) alongside the existing `just/` and `shell/`
dirs — immutable, versioned with the OS, no chezmoi, no `zdots` divergence. Per-script,
pick reidots (config-coupled, chezmoi-delivered) vs image (pure tool).

## Acceptance criteria

- [ ] `private_dot_local/bin/` exists with the `executable_` chezmoi attribute convention.
- [ ] A one-line note added to reidots `CLAUDE.md` documenting the `z-*` + `~/.local/bin`
      convention so future tools land consistently.
- [ ] `chezmoi apply -n` shows the scripts targeting `~/.local/bin/z-*` with mode `0755`.
