# Upload 03 — Long-form Script
## "Klipper Macros That Earn Their Keep — PRINT_START, Diagnostics & Status LEDs"

**Target runtime:** 11–13 min
**Format:** 16:9, screen capture + printer beauty shots for the LED sections
**Assets:** `Klipper_config/Voron2/macros.cfg`, `nhk_leds.cfg`, `Klipper_config/Reference/macros_documentation.md`

---

### 0:00–0:35 — Cold open (hook)

> **B-roll:** one continuous shot — hit print in Mainsail, then the machine does
> everything by itself: LEDs go orange, homes, levels, meshes, purges, starts.
> No cuts. The unbroken take *is* the argument.

**VO:**
"I pressed one button. It heated, homed, levelled the gantry, re-homed Z, meshed the
bed, purged, and started printing — and the LEDs told me which of those it was doing
the whole time.

None of that is clever. It's about forty lines of macro. But it means I cannot forget
a step at eleven at night, which is when I forget steps."

> **On-screen text (0:03):** ONE BUTTON
> **On-screen text (0:28):** ~40 lines of macro

---

### 0:35–1:15 — The actual argument for macros

**VO:**
"Most macro videos are somebody's config dump with a voiceover. I want to make a
narrower claim: a macro is worth writing when it stops you making a mistake, or when
it tells you something you'd otherwise have to go look up.

Everything I'm showing you does one of those two things. If a macro doesn't do
either, it's just a shortcut, and shortcuts age badly."

> **On-screen text:** prevents a mistake · or tells you something

---

### 1:15–4:30 — PRINT_START

> **B-roll:** `macros.cfg`, scrolling through `PRINT_START` while narrating.

**VO:**
"Start with the big one.

```ini
[gcode_macro PRINT_START]
gcode:
    {% set BED_TEMP = params.BED_TEMP|default(60)|float %}
    {% set EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(210)|float %}
```

Two parameters with defaults, so the slicer passes real numbers but a bare
`PRINT_START` typed by hand still does something sane instead of erroring.

Then, in order:

```ini
    STATUS_HEATING
    CHAMBER_LIGHTS_PRINT
    M140 S{BED_TEMP}          # start bed heating, don't wait
    STATUS_HOMING
    G28
    STATUS_HEATING
    M190 S{BED_TEMP}          # now wait for the bed
```

`M140` then home then `M190`. Start the bed heating, home while it's coming up, *then*
wait. The homing is free — it happens during time you were going to spend waiting
anyway.

```ini
    STATUS_LEVELING
    QUAD_GANTRY_LEVEL
    G28 Z
    STATUS_MESHING
    BED_MESH_CALIBRATE
```

And there's the sequence from the last video, welded in. QGL, then re-home Z because
QGL moved it, then mesh. I can't get that order wrong anymore because I don't type it
anymore. That's the entire point of this macro.

One deliberate choice: I mesh on *every* print. A saved mesh is faster and there's a
`BED_MESH_LOAD` macro if you want it. I don't, because twenty seconds is cheaper than
one failed twelve-hour print, and my bed moves with temperature like everyone's does.

```ini
    G1 X10 Y10 Z0.3 F3000
    STATUS_HEATING
    M109 S{EXTRUDER_TEMP}
    STATUS_PRINTING
    G92 E0
    G1 X100 E15 F1500
    G1 Y10.5 F1500
    G1 X10 E15 F1500
    G92 E0
```

Move to the corner, *then* bring the hotend to temperature, then purge two lines —
out and back, half a millimetre apart. Heating the nozzle last is on purpose: a hot
nozzle sitting over the plate for four minutes during QGL is how you get a blob and
a mark.

In the slicer, all of this is one line:

```gcode
PRINT_START BED_TEMP=[bed_temperature] EXTRUDER_TEMP=[nozzle_temperature]
```
"

> **On-screen text:** heat → home → wait → QGL → G28 Z → mesh → purge
> **On-screen text (highlight):** nozzle heats LAST

---

### 4:30–6:00 — PRINT_END, and the maths worth stealing

**VO:**
"`PRINT_END` has the one genuinely clever bit in my whole config, and I didn't write
it — it's from the Voron community and it's everywhere for good reason:

```ini
    {% set th = printer.toolhead %}
    {% set x_safe = th.position.x + 20 * (1 if th.axis_maximum.x - th.position.x > 20 else -1) %}
    {% set y_safe = th.position.y + 20 * (1 if th.axis_maximum.y - th.position.y > 20 else -1) %}
    {% set z_safe = [th.position.z + 2, th.axis_maximum.z]|min %}
```

Read what that does. It moves twenty millimetres away from the print — but it checks
whether there's twenty millimetres left in that direction first, and goes the other
way if there isn't. And it lifts Z by two, unless two would exceed the machine's
maximum, in which case it goes to the maximum.

A naive `G1 X+20` crashes into the gantry when your print finishes near the edge.
This one can't. That's a macro doing the thing I said at the top: preventing a
mistake you'd only make occasionally, which is the worst kind."

> **On-screen text:** move away — unless there's no room, then the other way
> **On-screen text:** lift 2mm — clamped to axis_maximum

---

### 6:00–8:00 — The diagnostics

**VO:**
"Two macros I run before anything long.

```ini
[gcode_macro TOOLHEAD_DIAG]
description: Blink ACT LED, check fan RPM, probe status, and MCU temp
gcode:
    RESPOND PREFIX=toolhead MSG="🔧 TOOLHEAD DIAG START"
    SET_PIN PIN=act_led VALUE=1
    QUERY_FAN fan
    QUERY_FAN hotend_fan
    QUERY_PROBE
    QUERY_ADC name=nhk_mcu_temp
    SET_PIN PIN=act_led VALUE=0
    RESPOND PREFIX=toolhead MSG="✅ TOOLHEAD DIAG COMPLETE"
```

Blink the activity LED so I know I'm talking to the right board — sounds trivial,
isn't, the first time you have two toolhead boards on the bench. Then part fan RPM,
hotend fan RPM, probe state, MCU temperature.

That's four things I'd otherwise check in four places, and it's the fan RPM that
earns it. My hotend fan reports tach. A number that's drifting down over weeks is a
bearing going, and I'd rather find that here than at layer four hundred.

Second one:

```ini
[gcode_macro CHECK_UART_HEALTH]
gcode:
    RESPOND MSG="📡 Dumping TMC driver status"
    DUMP_TMC stepper_x
    DUMP_TMC stepper_y
    DUMP_TMC stepper_z
    DUMP_TMC stepper_z1
    DUMP_TMC stepper_z2
    DUMP_TMC stepper_z3
    DUMP_TMC extruder
    RESPOND MSG="✅ UART check complete"
```

Seven drivers, one command. I run it after every config change that touches motion.
If a driver isn't answering on UART it shows up here, in ten seconds, instead of as a
layer shift in the third hour.

Both of these use `RESPOND` so the output is labelled in the console. When you're
reading back through a log at midnight, a line that says `toolhead: ✅` is worth
more than perfect terse output."

> **On-screen text:** TOOLHEAD_DIAG — 4 checks, one command
> **On-screen text:** CHECK_UART_HEALTH — 7 drivers, one command

---

### 8:00–10:00 — Status LEDs: the printer telling you what it's doing

> **B-roll:** the machine cycling through states, shot in a dark room, tight on the
> toolhead logo LEDs.

**VO:**
"Every macro I've shown you calls something like `STATUS_HEATING` or
`STATUS_LEVELING`. Those are LED effects on the toolhead — three GRBW Neopixels,
split into logo and nozzle:

- heating — orange, breathing
- homing — green
- levelling — purple
- meshing — lime
- printing — red-orange gradient
- cooling — blue
- ready — rainbow

This is not decoration, and I'll defend that. I can look at the printer from across
the room and know whether it's still levelling or has moved on to meshing, without
opening a tab. Purple means I've got about ninety seconds. Rainbow means come get
your part.

The effects live in `nhk_leds.cfg` and they need the LED effects plugin —
julianschill's, linked below. If static colours work and effects don't, you haven't
installed the plugin or you haven't restarted Klipper since you did.

The toolhead strip itself is three GRBW pixels on `nhk:gpio7`:

```ini
[neopixel jw_leds]
pin: nhk:gpio7
chain_count: 3
color_order: GRBW
```

Three. Not a hundred. You do not need many LEDs for this to be useful — you need them
where your eye already goes.

The case lighting is a second strip off the mainboard, and it's macro'd for the same
reason: `CHAMBER_LIGHTS_PRINT` while it's working, `CHAMBER_LIGHTS_DIM` at night,
`CHAMBER_LIGHTS_ON` at the end so I can actually see the part I'm inspecting."

> **⚠️ SEE BLOCKER BELOW — do not record this beat until the chamber LED naming is
> reconciled. The chamber macros currently address an LED object that isn't defined.**


> **On-screen text:** orange=heat · green=home · purple=level · lime=mesh · rainbow=done
> **On-screen text:** 3 LEDs. That's all it takes.
> **On-screen text:** needs klipper-led_effect (link in description)

---

### 10:00–11:30 — Steal these four

**VO:**
"If you take nothing else, take these.

**One:** put your calibration order inside `PRINT_START`. Order you can't get wrong
beats order you have to remember.

**Two:** steal the safe-move maths from `PRINT_END`. It's six lines and it prevents a
crash you'll otherwise hit once a year.

**Three:** write one diagnostic macro that answers 'is the machine okay' in one
command. Mine is `TOOLHEAD_DIAG`. Yours should check whatever *your* machine breaks.

**Four:** use `RESPOND` with a prefix in every macro. Future you is reading a log at
midnight and needs to know which macro said what.

Full config's in the repo, link below. That's the series — board tour, calibration,
macros. If there's a part of this setup you want pulled apart in more depth, put it
in the comments."

> **On-screen text (card, hold 5s):** 1. order inside PRINT_START · 2. steal the safe-move
> maths · 3. one diagnostic command · 4. RESPOND with a prefix
> **End screen 11:30–12:00**

---

## Shot list

| # | Shot | Source | Notes |
|---|------|--------|-------|
| 1 | One-button start, unbroken | new footage | cold open — do NOT cut it |
| 2 | `macros.cfg` PRINT_START | screen capture | scroll at reading speed |
| 3 | Bed heating + homing together | new footage | shows the overlap |
| 4 | QGL → G28 Z → mesh | new footage | speed-ramped |
| 5 | Purge lines, macro lens | new footage | slow motion, 120fps |
| 6 | PRINT_END safe move | new footage | print finished near the edge, on purpose |
| 7 | `TOOLHEAD_DIAG` console | screen capture | full output visible |
| 8 | ACT LED blink | macro on toolhead | matched to the console line |
| 9 | LED state cycle | new footage, dark room | the money shot of this video |
| 10 | `CHAMBER_RAINBOW` | new footage | outro under the CTA |

---

## Fact-check — source of truth

| Claim | Source |
|---|---|
| `PRINT_START` params + defaults (60 / 210) | `Klipper_config/Voron2/macros.cfg` `[gcode_macro PRINT_START]` |
| Full start order: M140 → G28 → M190 → QGL → G28 Z → mesh → purge | same macro, verbatim |
| Purge: `G1 X100 E15 F1500` / `G1 Y10.5` / `G1 X10 E15 F1500` | same macro |
| `PRINT_END` safe-move Jinja (x_safe / y_safe / z_safe) | `macros.cfg` `[gcode_macro PRINT_END]` |
| `TOOLHEAD_DIAG` body and description | `macros.cfg` lines 5–17 |
| `CHECK_UART_HEALTH` dumps 7 drivers | `macros.cfg` lines 27–38 |
| LED effect names / states | `Klipper_config/Voron2/nhk_leds.cfg` (`jw_logo_*`, `jw_nozzle_*`, `rainbow`) |
| 3 toolhead LEDs, GRBW | `Klipper_config/Reference/nhk_notes.md` + `nhk.cfg` |
| Chamber macros (`CHAMBER_LIGHTS_ON/OFF/DIM/PRINT`) | `macros.cfg` lines 159–177 |
| `[neopixel jw_leds]` — `nhk:gpio7`, chain_count 3, GRBW | `nhk_leds.cfg:61` |
| LED colour values (heating 1/0.18/0 orange, homing 0/0.6/0.2 green, levelling 0.5/0.1/0.4 purple, meshing 0.2/1.0/0 lime, cooling 0/0/1 blue) | `nhk_leds.cfg` `[led_effect jw_logo_*]` layers |
| LED effects plugin requirement | `Klipper_config/Reference/troubleshooting.md` → LED issues |

## ⚠️ Blockers — fix these before recording

**1. The chamber macros address an LED that isn't defined. This breaks the cold open.**

`macros.cfg` drives the chamber lighting with `SET_LED LED=chamber_leds` (lines 162,
167, 172, 177, 209). There is no `chamber_leds` object anywhere in this repo — the
mainboard strip is defined as:

```ini
[neopixel case_leds]     # voron2_leviathan.cfg:349
pin: PF10
chain_count: 3  # Reduced for testing
color_order: GRBW
```

`PRINT_START` calls `CHAMBER_LIGHTS_PRINT` on line 223, so on this config as
committed, Klipper raises `Unknown LED 'chamber_leds'` and **`PRINT_START` aborts** —
which is precisely the one-button sequence this video opens on. Either the object is
defined in a file that isn't committed (see blocker 2), or it's broken.

Resolve it one way or the other before you shoot:
- If the strip really is `case_leds`, rename the target in those five lines.
- If `chamber_leds` is defined on the Pi, commit that file so viewers can reproduce it.

**2. `CHAMBER_RAINBOW` loops 25 indices against a 3-LED chain.**

```ini
{% for i in range(25) %}    # macros.cfg:180-210
    SET_LED LED=chamber_leds ... INDEX={i+1}
```

`chain_count` is 3. Indices 4–25 have nothing to address, and
`Klipper_config/Reference/macros_documentation.md` describes this macro as "animated
rainbow effect across 25 LEDs" — so the doc, the macro and the hardware currently
disagree three ways. The `chain_count: 3  # Reduced for testing` comment suggests the
strip was scaled down and the macro wasn't. Don't put this on camera until the
number matches the hardware.

**3. `voron2_leviathan.cfg` line 10 includes a file that isn't in the repo.**

Line 10 has `[include nhk_diag.cfg]` and there's no `nhk_diag.cfg` here — it exists on the Pi or Klipper wouldn't
start. Either commit it or drop the include before you put that file on screen; a
viewer copying this config hits an error on a file that doesn't exist. (This is also
the most likely home for a missing `chamber_leds` definition — check there first.)
