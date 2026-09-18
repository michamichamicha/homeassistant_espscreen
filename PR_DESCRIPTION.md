## Add JC8012P4A1 10.1-inch display support

### Summary

Adds support for the Guition JC8012P4A1 10.1-inch ESP32-P4 touchscreen display.

The new board uses:

- ESP32-P4 with 16 MB flash
- ESP32-C6 hosted Wi-Fi
- 1280 × 800 MIPI-DSI display
- GSL3680 capacitive touchscreen
- Hardware backlight and display reset control
- Runtime rotation support

### Changes

- Added `jc8012p4a1.yaml` board profile.
- Added generated `packages/jc8012p4a1.yaml`.
- Added JC8012P4A1 support to package generation and installer workflows.
- Added board discovery and runtime handling in ESP Screen Manager.
- Added JC8012-specific:
  - Camera dimensions
  - Alert limits
  - Rotation handling
  - Guition-style editor layout
  - Installer UI option
- Added vendored MIPI-DSI and GSL3680 ESPHome components compatible with ESPHome `2026.6.2`.
- Added hardware/package tests.
- Updated documentation, changelog, add-on version, and frontend assets.
- Added third-party licensing and attribution documentation:
  - `THIRD_PARTY_NOTICES.md`
  - Component-level licensing notes and preserved upstream license files.

### Licensing

The vendored components retain their upstream licenses:

- `components/mipi_dsi/`
  - ESPHome license
  - Python portions under MIT
  - C/C++ portions under GPLv3
- `components/gsl3680/`
  - PolyForm Noncommercial License 1.0.0
  - Copyright notice preserved: `Copyright (c) 2026 jtenniswood`

These components are not relicensed under the repository’s top-level MIT license.

### Validation

- ESPHome configuration validation passed.
- JC8012P4A1 firmware compilation passed using ESPHome `2026.6.2`.
- Connected hardware was detected successfully as ESP32-P4 revision v1.3.
- Firmware was flashed and tested on a physical JC8012P4A1.
- Display initialization and landscape orientation work.
- Touch input works through the GSL3680 controller.
- Runtime rotation was tested successfully.
- Package generation checks passed.
- Targeted Python tests passed.
- Frontend tests, type-checking, and production build passed.

### Testing notes

The following still require broader physical acceptance testing:

- Long-term touchscreen reliability
- All tile/control types
- Camera tiles
- Alerts
- Sleep/wake behavior
- Wi-Fi/API pairing across different Home Assistant installations
- OTA updates

### Related follow-up

Dynamic grid support for the JC8012P4A1 is intentionally deferred. Existing boards remain on their current 2×3 grid layout. Future work is documented in:

```text
docs/JC8012_DYNAMIC_GRID.md
```
