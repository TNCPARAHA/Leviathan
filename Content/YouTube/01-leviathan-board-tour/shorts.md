# Upload 01 — Shorts

Three vertical cuts. Each stands alone (no "as I mentioned in the long video"),
each ends on a reason to go watch the long one. 9:16, 1080×1920, burned-in captions,
hook fully visible in the first 1.5 seconds.

---

## Short 1A — "Seven drivers, zero modules" (0:38)

**Hook (0:00–0:02)**
On-screen, huge: **7 STEPPER DRIVERS. NONE OF THEM CAN FALL OUT.**
Shot: top-down on `Media/front_no_heatsink.jpg`, fast push-in.

**0:02–0:12**
"Five TMC2209s and two TMC5160s, soldered to the board. No driver modules, no
pin headers, nothing to seat crooked at midnight."

**0:12–0:24**
"The two 5160s are the forty-eight volt pair, and on a CoreXY they go where they
matter — the A and B motors."
> Caption: X/Y = TMC5160 @ 48V

**0:24–0:33**
"Everything else — four Z motors and the extruder — is on 2209s. One board, whole
printer."
> Caption: 4× Z + E = TMC2209

**0:33–0:38 — CTA**
"Full board tour and my actual config — link in the description."
> Caption: FULL TOUR → main channel

---

## Short 1B — "The one line that breaks everyone's flash" (0:44)

**Hook (0:00–0:02)**
On-screen: **"MCU 'MCU': UNABLE TO CONNECT"**
Shot: the error in the Klipper console, red.

**0:02–0:14**
"Nine times out of ten this isn't your board. It's that you copied someone else's
serial ID out of their config — including mine."

**0:14–0:30**
Screen capture, typed live:
```bash
ls /dev/serial/by-id/
```
"That string has your chip's unique ID baked into it. Mine ends in
`310034000251333236353439`. Yours won't. It can't."
> Caption: COPY *YOUR* ID

**0:30–0:40**
"Paste yours into `[mcu] serial:`, restart Klipper, done. Same rule for your
toolhead board and your probe — three MCUs, three different IDs."

**0:40–0:44 — CTA**
"Whole Leviathan setup walkthrough on the channel."

---

## Short 1C — "Your fan is telling you it's dying" (0:41)

**Hook (0:00–0:02)**
On-screen: **YOUR HOTEND FAN IS TELLING YOU IT'S ABOUT TO DIE**
Shot: macro on the hotend fan spinning.

**0:02–0:16**
"This board has tachometer inputs on the fan ports. So the fan doesn't just spin —
it reports RPM back to Klipper."

**0:16–0:30**
"Which means a bearing going bad shows up as a number sliding down over a couple of
weeks. Not as a heat-creep clog four hundred layers into a twenty-hour print."
> Caption: RPM ↓ weeks before failure

**0:30–0:37**
"I check it with one macro before anything long. Takes two seconds."
> Caption: TOOLHEAD_DIAG

**0:37–0:41 — CTA**
"Config's public — link in the description."

---

## Posting plan

| Short | Post | Purpose |
|---|---|---|
| 1B (serial ID) | day of the long-form upload | highest search intent, strongest standalone value |
| 1A (drivers) | +2 days | spec-flex, feeds the "should I buy" audience |
| 1C (fan tach) | +4 days | sets up Upload 03 (`TOOLHEAD_DIAG`) |

**Rules that apply to all three:** no music bed over the terminal shots — screen
text needs to be readable and quiet reads as competent. Caption every spoken line;
most of this audience watches muted at work.
