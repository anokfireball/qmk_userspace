# External QMK userspace for anokfireball keyboards

Keymaps and build configuration for the MonsGeek M1 and NEO Ergo, built against [anokfireball/qmk_firmware](https://github.com/anokfireball/qmk_firmware) via the [QMK External Userspace](https://docs.qmk.fm/newbs_external_userspace) mechanism.

## How it works

- **Firmware source**: pinned to the exact fork commit in [`firmware-fork-ref`](firmware-fork-ref). Renovate bumps this pin whenever the fork's `master` is rebased and force-published; merging that PR rebuilds both keyboards against the new firmware.
- **Builds**: GitHub Actions builds on every push to `main` (keymap changes and pin bumps) and on manual dispatch. Each green run publishes the two firmware binaries plus a `checksums.txt` as a GitHub release tagged `firmware-<timestamp>-<commit>`.
- **Rebases**: the fork tracks upstream QMK release tags via its own Renovate config and rebase workflow; this repo only follows the fork.

## Build locally

```sh
qmk config user.qmk_home=/path/to/qmk_firmware
qmk config user.overlay_dir=$(pwd)
qmk userspace-compile
```
