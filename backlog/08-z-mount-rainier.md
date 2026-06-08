# 08 — `z-mount-rainier` (tool only; data stays personal)

**Status:** proposed · **Kind:** script · **Depends on:** 01

Interactive TUI + CLI for managing remote SSHFS mounts declared in a TOML file.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-mount-rainier`
- **Target:** `private_dot_local/bin/executable_z-mount-rainier`
- **Stays personal:** `dot_config/rl-mount-rainier/drives.toml` (the actual hosts) and the
  matching `~/.ssh/config` entries — **never** promote these.

## What / why

The script is a generic, dependency-light mount manager (no hardcoded hosts; it reads the
drive list from config). Only the *config* is personal, so the tool is safe to ship while
the data layers in from `dotfiles-dmsniri`.

## Changes needed

- Rename `rl-` → `z-`, including the config path
  (`~/.config/rl-mount-rainier/drives.toml` → `~/.config/z-mount-rainier/drives.toml`)
  and the `RL_MOUNT_CONFIG` env var (`Z_MOUNT_CONFIG`). Keep them in sync with the
  personal repo's config location.
- Replace the Arch-specific install hints in error messages
  (`"Install: sudo pacman -S sshfs"`, `fuse3`, `xdg-utils`) with distro-neutral wording —
  on reiconium these packages are baked in, so the hint should just name the package.
- Ship an example/commented `drives.toml` as documentation **only if** it contains no real
  hosts (otherwise leave config entirely to the personal repo).

## Image prerequisite (reiconium)

- `sshfs`, `fuse3` (`fusermount3`), `openssh` client, `xdg-utils` (`xdg-open`),
  `util-linux` (`mountpoint`), `ncurses` (`tput`).

## Acceptance criteria

- [ ] Tool runs with an empty/missing config and prints a helpful "no drives configured"
      message (no personal hosts embedded).
- [ ] `z-mount-rainier list/mount/umount/tui` work against a user-supplied `drives.toml`.
- [ ] No `pacman` strings remain.
