# Upload day checklist

One pass per upload. Everything here is a thing that's cheap to fix before publishing
and expensive after.

## Before recording

- [ ] Re-read the fact-check table at the bottom of the script — configs change
- [ ] Open every file you'll show on camera and confirm the lines still say what the
      script quotes
- [ ] Fill in any empty table in a Reference doc you plan to show (an empty template
      on screen reads as an unfinished build)
- [ ] **Upload 03 only:** confirm `PRINT_START` actually completes on the current
      config — the chamber LED naming blocker in `03-.../script.md` aborts it as
      committed. No point shooting a one-button cold open that errors
- [ ] Decide the doc-drift beats: are you correcting them on camera, or fixing the
      docs first? Either is fine; being surprised by them mid-take isn't
- [ ] Clear the desktop, close DMs, hide anything with a serial number you don't want
      published beyond the board IDs you're deliberately showing

## Recording

- [ ] Screen capture at 1080p minimum, terminal font ≥ 16pt — it will be watched on a
      phone
- [ ] Editor/console theme in dark mode, consistent across every capture in the video
- [ ] Room audio: one take of 10s silence for noise profiling
- [ ] Shoot the cold open **last**, once you know what the video actually says
- [ ] Any one-take demo (Upload 03's start sequence) at 60fps for the speed ramp

## Edit

- [ ] Chapters match the description timestamps exactly — YouTube parses them literally
- [ ] First chapter starts at `0:00` or chapters won't render at all
- [ ] On-screen text out of the bottom-right 15% (duration stamp) and bottom 10%
      (progress bar)
- [ ] Every command shown on screen is also in the description or pinned comment —
      people can't copy from video
- [ ] No music under terminal/config sections
- [ ] Loudness around −14 LUFS integrated
- [ ] Captions: auto-generate, then fix every command, config key and product name by
      hand. `scanner_touch_threshold` will not survive ASR

## Metadata

- [ ] Title under 60 characters, board + printer name in the first 45
- [ ] Description first two lines work as the search snippet on their own
- [ ] Chapters pasted and verified after upload processes
- [ ] Tags pasted from `metadata.md`
- [ ] Playlist: *Voron 2.4 + Leviathan build*, correct position in the series
- [ ] End screen cards set (two, never three)
- [ ] Thumbnail uploaded, then checked at 120px wide — if the text is unreadable
      there, it's the wrong text
- [ ] Pinned comment posted immediately after publish
- [ ] "Not made for kids" set
- [ ] Shorts remixing allowed

## Shorts

- [ ] 1080 × 1920, under 60s
- [ ] Hook text fully readable within the first 1.5s
- [ ] Burned-in captions on every spoken line
- [ ] Description links the long-form video
- [ ] Any code card held ≥ 4s — people screenshot those
- [ ] Scheduled per the posting plan in `shorts.md` (day-of, +2 days, +4 days)

## After publish

- [ ] Watch the first 30s on a phone, muted, at arm's length. If the hook doesn't land
      that way it doesn't land
- [ ] Answer comments for the first 2 hours
- [ ] Add corrections to the pinned comment rather than editing the description —
      viewers who already watched won't re-read the description
- [ ] Note in the relevant Reference doc anything a commenter corrects, so the config
      and the videos stay in agreement
