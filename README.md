# OrangeFox for Infinix Smart 8 (X6525)

Unofficial OrangeFox **R12.0_2** as `vendor_boot.img` for X6525 / UMS9230.
Recovery lives in `vendor_boot` (A/B, virtual A/B, header v4), so the build target is `vendorboot`.

## Credits

- **OrangeFox Recovery Project** — recovery source, vendor and sync scripts (base `R12.0`).
- **Kyros70** — original X6525 device tree (`OFOX`) and the `graphics_drm.cpp` fix.
- **Bocchi-The-Dev** — GitHub Actions workflow.
- **TeamWin / TWRP** — minimal manifest and common recovery config.
- **Mooments1** — fork, patches, builds and releases.

## Sources

| Source | Ref |
|---|---|
| [Device tree (original)](https://github.com/Kyros70/android_device_infinix_x6525_recovery) | `OFOX` |
| [Device tree (patched)](https://github.com/Mooments1/android_device_infinix_x6525_recovery) | `OFOX-Mooments1` |
| [OrangeFox sync script](https://gitlab.com/OrangeFox/sync) | `master` (v034, `R12.0`) |
| [TWRP minimal manifest](https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp) | `twrp-12.1` |
| [OrangeFox recovery](https://gitlab.com/OrangeFox/bootable/Recovery) | `fox_12.1` |
| [OrangeFox vendor](https://gitlab.com/OrangeFox/vendor/recovery) | `main` |
| [graphics_drm.cpp patch](https://github.com/Kyros70/android_bootable_recovery) | `android-12.1` |

## Build

Actions → **OrangeFox [OFRP]** → Run workflow with `MANIFEST_BRANCH=12.1`, `DEVICE_TREE_BRANCH=OFOX-Mooments1`, `BUILD_TARGET=vendorboot` (other inputs stay default).

```bash
gh workflow run build-ofox.yml --repo Mooments1/android_device_infinix_x6525_recovery \
  -f MANIFEST_BRANCH=12.1 \
  -f DEVICE_TREE=https://github.com/Mooments1/android_device_infinix_x6525_recovery \
  -f DEVICE_TREE_BRANCH=OFOX-Mooments1 \
  -f DEVICE_NAME=X6525 -f DEVICE_PATH=device/infinix/X6525 \
  -f BUILD_TARGET=vendorboot -f LDCHECK=false -f LDCHECKPATH=system/bin/qseecomd
```

The workflow syncs `fox_12.1`, clones the tree, applies the graphics fix, runs
`lunch twrp_X6525-eng && mka adbd vendorbootimage`, then publishes the image to a Release.

## Release and flash

Release tag = workflow run ID; asset = `OrangeFox-R12.0_2-Unofficial-X6525.img` (100 MB).

```bash
gh release download <run_id> --repo Mooments1/android_device_infinix_x6525_recovery \
  --dir ~/Downloads/OrangeFox-X6525
adb reboot fastboot
fastboot flash vendor_boot_a ~/Downloads/OrangeFox-X6525/OrangeFox-*.img
fastboot reboot recovery
```

AVB must be disabled. Known issue: `/data` decryption does not work.
