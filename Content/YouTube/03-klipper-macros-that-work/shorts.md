# Upload 03 — Shorts

Three vertical cuts. This upload has the best visual material in the series — the
one-button start and the LED states are both self-explaining without narration, which
is exactly what Shorts reward.

---

## Short 3A — "One button" (0:40) ★ flagship

**Hook (0:00–0:02)**
No text. Just a finger hitting print, then the machine moving.
Text lands at 0:02: **I PRESSED ONE BUTTON**

**0:02–0:26**
Unbroken speed-ramped take of the whole start sequence, captions naming each phase
as the LEDs change:
> `HEATING` (orange) → `HOMING` (green) → `LEVELLING` (purple) → `MESHING` (lime) →
> `PURGING` → `PRINTING`

VO, sparse — let the machine carry it:
"Heats. Homes. Levels the gantry. Re-homes Z. Meshes. Purges. Starts."

**0:26–0:36**
"That's `PRINT_START`. About forty lines. The point isn't that it's faster — it's
that I can't forget a step at eleven at night."
> Caption: ~40 lines of macro

**0:36–0:40 — CTA**
"Full macro breakdown on the channel."

---

## Short 3B — "Your printer should tell you what it's doing" (0:38)

**Hook (0:00–0:02)**
Dark room, toolhead LEDs breathing purple.
On-screen: **PURPLE MEANS I HAVE 90 SECONDS**

**0:02–0:24**
Cut between states, caption each as it appears:
- orange breathing → `HEATING`
- green → `HOMING`
- purple → `LEVELLING`
- lime → `MESHING`
- red-orange → `PRINTING`
- rainbow → `DONE — COME GET IT`

"Every macro sets an LED state. So from across the room I know whether it's still
levelling or already meshing, without opening a tab."

**0:24–0:34**
"Three GRBW LEDs on the toolhead and the LED effects plugin. That's the whole trick."
> Caption: klipper-led_effect

**0:34–0:38 — CTA**
"Config's linked on the channel."

---

## Short 3C — "Six lines that stop a crash" (0:45)

**Hook (0:00–0:02)**
On-screen: **THIS STOPS YOUR PRINT_END CRASHING THE GANTRY**

**0:02–0:14**
"Standard `PRINT_END` moves the toolhead twenty millimetres away from the print. What
happens when the print finished twenty millimetres from the edge?"
> Caption: …it doesn't go well

**0:14–0:34**
Code on screen, one line at a time:
```jinja
{% set x_safe = th.position.x + 20 * (1 if th.axis_maximum.x - th.position.x > 20 else -1) %}
{% set z_safe = [th.position.z + 2, th.axis_maximum.z]|min %}
```
"It checks whether there's room in that direction first — and goes the other way if
there isn't. And the Z lift is clamped to the machine maximum, so it can't ask for
a height that doesn't exist."

**0:34–0:41**
"Community Voron macro, not mine. Steal it. It prevents the kind of crash you only
hit once a year, which is the worst kind."
> Caption: STEAL THIS

**0:41–0:45 — CTA**
"Full macro walkthrough — link in the description."

---

## Posting plan

| Short | Post | Purpose |
|---|---|---|
| 3A (one button) | day of the long-form upload | best hook in the whole series; broadest reach |
| 3B (LED states) | +2 days | most shareable; carries with sound off |
| 3C (safe move) | +4 days | narrowest but highest-credibility with people who write configs |

**Note on 3A — blocker:** this Short is the long video's cold open, and it can't be
shot until the chamber LED naming issue in `script.md` is resolved. `PRINT_START`
calls `CHAMBER_LIGHTS_PRINT`, which addresses an LED object that isn't defined in the
committed config, so the sequence aborts. Fix that first; there's no version of this
Short that works without a clean run.

**Note on 3A:** shoot the source take at 60fps so the speed ramp holds up, and get the
whole sequence in one continuous shot. If you cut it, the claim stops being a
demonstration and becomes a montage — and viewers can tell.
