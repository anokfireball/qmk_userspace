# External QMK userspace for anokfireball keyboards

Keymaps and build configuration for the MonsGeek M1 and NEO Ergo, built against [anokfireball/qmk_firmware](https://github.com/anokfireball/qmk_firmware) via the [QMK External Userspace](https://docs.qmk.fm/newbs_external_userspace) mechanism.

## How it works

- **Firmware source**: pinned to the exact fork commit in [`firmware-fork-ref`](firmware-fork-ref). Renovate bumps this pin whenever the fork's `master` is rebased and force-published; merging that PR rebuilds both keyboards against the new firmware.
- **Builds**: GitHub Actions builds on every push to `main` (keymap changes and pin bumps) and on manual dispatch. Each green run publishes the two firmware binaries plus a `checksums.txt` as a GitHub release tagged `firmware-<timestamp>-<commit>`.
- **Rebases**: the fork tracks upstream QMK release tags via its own Renovate config and rebase workflow; this repo only follows the fork.

## SOCD cleaning

Both keymaps use [Getreuer's SOCD Cleaner](https://getreuer.info/posts/keyboards/socd-cleaner/index.html) community module (vendored as the `modules/getreuer` submodule, enabled through each keymap's `keymap.json`) on `W`/`S` and `A`/`D` with last-input-priority resolution. It is active only while the default layer is `DEF_B`: the plain layer OS detection falls back to when it matches neither Windows/Linux nor macOS, which can also be selected by hand via the FN layers' `DF` keys (`DEF_B` → `WIN_B` → `MAC_B` → `DEF_B`). The WIN and MAC base layers put home-row mod-taps on `A`/`S`/`D`, so SOCD is off there. Check game rules before use; some games (e.g. Counter-Strike) prohibit SOCD filtering.

## Build locally

```sh
qmk config user.qmk_home=/path/to/qmk_firmware
qmk config user.overlay_dir=$(pwd)
qmk userspace-compile
```
