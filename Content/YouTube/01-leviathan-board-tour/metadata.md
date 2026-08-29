# Upload 01 — Metadata

## Titles

**Primary:**
`This One Board Runs My Entire Voron 2.4 — Leviathan V1.3 Full Tour`

**Alternates (A/B these):**
- `Leviathan V1.3: 7 Stepper Drivers, 48V, and a Raspberry Pi Mount`
- `I Replaced 4 Components With One Board — Voron 2.4 + Leviathan V1.3`
- `Leviathan V1.3 Wiring & First Boot (Voron 2.4, Start to Finish)`

Keep the board name and the printer name in the first 45 characters — that's what
survives truncation on mobile.

---

## Description

```
The Leviathan V1.3 runs everything on my Voron 2.4: seven integrated stepper
drivers, both heaters, four fans with tachometer feedback, and the Raspberry Pi
that drives it all. Here's the full board tour, how mine is wired, and the exact
Klipper config — including the one flashing step that costs people an evening.

My machine:
• Printer — Voron 2.4, 350×350
• Mainboard — Leviathan V1.3
• Toolhead — LDO Nitehawk Jabberwocky
• Probe — Cartographer 3D scanner

CHAPTERS
0:00 Seven drivers, one board
0:35 Who this video is for
1:10 Board tour: every connector
3:30 The Raspberry Pi mount (UART vs USB)
5:00 Flashing — and the serial ID everyone gets wrong
7:00 The config, section by section
10:00 Fan tachometers: the underrated feature
11:30 Should you buy one?

CONFIG + WIRING
Every file shown in this video is public:
https://github.com/MotorDynamicsLab/Leviathan

Official setup guide (LDO):
https://ldomotion.com/p/guide/VORON-Leviathan-V12

REFERENCE
Klipper config reference — https://www.klipper3d.org/Config_Reference.html
Voron docs — https://docs.vorondesign.com
LDO Nitehawk docs — https://docs.ldomotors.com/en/voron/nitehawk-sb
Cartographer docs — https://docs.cartographer3d.com

Correction noted in-video: the repo's front-page spec table lists an STM32F446 for
the earlier board revision. The V1.3 in this build enumerates as stm32h743xx. Always
read what your own board reports.

Next in this series: Cartographer touch calibration — getting a Z offset that
actually repeats.

#Voron #Klipper #3DPrinting #Voron24 #Leviathan #CoreXY
```

---

## Tags

```
leviathan v1.3, leviathan mainboard, voron 2.4, voron leviathan, klipper config,
klipper mainboard, tmc5160, tmc2209, corexy, ldo motors, nitehawk jabberwocky,
cartographer 3d, klipper flashing, dev serial by id, voron build, 3d printer
electronics, stm32 klipper, raspberry pi klipper, voron wiring, klipper setup guide
```

---

## Thumbnail

**Concept:** the board, shot straight down on a black background, heatsinks off so
the seven driver ICs are visible. Hard side light. Three short callouts in the same
weight, no clutter:

- `7 DRIVERS`
- `48V`
- `ONE BOARD`

Face in the bottom-right corner at ~20% of frame, neutral expression, not shouting.
No red arrows. The board is the hook — let it be the hook.

**Source plate:** `Media/front_no_heatsink.jpg`
**Text safe area:** keep all text out of the bottom-right 15% (duration stamp).

---

## Pinned comment

```
Two things worth pinning:

1) The serial ID at 5:00 is MINE. Run `ls /dev/serial/by-id/` and use whatever
your own board reports — that string contains a per-chip unique ID.

2) The repo's spec table lists STM32F446 for the earlier revision; my V1.3 comes up
as stm32h743xx. If yours reports something else, say so below and I'll add it here.

Config files: https://github.com/MotorDynamicsLab/Leviathan
```

---

## End screen

- 0:30 hold after the outro line.
- Left card: next video in the series (Upload 02, Cartographer).
- Right card: subscribe.
- No third card — two choices convert better than three.

---

## Upload settings

| Setting | Value |
|---|---|
| Visibility | Public |
| Category | Science & Technology |
| Audience | Not made for kids |
| Language | English |
| Comments | On, newest first |
| Playlist | Voron 2.4 + Leviathan build |
| Shorts remixing | Allow |
| Publish | Tue or Thu, 09:00 local |
