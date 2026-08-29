# YouTube Content — Voron 2.4 + Leviathan V1.3

Three upload packages, each one long-form video plus three Shorts. Everything is
written from the config that's actually in this repo — every number in every script
traces back to a file, and each script ends with a fact-check table saying which one.

## The three uploads

| # | Upload | Runtime | Shorts | Core idea |
|---|--------|---------|--------|-----------|
| [01](01-leviathan-board-tour/) | Leviathan V1.3 full board tour + wiring | 12–14 min | 3 | One board runs the whole machine |
| [02](02-cartographer-touch-calibration/) | Cartographer touch calibration | 10–12 min | 3 | The calibration *order* is the fix |
| [03](03-klipper-macros-that-work/) | Klipper macros that earn their keep | 11–13 min | 3 | One button, because order you can't forget beats order you remember |

They're a series and they refer forward and back, but each one works cold — someone
arriving at 02 from search never needs to have seen 01.

## What's in each folder

| File | Contents |
|------|----------|
| `script.md` | Full narration with timecodes, on-screen text, b-roll cues, shot list, and a fact-check table |
| `shorts.md` | Three vertical scripts with beat timing and captions, plus a posting plan |
| `metadata.md` | Titles (primary + A/B alternates), description with chapters, tags, thumbnail spec, pinned comment, end screen, upload settings |

Plus [`upload-checklist.md`](upload-checklist.md) — the run-through for publishing day.

## The machine these are about

| | |
|---|---|
| Printer | Voron 2.4, 350 × 350, CoreXY |
| Mainboard | Leviathan V1.3 (`stm32h743xx`) |
| Toolhead | LDO Nitehawk Jabberwocky (RP2040) |
| Probe | Cartographer 3D scanner, touch mode, threshold 3500 |
| X/Y | TMC5160, 0.9° motors, `rotation_distance: 40`, 32 microsteps |
| Z0–Z3 + E | TMC2209 |

## Before you record: known issues

All of these are called out in the relevant script's fact-check or blocker section,
but they belong in one place, because every one of them would be visible on camera.

### Blocker — Upload 03 can't be filmed as written until this is resolved

**The chamber lighting macros address an LED object that doesn't exist.**
`macros.cfg` drives them with `SET_LED LED=chamber_leds` (5 places), but the only
mainboard strip defined is `[neopixel case_leds]` (`voron2_leviathan.cfg:349`, 3
pixels on PF10). `PRINT_START` calls `CHAMBER_LIGHTS_PRINT` at line 223 — so on this
config as committed, Klipper raises `Unknown LED 'chamber_leds'` and PRINT_START
aborts. That's the exact one-button sequence Upload 03 opens on.

Either the object is defined in a file that isn't committed (`nhk_diag.cfg` is the
likely candidate — see below), or it's broken. Nothing in this content package
changes the config; that's a decision for the machine's owner, not the scripts.

Related: `CHAMBER_RAINBOW` loops 25 indices against that 3-pixel chain, and
`macros_documentation.md` describes it as "across 25 LEDs" — doc, macro and hardware
disagree three ways. The `chain_count: 3  # Reduced for testing` comment suggests the
strip was scaled down and neither the macro nor the doc followed.

### Doc drift — correct on camera or fix first

1. **`README.md` lists STM32F446** in the front-page spec table — that's the earlier
   board revision. The V1.3 in this build enumerates as `stm32h743xx`. Upload 01
   addresses this out loud rather than pretending it isn't there.
2. **`Klipper_config/Reference/pin_mappings.md` lists X/Y endstops as `nhk:gpio13` /
   `nhk:gpio12`**, but the live config homes X on the board pin `PC1`. Reconcile that
   doc before showing it on screen.
3. **`voron2_leviathan.cfg:10` includes `nhk_diag.cfg`, which isn't in the repo.**
   It exists on the Pi (Klipper wouldn't start otherwise) — commit it or drop the
   include before pointing viewers at this config. It's also the most likely home for
   a missing `chamber_leds` definition, so check there first.

## House rules these scripts follow

- **Every number is sourced.** If it isn't in a config file in this repo, it isn't
  stated as fact on camera.
- **Say what's yours.** Serial IDs, threshold values and PID numbers are
  machine-specific; each script says so at the moment it shows one.
- **Credit borrowed work.** The `PRINT_END` safe-move logic is a community Voron
  macro and Upload 03 says so twice.
- **Don't fake results.** Speed-ramp the waits, never cut a sequence to imply it
  worked first time.
- **Captions on every Short.** This audience watches muted.
