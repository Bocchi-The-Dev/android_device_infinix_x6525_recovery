# OrangeFox for Infinix Smart 8 (X6525)

Unofficial OrangeFox **R12.0_2** as `vendor_boot.img` for X6525 / UMS9230.
Recovery lives in `vendor_boot` (A/B, virtual A/B, header v4), so the build target is `vendorboot`.

## Sources

| Source | Ref |
|---|---|
| [Device tree (original)](https://github.com/Kyros70/android_device_infinix_x6525_recovery) | `OFOX` |
| [Device tree (patched)](https://github.com/Mooments1/android_device_infinix_x6525_recovery) | `OFOX` |
| [OrangeFox sync script](https://gitlab.com/OrangeFox/sync) | `master` (v034, `R12.0`) |
| [TWRP minimal manifest](https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp) | `twrp-12.1` |
| [OrangeFox recovery](https://gitlab.com/OrangeFox/bootable/Recovery) | `fox_12.1` |
| [OrangeFox vendor](https://gitlab.com/OrangeFox/vendor/recovery) | `main` |
| [graphics_drm.cpp patch](https://github.com/Kyros70/android_bootable_recovery) | `android-12.1` |

## Build

Actions → **OrangeFox [OFRP]** → Run workflow with `MANIFEST_BRANCH=12.1`, `DEVICE_TREE_BRANCH=OFOX`, `BUILD_TARGET=vendorboot` (other inputs stay default).

```bash
gh workflow run build-ofox.yml --repo Mooments1/android_device_infinix_x6525_recovery \
  -f MANIFEST_BRANCH=12.1 \
  -f DEVICE_TREE=https://github.com/Mooments1/android_device_infinix_x6525_recovery \
  -f DEVICE_TREE_BRANCH=OFOX \
  -f DEVICE_NAME=X6525 -f DEVICE_PATH=device/infinix/X6525 \
  -f BUILD_TARGET=vendorboot -f LDCHECK=false -f LDCHECKPATH=system/bin/qseecomd
```

The workflow syncs `fox_12.1`, clones the tree, applies the graphics fix, runs
`lunch twrp_X6525-eng && mka adbd vendorbootimage`, then publishes the image to a Release.

## Credits

- [**OrangeFox Recovery Project**](https://orangefox.pro) — recovery source, vendor and sync scripts (base `R12.0`).
- [**Kyros70**](https://github.com/Kyros70) — original X6525 device tree (`OFOX`) and the `graphics_drm.cpp` fix.
- [**Bocchi-The-Dev**](https://github.com/Bocchi-The-Dev) — GitHub Actions workflow.
