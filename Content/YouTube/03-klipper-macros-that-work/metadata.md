# Upload 03 — Metadata

## Titles

**Primary:**
`One Button Starts My Voron — The Klipper Macros That Earn Their Keep`

**Alternates (A/B these):**
- `Klipper Macros That Actually Do Something (PRINT_START, Diagnostics, LEDs)`
- `Steal These 4 Klipper Macros — Voron 2.4 + Leviathan`
- `My PRINT_START Does 8 Things So I Can't Forget Any Of Them`

Avoid "top 10 macros" framing — the video's argument is that most macro lists are
shortcuts, and the title shouldn't undercut it.

---

## Description

```
I press one button and the printer heats, homes, levels the gantry, re-homes Z,
meshes the bed, purges and starts — with the toolhead LEDs telling me which of those
it's doing the whole time. That's about 40 lines of macro.

This isn't a config dump. A macro earns its place if it stops you making a mistake,
or tells you something you'd otherwise go look up. Everything here does one of those.

WHAT'S COVERED
• PRINT_START — the full sequence and why the nozzle heats LAST
• PRINT_END — the safe-move maths that stops a gantry crash (steal this)
• TOOLHEAD_DIAG — fan RPM, probe, MCU temp, in one command
• CHECK_UART_HEALTH — all 7 TMC drivers, one command
• Status LEDs — the printer telling you what it's doing from across the room

CHAPTERS
0:00 One button
0:35 When a macro is actually worth writing
1:15 PRINT_START, line by line
4:30 PRINT_END and the maths worth stealing
6:00 TOOLHEAD_DIAG and CHECK_UART_HEALTH
8:00 Status LEDs
10:00 The four to steal

MY SETUP
• Printer — Voron 2.4, 350×350
• Mainboard — Leviathan V1.3
• Toolhead — LDO Nitehawk Jabberwocky (3× GRBW toolhead LEDs)
• Probe — Cartographer 3D scanner

CONFIG
Every macro shown: https://github.com/MotorDynamicsLab/Leviathan

REFERENCE
Klipper G-Code macros — https://www.klipper3d.org/Command_Templates.html
Klipper config reference — https://www.klipper3d.org/Config_Reference.html
LED effects plugin (julianschill) — https://github.com/julianschill/klipper-led_effect
LDO Nitehawk docs — https://docs.ldomotors.com/en/voron/nitehawk-sb
Voron docs — https://docs.vorondesign.com

The PRINT_END safe-move logic is a community Voron macro, not mine — credit where
it's due, and it belongs in everyone's config.

Series: 1) Leviathan V1.3 board tour  2) Cartographer touch calibration  3) this one.

#Voron #Klipper #3DPrinting #Voron24 #KlipperMacros #Leviathan
```

---

## Tags

```
klipper macros, print_start macro, print_end macro, klipper print start, voron 2.4,
klipper gcode macro, toolhead diagnostics klipper, dump_tmc, tmc uart, klipper led
effects, status leds klipper, klipper-led_effect, nitehawk jabberwocky, voron
leviathan, klipper config, quad gantry level macro, klipper purge line, voron macros,
klipper jinja, 3d printer automation
```

---

## Thumbnail

**Concept:** dark-room shot of the toolhead with the LEDs mid-purple (levelling),
shot slightly below eye level so the logo LEDs read as a face. One overlay line,
bottom third, heavy weight:

**`1 BUTTON. 8 STEPS.`**

Optional small inset, top-left, at ~15% of frame: three lines of the macro, blurred
just enough to read as code without being legible. Nothing else.

This is the strongest thumbnail plate in the series — the LEDs give saturated colour
against black, which is what survives being 120px wide in a sidebar.

**Text safe area:** nothing in the bottom-right 15%.

---

## Pinned comment

```
The four worth stealing, in order of how much trouble they save you:

1. Put your calibration order INSIDE PrintStart — order you can't get wrong beats
   order you have to remember. (G28 → QGL → G28 Z → mesh. QGL moves Z.)
2. The PRINT_END safe-move maths — checks whether there's room before moving, clamps
   the Z lift to axis_maximum. Community Voron macro, not mine.
3. One diagnostic macro that answers "is the machine okay" in one command.
4. RESPOND with a prefix in every macro, so your console log is readable at midnight.

LED effects need julianschill's plugin — if static colours work but effects don't,
you either haven't installed it or haven't restarted Klipper since you did:
https://github.com/julianschill/klipper-led_effect

Config: https://github.com/MotorDynamicsLab/Leviathan
Which part should I pull apart next?
```

---

## End screen

- 0:30 hold.
- Left card: Upload 01 (board tour) — completes the series for new viewers.
- Right card: subscribe.
- Series playlist linked in the description, not on the end screen; two cards only.

---

## Upload settings

| Setting | Value |
|---|---|
| Visibility | Public |
| Category | Science & Technology |
| Audience | Not made for kids |
| Comments | On, newest first |
| Playlist | Voron 2.4 + Leviathan build |
| Shorts remixing | Allow |
| Publish | Tue or Thu, 09:00 local — 7 days after Upload 02 |
