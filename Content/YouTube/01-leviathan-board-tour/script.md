# Upload 01 — Long-form Script
## "The Leviathan V1.3 Runs My Whole Voron 2.4 — Full Board Tour + Wiring"

**Target runtime:** 12–14 min
**Format:** 16:9, talking head + overhead bench cam + screen capture
**Assets referenced:** all paths are relative to the repo root (`/Media`, `/Klipper_config`)

---

### 0:00–0:35 — Cold open (hook)

> **B-roll:** `Media/front.jpg` slow push-in, then hard cut to the printer running.

**VO:**
"Seven stepper drivers. Two of them rated for forty-eight volts. Four fan channels
with tach feedback, a hotend channel good for a hundred and eighty watts, a bed
channel good for two-forty — and a mounting pad for the Raspberry Pi that runs the
whole thing. This is one board. It replaced four things in my Voron 2.4.

In the next twelve minutes I'm going to walk you around every connector on the
Leviathan V1.3, show you exactly how mine is wired, and show you the config that
makes it work. If you're deciding whether to put one of these in your build,
this is the video I wanted before I did."

> **On-screen text (0:05):** LEVIATHAN V1.3 · VORON 2.4 · 350mm
> **On-screen text (0:28):** Full wiring + config → repo link in description

---

### 0:35–1:10 — Who this is for

**VO:**
"Quick context so you know whether to keep watching. My machine is a Voron 2.4,
350 by 350. The toolhead is an LDO Nitehawk Jabberwocky, the probe is a
Cartographer scanner, and the mainboard is the Leviathan V1.3. Everything I show
you is running config that's public — it's in the repo linked below, and I'll put
the exact file name on screen every time I open something."

> **On-screen text:** Voron 2.4 · 350mm · Nitehawk Jabberwocky · Cartographer · Leviathan V1.3

---

### 1:10–3:30 — Board tour, front side

> **B-roll:** `Media/front_no_heatsink.jpg` and `Media/overview.jpg` as stills with
> callout arrows; live overhead shot of the board on the bench.

**VO — beat by beat, one callout per connector group:**

1. **Main power in + HV stepper in.** "Two separate supply inputs, both reverse-polarity
   protected. Main rail is eighteen to twenty-eight volts — twenty-four is the normal
   answer. The HV rail for the two big drivers takes twenty-four up to fifty-five,
   and forty-eight is what it's designed around."
2. **The stepper drivers.** "Five integrated TMC2209s and two integrated TMC5160s.
   Integrated — you're not plugging in driver modules, and you're not losing one to
   a bad seat at two in the morning. The 5160s are the forty-eight volt pair."
3. **Heaters.** "Hotend channel, one-eighty watts. Bed channel, two-forty. Both fused."
4. **Fans.** "Four fan ports. Selectable five or twenty-four volt, flyback protection,
   and tachometer inputs — which matters more than people think, and I'll come back
   to that."
5. **Thermistors.** "Four ports, twenty-two hundred ohm pullups."
6. **Probe / endstops / filament.** "Dedicated Z probe port with selectable voltage and
   an integrated diode, three endstop ports, and a filament sensor port."
7. **LEDs.** "One dedicated LED strip channel with flicker-free dimming down to one
   percent, plus a Neopixel port."

> **On-screen text (as each is named):** 5× TMC2209 · 2× TMC5160 (48V) · 180W hotend ·
> 240W bed · 4× fan + tach · 4× thermistor

**VO (button):**
"The thing that sold me is the boring one: passive cooling done properly. Big
heatsinks, mounting done right, no whiny 40mm fan bolted to a driver."

> **B-roll:** `Media/back.jpg`, `Media/back_no_heatsink.jpg`

---

### 3:30–5:00 — The Raspberry Pi mount

> **B-roll:** `Media/rpi4_installed_V1.3.jpg`, `Media/rpi4_usb_installed_V1.3.jpg`,
> `Media/rpi5_installed_V1.3.jpg`, `Media/rpimounting.jpg`

**VO:**
"There's a dedicated pad for a Pi 3, 4, 5, or a Zero 2W, with its own regulated
supply — rated three amps — and a dedicated UART port. So you get two options.

Option one: UART. Pi talks to the MCU over the header, no USB cable in the loop.
Option two: USB, which is what I run. Both are supported and both are documented
in the manual PDF in the repo.

I run USB for one unglamorous reason: when I'm bisecting a problem I want to be able
to unplug one cable and know exactly what I disconnected."

> **On-screen text:** UART or USB — both supported
> **On-screen text:** Mine: USB (easier to isolate when debugging)

---

### 5:00–7:00 — Flashing, and the serial ID that everyone gets wrong

> **B-roll:** screen capture in the Pi terminal; stills `Media/bootloader_led_V1.3.jpg`,
> `Media/katapult_klipper_flash_V1.3.png`, `Media/lsusbklipper_V1.3.png`

**VO:**
"Flashing. I'm not going to re-record the LDO setup guide — it's linked below and
it's good. What I *am* going to show you is the one step that eats people's evenings.

After you flash, you go find the board:

```bash
ls /dev/serial/by-id/
```

You get a string back. That string is unique to *your* board. Mine is:

```
/dev/serial/by-id/usb-Klipper_stm32h743xx_310034000251333236353439-if00
```

Copy yours. Not mine. That long number is the chip's unique ID — if you paste mine
into your config, Klipper will tell you it can't connect and you will spend an hour
being angry at the wrong thing.

One note while it's on screen: the repo's front-page spec table lists an STM32F446 —
that's the earlier revision. The V1.3 I'm holding enumerates as `stm32h743xx`.
Read what your own machine reports; that's the only answer that counts."

> **On-screen text:** `ls /dev/serial/by-id/`
> **On-screen text (red):** COPY *YOUR* ID — NOT MINE

---

### 7:00–10:00 — The config, section by section

> **B-roll:** screen capture of `Klipper_config/Voron2/voron2_leviathan.cfg`

**VO:**
"Here's the actual config. Three MCUs on this machine: the Leviathan, the Nitehawk
toolhead, and the Cartographer.

The mainboard:

```ini
[mcu]  # MAINBOARD  LEVIATHAN_V1.3
serial: /dev/serial/by-id/usb-Klipper_stm32h743xx_310034000251333236353439-if00
restart_method: command
```

The toolhead, which is an RP2040 and gets its own name so I can prefix pins with
`nhk:` later:

```ini
[mcu nhk]  # NITEHAWK JABBERWOCKY TOOLHEAD
serial: /dev/serial/by-id/usb-Klipper_rp2040_3033393834055A9D-if00
baud: 1000000
restart_method: command
```

And the Cartographer, which I do *not* declare as an `[mcu]` — the `[scanner]`
section opens its own connection. Declare it twice and you get a fight over the
port. That's in `cartographer_scanner.cfg` and it's the whole subject of the next
video.

Printer limits:

```ini
[printer]
kinematics: corexy
max_velocity: 300
max_accel: 10000
max_z_velocity: 15
max_z_accel: 350
square_corner_velocity: 5.0
```

Ten thousand on accel is a *ceiling*, not a brag — input shaper decides what I
actually run. Fifteen on Z velocity is the conservative number for twelve-volt Z
drivers; you can push it on twenty-four.

Now X and Y — this is the Leviathan-specific part:

```ini
[stepper_x]
step_pin: PB10
dir_pin: PB11
enable_pin: !PG0
rotation_distance: 40
microsteps: 32
full_steps_per_rotation: 400
endstop_pin: PC1
position_endstop: 350
position_max: 350
```

`full_steps_per_rotation: 400` means point-nine degree motors. X and Y are on the
two TMC5160 high-voltage channels — that's the whole reason to want those channels:
CoreXY A and B motors are the ones that benefit from headroom. The four Z motors are
on TMC2209s, and so is the extruder, out on the toolhead."

> **On-screen text:** X/Y → TMC5160 (HV) · Z0–Z3 + E → TMC2209
> **On-screen text:** `full_steps_per_rotation: 400` = 0.9° motors

---

### 10:00–11:30 — Fans, and why tach is the good feature

> **B-roll:** config capture of the `temperature_fan` sections

**VO:**
"Back to fan tachometers. Two of my fans are temperature-controlled and never touched
by hand:

```ini
[temperature_fan electronics_bay_auto]
pin: PB3
sensor_type: temperature_mcu
target_temp: 45.0

[temperature_fan motherboard_auto]
pin: PF7
tachometer_pin: PF6
sensor_type: temperature_host
target_temp: 50.0
```

The bay fan follows MCU temperature. The board fan follows the *Pi's* temperature —
sensor_type `temperature_host` — because the Pi sits right there in the same air.

And the tach line: the hotend fan on my toolhead reports RPM. That means a dying
fan shows up as a falling number *before* it shows up as a heat-creep clog at layer
four hundred. That's not a spec-sheet feature, that's a Saturday you get back."

> **On-screen text:** hotend fan RPM ↓ = warning *before* the clog

---

### 11:30–13:00 — What I'd tell you before you buy

**VO:**
"Honest summary.

Buy it if: you're running a 2.4 or a Trident, you want the Pi and the board to be
one assembly, you want integrated drivers you can't unseat, and you like the idea of
forty-eight volts on your CoreXY motors.

Think twice if: you need more than one toolhead's worth of ports, or you're on a
CAN-everything build where the mainboard is deliberately dumb.

What I'd do differently: I'd have written down my serial IDs the day I flashed,
instead of re-deriving them every time I rebuilt the Pi.

Next video is Cartographer — touch calibration, the threshold value, and getting a
Z offset that actually repeats. Config's in the repo, link's below. Subscribe if
you want the rest of the series."

> **On-screen text:** Next: Cartographer touch calibration
> **End screen 13:00–13:30:** subscribe + next video card

---

## Shot list

| # | Shot | Source | Notes |
|---|------|--------|-------|
| 1 | Board hero, front | `Media/front.jpg` | cold-open push-in |
| 2 | Board, heatsinks off | `Media/front_no_heatsink.jpg` | driver callouts |
| 3 | Board rear | `Media/back.jpg`, `Media/back_no_heatsink.jpg` | cooling section |
| 4 | Connector overview | `Media/overview.jpg`, `Media/pin_assignment.jpg` | callout arrows |
| 5 | Pi mounted | `Media/rpi4_installed_V1.3.jpg`, `Media/rpi5_installed_V1.3.jpg` | mount section |
| 6 | Pi USB wiring | `Media/rpi4_usb_installed_labels_V1.3.jpg` | USB-vs-UART beat |
| 7 | Bootloader LED | `Media/bootloader_led_V1.3.jpg` | flashing section |
| 8 | Flash terminal | `Media/katapult_klipper_flash_V1.3.png` | screen capture |
| 9 | `ls /dev/serial/by-id/` | live capture | red on-screen warning |
| 10 | Wiring diagram | `Media/wiring_V1.3.jpg` | recap over outro |
| 11 | Live print, chamber lit | new footage | b-roll bed under VO |

---

## Fact-check — source of truth for every claim

Everything numeric above traces to a file in this repo. Re-verify before recording.

| Claim in script | Source |
|---|---|
| 5× TMC2209, 2× TMC5160, 180W hotend, 240W bed, 4 fans, 4 thermistors | `README.md` feature list + electrical table |
| Main 18–28V, HV 24–55V, Pi out 3A | `README.md` electrical specification table |
| Mainboard serial `usb-Klipper_stm32h743xx_310034000251333236353439-if00` | `Klipper_config/Voron2/voron2_leviathan.cfg:24` |
| NHK serial + `baud: 1000000` | `voron2_leviathan.cfg:29-31` |
| Printer limits (300 / 10000 / 15 / 350 / 5.0) | `voron2_leviathan.cfg` `[printer]` |
| `[stepper_x]` pins, rotation 40, microsteps 32, 400 full steps, endstop PC1 | `voron2_leviathan.cfg` `[stepper_x]` |
| X/Y on TMC5160, Z0–Z3 + extruder on TMC2209 | `voron2_leviathan.cfg` `[tmc5160 …]` / `[tmc2209 …]` sections |
| `electronics_bay_auto` PB3 @ 45 °C, `motherboard_auto` PF7 tach PF6 @ 50 °C | `Klipper_config/Voron2/macros.cfg` |
| Cartographer is not declared as `[mcu]` | `Klipper_config/Voron2/cartographer_scanner.cfg` (commented out, with reason) |

**Known doc drift — say the config value, not the doc value:**
- `README.md` lists **STM32F446**; this build's V1.3 reports **stm32h743xx**. Script
  addresses this out loud rather than ignoring it.
- `Klipper_config/Reference/pin_mappings.md` lists X/Y endstops as `nhk:gpio13` /
  `nhk:gpio12`. The live config homes X on the **board** pin `PC1`. Don't read the
  reference doc on camera until it's reconciled.
