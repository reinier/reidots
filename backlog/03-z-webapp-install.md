# 03 — `z-webapp-install`

**Status:** proposed · **Kind:** script · **Depends on:** 01

Create a Chromium-based web-app launcher (`.desktop` + icon + stable Wayland app-id). This
is the direct analog of Omarchy's `omarchy-webapp-install`, already reimplemented with no
`gum`/omarchy dependency.

- **Source:** `dotfiles-dmsniri/dot_local/bin/executable_rl-webapp-install`
- **Target:** `private_dot_local/bin/executable_z-webapp-install`

## What / why

Generic, self-contained, interactive or non-interactive. Autodetects any Chromium-family
browser, derives the per-window app-id the way Chromium does, fetches a favicon. No
personal data (the *web apps you create* stay personal).

## Changes needed

- Rename `rl-` → `z-` in help text / examples.
- Keep the `WEBAPP_BROWSER` env override and `-b/--browser` flag as-is.

## Image prerequisite (reiconium)

- A Chromium-family browser (`chromium` or similar) — already implied by the DMS setup.
- `curl` (favicon fetch), `desktop-file-utils` (`update-desktop-database`).

## Acceptance criteria

- [ ] `z-webapp-install "Name" https://example.com` writes a working `.desktop` and the
      window's app-id matches `StartupWMClass` (icon associates in DMS).
- [ ] Runs with no `gum`/omarchy/`rl-` dependency.
