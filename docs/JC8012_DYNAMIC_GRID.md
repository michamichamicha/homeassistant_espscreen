# JC8012P4A1 Dynamic Grid

Grid dimensions are now a board profile rather than a fixed application-wide
constant. Existing boards keep their 2 × 3 profile; the JC8012P4A1 profile uses
4 × 4 slots per page.

## Goal

Keep the existing boards unchanged:

- CYD: 2 columns × 3 rows per page
- 4-inch Guition: 2 columns × 3 rows per page

Add a JC8012P4A1-specific grid:

- 4 columns × 4 rows per page
- Up to 16 ordinary tile slots per page

The ESP Screen Manager protocol should remain compatible with existing firmware
and layouts.

## Current constraints

The geometry is supplied by several layers:

- `components/smart_display/runtime_model.h` derives slots from `GRID_COLUMNS`,
  `GRID_ROWS`, and `GRID_MAX_PAGES` build flags (defaulting to 2 × 3 × 8).
- `components/smart_display/runtime_tiles.h` allocates the board's maximum
  runtime tile capacity and derives wide/full placement from those dimensions.
- Board YAML files bind only ten runtime tile widgets and position two columns.
- `web/src/model/layout.ts` switches its active profile when a screen is selected.
- `web/src/components/DevicePage.vue` renders the selected profile's rows and columns.
- Manager and editor validation use the paired screen's profile.

## Adding another board

Add the board to `GRID_PROFILES` in `screen_manager/app/core.py`, make discovery
report that board name, and add the matching profile to the board's `Screen`
metadata. In the firmware profile, set:

```yaml
substitutions:
  GRID_COLUMNS: "4"
  GRID_ROWS: "4"
  GRID_MAX_PAGES: "8"
```

Pass those substitutions as `-DGRID_COLUMNS`, `-DGRID_ROWS`, and
`-DGRID_MAX_PAGES` build flags, bind one runtime tile widget for every slot,
and generate the package with `tools/generate_packages.py`.

The existing 2 × 3 profile should remain the default so current boards and saved
layouts retain their behavior. The JC8012 profile should provide 4 × 4 geometry
and enough LVGL widgets for its slots.

The manager must identify the board before applying the corresponding tile limit
and layout validation. Existing layouts must not be silently reflowed when a
screen is rediscovered.

## Work checklist

- [x] Define a shared grid-profile shape and preserve the existing 2 × 3 default.
- [x] Generalize firmware placement, page rendering, wide tiles, and full-page tiles.
- [x] Generalize editor placement, drag/drop, empty-slot rendering, and page counts.
- [x] Make manager validation and tile limits board-aware.
- [x] Add regression coverage for both 2 × 3 and 4 × 4 profiles.
- [ ] Test compact-card readability and touch hitboxes on physical hardware.
- [ ] Test standard, wide, full-page, control, camera, and navigation tiles.

## Acceptance criteria

Existing CYD and 4-inch Guition layouts render exactly as before. A JC8012P4A1
can display and edit a 4 × 4 page, persist explicit slot positions, navigate
between pages, and use all supported tile types without affecting existing
boards.
