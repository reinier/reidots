# 11 — Leader-menu keybind (Dank Lader)

**Status:** proposed · **Kind:** niri config · **Optional** · **Depends on:** —

Ship a default keybind that toggles the **Dank Lader** leader menu, so an opinionated
"press the leader key → menu" workflow works out of the box.

- **Target:** `system/niri/includes/binds.kdl`
- **Out of scope (stays personal):** the *menu contents* (`dank-lader/config.toml` — apps,
  vaults, hostnames, snippets) and the `rl-binds` key choices.

## What / why

Dank Lader (DMS plugin) is the chosen leader menu — replacing the wlr-which-key/`rl-leaderkey`
approach, which is **not** being promoted. The base should provide the *trigger* keybind;
the user provides the *menu*.

## Open questions to resolve before doing this

- **Invocation:** confirm how the Dank Lader DMS plugin is toggled (likely
  `dms ipc call <plugin> toggle`, not the old `dank-lader-ctl` Unix-socket script). Do
  **not** promote `dank-lader-ctl` until confirmed it's still the right interface.
- **Plugin availability:** the keybind only makes sense if reiconium ships/enables the
  Dank Lader plugin — that's a reiconium image decision. Gate this item on it.
- **Key choice:** pick a sensible default (the personal setup overloads the Super tap via
  keyd → `Mod+F12`; that's hardware-specific and stays personal). Choose something
  hardware-neutral for the base.

## Acceptance criteria

- [ ] Dank Lader plugin confirmed shipped/enabled in reiconium.
- [ ] One base keybind toggles it; no personal menu contents in reidots.
- [ ] `rl-leaderkey` / wlr-which-key explicitly **not** carried over.
