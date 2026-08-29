# Upload 02 — Metadata

## Titles

**Primary:**
`Cartographer Z Offset That Actually Repeats — Do These 4 Steps In Order`

**Alternates (A/B these):**
- `You're Calibrating Cartographer In The Wrong Order (Voron 2.4)`
- `Cartographer Touch Calibration: Threshold, QGL, Bed Mesh — Full Setup`
- `The Z Offset Fix Nobody Tells You About (Cartographer + Klipper)`

The order idea is the hook. Lead with it — "in order" and "wrong order" both test
well and both survive mobile truncation.

---

## Description

```
My first layer used to be a coin flip. It wasn't the probe and it wasn't the bed —
it was the order I was running the calibration steps in. Quad gantry level moves
your Z, so anything you calibrated before it is already stale.

Here's the whole thing: the four commands in the order that works, what
scanner_touch_threshold actually does, my full bed mesh config, and the four failure
modes with what each one really means.

THE ORDER
  G28
  QUAD_GANTRY_LEVEL
  G28 Z
  G1 X175 Y175 F6000
  CARTOGRAPHER_TOUCH

MY SETUP
• Printer — Voron 2.4, 350×350
• Mainboard — Leviathan V1.3
• Toolhead — LDO Nitehawk Jabberwocky
• Probe — Cartographer 3D scanner, touch mode, threshold 3500

CHAPTERS
0:00 You're doing it in the wrong order
0:30 What a scanner is actually measuring
1:20 My full [scanner] config
2:40 scanner_touch_threshold, explained
4:20 The four steps, in order
7:00 Bed mesh: inset, point count, zero reference
9:00 Four failures and what they really mean
10:30 What's next

CONFIG
Every file shown: https://github.com/MotorDynamicsLab/Leviathan

REFERENCE
Cartographer docs — https://docs.cartographer3d.com
Cartographer Klipper — https://github.com/Cartographer3D/cartographer-klipper
Klipper bed mesh — https://www.klipper3d.org/Bed_Mesh.html
Klipper resonance/QGL reference — https://www.klipper3d.org/Config_Reference.html
Ellis' Print Tuning Guide — https://ellis3dp.com/Print-Tuning-Guide/

Threshold values are machine-specific. 3500 is what works on my mount, my plate, my
nozzle. Use it as a starting point, change it one step at a time, and write down
what each value did.

Previous: Leviathan V1.3 full board tour.
Next: the Klipper macros that run this sequence so I can't get the order wrong.

#Voron #Klipper #Cartographer #3DPrinting #Voron24 #BedMesh
```

---

## Tags

```
cartographer 3d, cartographer probe, cartographer touch, scanner touch threshold,
klipper z offset, quad gantry level, qgl not converging, bed mesh klipper, voron 2.4,
voron leviathan, klipper calibration, first layer klipper, zero reference position,
bicubic bed mesh, klipper probe setup, cartographer klipper config, voron first layer,
z offset repeatable, klipper touch calibration, 3d printer calibration order
```

---

## Thumbnail

**Concept:** split frame, hard vertical divide.

- Left: a bad first layer, raking light, gaps visible. Label `BEFORE`.
- Right: a clean first layer, same light, same lens. Label `AFTER`.
- Centred across the seam, one line in the heaviest weight: **ORDER MATTERS**

No face on this one. The two first layers are the argument, and a face competes with
them. Shoot both plates with the same lighting setup on the same day or the
comparison reads as dishonest.

**Text safe area:** nothing in the bottom-right 15%.

---

## Pinned comment

```
The four steps, so you don't have to scrub for them:

  G28
  QUAD_GANTRY_LEVEL
  G28 Z                    ← the one everyone skips
  G1 X175 Y175 F6000
  CARTOGRAPHER_TOUCH

QGL moves your Z. That's why the second G28 Z is there and why offset-before-level
never sticks.

scanner_touch_threshold: 3500 is MY value. Yours depends on your mount, plate and
nozzle — start around 3500, change it one step at a time, write down what each value
did. Post yours below with your mount type and I'll collect them here.

Config: https://github.com/MotorDynamicsLab/Leviathan
```

---

## End screen

- 0:30 hold.
- Left card: Upload 03 (macros).
- Right card: Upload 01 (board tour) for anyone who arrived from search.
- Subscribe watermark throughout.

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
| Publish | Tue or Thu, 09:00 local — 7 days after Upload 01 |
