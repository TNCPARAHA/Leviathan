# Upload 02 — Long-form Script
## "Cartographer Touch Calibration: A Z Offset That Actually Repeats"

**Target runtime:** 10–12 min
**Format:** 16:9, screen capture heavy, macro lens on the nozzle for the touch beats
**Assets:** `Klipper_config/Voron2/cartographer_scanner.cfg`, `Klipper_config/Reference/calibration_guide.md`

---

### 0:00–0:30 — Cold open (hook)

> **B-roll:** macro shot of the nozzle tapping the bed, three times, cut tight.

**VO:**
"Your first layer isn't inconsistent because your bed is bad. It's inconsistent
because you're running the calibration steps in the wrong order, and one of them
quietly invalidates the one before it.

There's a correct order. It's four commands. And there's one number —
`scanner_touch_threshold` — that decides whether any of it repeats."

> **On-screen text (0:04):** ORDER MATTERS MORE THAN THE PROBE
> **On-screen text (0:22):** scanner_touch_threshold

---

### 0:30–1:20 — What a scanner is actually doing

**VO:**
"Quick model, because it changes how you debug this. An inductive probe or a
microswitch gives you one bit: touched, or not touched. The Cartographer is an
eddy-current scanner — it reads a continuous distance to the bed, so it can sweep a
mesh without stopping to tap.

But for the Z *offset*, I'm running it in touch mode:

```ini
calibration_method: touch
```

which means it physically drives the nozzle down and detects the contact by the
change in signal. That's the mode where the threshold matters."

> **On-screen text:** scan for the mesh · touch for the offset

---

### 1:20–2:40 — The config, all of it

> **B-roll:** `cartographer_scanner.cfg` on screen.

**VO:**
"Here's my entire scanner section. It's short.

```ini
[scanner]
serial: /dev/serial/by-id/usb-Cartographer_614e_230029000443565633393720-if00
x_offset: 0
y_offset: 0
calibration_method: touch
sensor: cartographer
sensor_alt: carto
scanner_touch_threshold: 3500
```

`x_offset: 0`, `y_offset: 0` — mine is a nozzle-coaxial mount, so the probe point
*is* the nozzle point. If yours is offset, put the real numbers there; a wrong offset
is a mesh that's correct and shifted, which is the most confusing failure in this
hobby.

Now the part that trips people. I do **not** declare the Cartographer as an `[mcu]`.
It's commented out in my file, with the reason written next to it:

```ini
# [mcu Cartographer]
# COMMENTED OUT - [scanner] section creates its own MCU connection
```

The `[scanner]` section opens the serial port itself. Declare it in both places and
two things fight over one port. If your scanner connects, then drops, then connects —
check this first."

> **On-screen text (red):** Don't declare `[mcu Cartographer]` — `[scanner]` owns the port

---

### 2:40–4:20 — The threshold number

**VO:**
"`scanner_touch_threshold: 3500`.

That's how much signal change counts as 'I touched the bed.' Too low and it triggers
on noise — you get a phantom touch in the air, and a Z offset that's too high. Too
high and it doesn't register until the nozzle is actually pressing into the plate,
and you scratch a fresh sheet of PEI to learn that.

The usable range on mine is roughly twenty-five hundred to forty-five hundred.
Thirty-five hundred is where I landed and it's been stable.

Here's the honest part. This number is not universal. It depends on your mount, your
bed surface, and your nozzle. Which is why I keep a table:

| Threshold | Result |
|-----------|--------|
| 3500      | current, repeatable |

Two columns, one row so far. Every time you change the value, add a row and write
what happened. Six months from now when you swap the build plate, that table is worth
more than any video."

> **On-screen text:** ~2500–4500 typical · 3500 here
> **On-screen text:** WRITE DOWN WHAT YOU TRIED

---

### 4:20–7:00 — The order. This is the video.

> **B-roll:** live console, running each command in sequence, real timing (speed-ramp
> the waits, never fake the results).

**VO:**
"Four steps. In this order. Every time.

**One — home.**
```
G28
```

**Two — quad gantry level.**
```
QUAD_GANTRY_LEVEL
```
Here's why the order matters: QGL *moves your Z*. Every Z reference you established
before QGL is now wrong. So calibrating your offset first and then levelling is
sanding a board before you cut it.

My QGL config:
```ini
[quad_gantry_level]
retries: 5
retry_tolerance: 0.0075
max_adjust: 10
```
Seventy-five ten-thousandths of a millimetre, up to five tries. If it isn't
converging inside five, stop turning knobs in software — that's belt tension, frame
square, or a Z coupler. Software can't fix a mechanical problem, it can only hide it
until it's expensive.

**Three — home Z again.**
```
G28 Z
```
Because of step two. This is the line everybody skips.

**Four — go to the middle, and touch.**
```
G90
G1 X175 Y175 F6000
CARTOGRAPHER_TOUCH
```
X175 Y175 is the centre of a 350 bed, and it's the same point as my mesh's
`zero_reference_position` — which is deliberate, and I'll show you why in ninety
seconds."

> **On-screen text (full-screen card, hold 4s):**
> `G28` → `QUAD_GANTRY_LEVEL` → `G28 Z` → `CARTOGRAPHER_TOUCH`
> **On-screen text:** QGL moves Z. Everything before it is now stale.

---

### 7:00–9:00 — Bed mesh

**VO:**
"Mesh config:

```ini
[bed_mesh]
speed: 300
horizontal_move_z: 10
mesh_min: 40, 40
mesh_max: 310, 310
fade_start: 0.6
fade_end: 10.0
probe_count: 5,5
algorithm: bicubic
zero_reference_position: 175,175
```

Three things worth pausing on.

`mesh_min: 40,40` and `mesh_max: 310,310` on a 350 bed. That's inset forty millimetres
on every side — not because the bed ends there, but because the *nozzle* can't reach
the corners with the probe where it is. Probe outside what you can reach and Klipper
will tell you, loudly.

`probe_count: 5,5` — twenty-five points. A scanner can do fifty by fifty. I don't,
because with bicubic interpolation on a flat bed, twenty-five points and four hundred
points give me a first layer I can't tell apart, and one of them takes twenty seconds.

`zero_reference_position: 175,175` — the mesh's zero point. Same coordinates I touch
at. That means the point where my offset was measured is the point where my mesh
reads zero. Put those in different places and you've built a constant error into
every print, evenly, everywhere.

Then:
```
BED_MESH_CALIBRATE
BED_MESH_PROFILE SAVE=default
SAVE_CONFIG
```
`SAVE_CONFIG` restarts Klipper. That's expected, not a crash."

> **On-screen text:** 40mm inset = nozzle reach, not bed size
> **On-screen text (highlight):** touch point == zero_reference_position

---

### 9:00–10:30 — When it goes wrong

**VO:**
"Four failures and what they actually mean.

**Mesh comes out different every time.** Bed isn't at temperature. Heat soak it —
the plate moves as it warms, and you're measuring a moving target.

**Touch triggers in mid-air.** Threshold too low, or the sensor face is dirty. Wipe
it. Do that before you touch the number.

**QGL won't converge in five.** Mechanical. Belts, frame, couplers. Do not raise
`retry_tolerance` to make the message go away — that's not fixing it, that's turning
off the alarm.

**Scanner connects then drops.** Back to two forty in this video: you've declared it
twice.

And the meta-rule: change one thing, then test. If you change the threshold and clean
the sensor and re-run QGL, and it works, you've learned nothing and you'll be back
here in a month."

> **On-screen text:** ONE CHANGE AT A TIME

---

### 10:30–11:15 — Close

**VO:**
"So: home, level, home Z, touch. Mesh reference at the same point you touched.
Threshold written down with what it did.

Next video is macros — the `PRINT_START` that runs this whole sequence for me so I
can't forget the order, plus the diagnostics I run before anything long. Config's in
the repo. See you there."

> **End screen 11:15–11:45**

---

## Shot list

| # | Shot | Source | Notes |
|---|------|--------|-------|
| 1 | Nozzle touch, macro | new footage | cold open, three taps |
| 2 | `cartographer_scanner.cfg` | screen capture | scroll slowly, whole file fits |
| 3 | Commented `[mcu Cartographer]` | screen capture | red box overlay |
| 4 | QGL running, 4 corners | new footage | speed-ramp to ~4× |
| 5 | Console during `CARTOGRAPHER_TOUCH` | screen capture | real timing, real output |
| 6 | Mesh heightmap in Mainsail | screen capture | after `BED_MESH_CALIBRATE` |
| 7 | First layer, raking light | new footage | close, over the summary |
| 8 | Threshold table | graphic | build in post |

---

## Fact-check — source of truth

| Claim | Source |
|---|---|
| `calibration_method: touch`, `scanner_touch_threshold: 3500`, offsets 0/0 | `Klipper_config/Voron2/cartographer_scanner.cfg` `[scanner]` |
| `[mcu Cartographer]` commented out because `[scanner]` opens its own connection | same file, lines 6–8 (comment is verbatim in the config) |
| Mesh: speed 300, hmz 10, 40,40 → 310,310, fade 0.6/10.0, 5×5, bicubic, zero ref 175,175 | same file, `[bed_mesh]` |
| QGL: retries 5, retry_tolerance 0.0075, max_adjust 10, speed 150 | same file, `[quad_gantry_level]` |
| Calibration order (G28 → QGL → G28 Z → centre → touch) | `Klipper_config/Reference/calibration_guide.md` §3 |
| Threshold usable range 2500–4500 | `Klipper_config/Reference/cartographer_notes.md` |
| Bed 350×350, centre 175,175 | `voron2_leviathan.cfg` `[safe_z_home]` + `position_max: 350` |

**Before recording:** `cartographer_notes.md` has an empty calibration-history table.
Fill in the current Z offset and date first, then read it on camera — an empty table
on screen reads as a template, not a build log.
