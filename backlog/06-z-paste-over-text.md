# 06 — `z-paste-over-text`

**Status:** proposed · **Kind:** script · **Depends on:** 01

Turn the primary selection + a URL on the clipboard into a markdown link
`[selection](url)` and put it back on the clipboard, ready to paste.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-paste-over-text`
- **Target:** `private_dot_local/bin/executable_z-paste-over-text`

## What / why

Small, fully generic clipboard utility. No personal data, no app coupling.

## Changes needed

- Rename `rl-` → `z-`.
- The header comment references `wlr-which-key` as the example trigger — reword to
  "bind it to a key (or a Dank Lader entry)", since reiconium uses Dank Lader, not
  wlr-which-key.

## Image prerequisite (reiconium)

- `wl-clipboard` (`wl-paste`/`wl-copy`), `libnotify` (`notify-send`).

## Acceptance criteria

- [ ] With a URL on the clipboard and text selected, invoking it leaves
      `[text](url)` on the clipboard and shows a notification.
- [ ] Sensible errors when the clipboard or selection is empty.
