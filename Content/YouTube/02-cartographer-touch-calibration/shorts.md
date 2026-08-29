# Upload 02 — Shorts

Three vertical cuts. The calibration-order one is the flagship — it's the single most
searchable idea in the long video and it works completely on its own.

---

## Short 2A — "Four commands, in this order" (0:47) ★ flagship

**Hook (0:00–0:02)**
On-screen, full frame: **YOU'RE CALIBRATING Z IN THE WRONG ORDER**
Shot: nozzle macro, one tap.

**0:02–0:12**
"Quad gantry level moves your Z. So if you set your Z offset first and then level,
you just threw the offset away. Every time."
> Caption: QGL MOVES Z

**0:12–0:34**
Full-screen card, one line appearing at a time, spoken as they land:
```
G28
QUAD_GANTRY_LEVEL
G28 Z          ← the one everyone skips
CARTOGRAPHER_TOUCH
```
"Home. Level. Home Z *again* — that's the one people skip. Then touch, at the centre
of the bed."

**0:34–0:43**
"Same four commands every time and my first layer stopped being a coin flip."
> Caption: same order, every time

**0:43–0:47 — CTA**
"Full calibration walkthrough and my config — on the channel."

---

## Short 2B — "The number that decides if it repeats" (0:43)

**Hook (0:00–0:02)**
On-screen: **ONE NUMBER DECIDES YOUR Z OFFSET**
Shot: config on screen, `scanner_touch_threshold: 3500` highlighted.

**0:02–0:16**
"`scanner_touch_threshold`. It's how much signal change counts as touching the bed.
Too low, it triggers in mid-air and your nozzle sits too high. Too high, it doesn't
trigger until you're digging into the plate."

**0:16–0:30**
"Usable range is about twenty-five hundred to forty-five hundred. I'm at thirty-five
hundred. But this depends on your mount, your plate, and your nozzle — so it's not
a number you can copy off me."
> Caption: ~2500–4500 · mine: 3500

**0:30–0:39**
"Change it one step at a time and write down what happened. Future you is the one
who needs that note."
> Caption: ONE CHANGE AT A TIME

**0:39–0:43 — CTA**
"Full Cartographer setup — link below."

---

## Short 2C — "Scanner connects, then drops" (0:36)

**Hook (0:00–0:02)**
On-screen: **SCANNER KEEPS DROPPING? YOU DECLARED IT TWICE.**

**0:02–0:16**
"If your Cartographer connects and then disconnects, check this before anything
else: do you have an `[mcu Cartographer]` section *and* a `[scanner]` section?"

**0:16–0:28**
Screen capture, the commented block:
```ini
# [mcu Cartographer]
# COMMENTED OUT - [scanner] section creates its own MCU connection
```
"The `[scanner]` section opens the serial port itself. Declare it in both places and
two things fight over one port. Comment the `[mcu]` one out."

**0:28–0:32**
"Same idea as a circular include — two things claiming one thing."

**0:32–0:36 — CTA**
"More Cartographer config on the channel."

---

## Posting plan

| Short | Post | Purpose |
|---|---|---|
| 2A (order) | day of the long-form upload | flagship; highest evergreen search value |
| 2C (drops) | +2 days | pure problem/solution, pulls people mid-troubleshoot |
| 2B (threshold) | +4 days | deepest, converts best to the long video |

**Note on 2A:** hold the four-line card on screen for at least four seconds at the
end. People screenshot it — that's the point, and screenshots get re-shared.
