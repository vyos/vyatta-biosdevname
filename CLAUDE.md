# CLAUDE.md

## Project purpose

VyOS fork of `biosdevname` — a Dell-originated utility that maps Linux kernel network-device names (e.g. `eth0`) to BIOS/chassis-supplied names (e.g. `Gb1`). Ships as the `vyatta-biosdevname` Debian package and is invoked by udev rules during interface naming.

## Tech stack

- C, GNU autotools (`configure.ac`, `Makefile.am`, `ltmain.sh`).
- Debian packaging in `debian/` (debhelper >= 5, `autotools-dev`, `libpci-dev`, `autoconf`, `automake`).
- License: GPL-2.0 (per `COPYING`).

## Build / test / run

```sh
autoreconf -fi
./configure
make
dpkg-buildpackage -us -uc
```

No in-tree test runner.

## Repository layout

- `src/` — main biosdevname source.
- `biosdevname.1` — man page.
- `biosdevname.rules.in` — udev rules template.
- `biosdevname.spec.fedora.in`, `biosdevname.spec.suse.in` — RPM specs (upstream).
- `debian/`.

## Cross-repo context

Listed in `VyOS-Networks/vyos-build-packages/repos.toml`. Pulled into the ISO via `vyos/vyos-build`. Provides predictable interface naming used throughout `vyos-1x`'s ifconfig logic.

## Conventions

- Default branch `current`.
- Commit / PR title format: `component: T12345: description` (Phorge task ID at https://vyos.dev).
- Active workflows: `cla-check.yml`, `trigger-rebuild-repo-package.yml` — wired into the rebuild-dispatch chain, but **not** the PR-mirror pipeline.
- Treat as upstream-vendored; minimise diffs against the original Dell biosdevname.

## Mirror relationship

Mirror twin: `VyOS-Networks/vyatta-biosdevname`. Canonical side is here. The VyOS-Networks twin's default branch is the experimental `git-actions` (per relations doc §8.2) — ignore for canonical state.

## Notes for future contributors

- Renaming the source/binary package from upstream `biosdevname` to `vyatta-biosdevname` is intentional — don't revert.
- Any change here can affect interface naming on every VyOS install; coordinate with `vyos-1x` if you change rule names.
