# OrangeFox для X6525

Исходное дерево: https://github.com/Kyros70/android_device_infinix_x6525_recovery (ветка `OFOX`).

## Ветки этого форка

- `OFOX` — оригинальное дерево Kyros70 + workflow'ы (первый успешный билд `fox_12.1`).
- `OFOX-Mooments1` — патч 1: убран несуществующий `pb_X6525.mk` из `AndroidProducts.mk`, брендинг `TW_DEVICE_VERSION` / `OF_MAINTAINER` заменён на `Mooments1`.
- `OFOX-16` — патч 3: адаптация дерева под `fox_16.0` (убрана ссылка на отсутствующий в Android 16 `gsi_keys.mk`, добавлены совместимые с OF16 переменные).

## Workflow'ы

- `build-ofox.yml` — проверенный `fox_12.1`, target `vendorboot`.
- `build-ofox16.yml` — `fox_16.0` через минимальный манифест `OrangeFox16/platform_manifest_twrp_aosp` (`twrp-16`) и `OrangeFox16/sync`, JDK 17, target `vendorboot`.

Известный баг обоих сборок: дешифрование `/data`.
