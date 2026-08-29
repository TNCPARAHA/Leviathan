# Assembly Brief

For the Claude Code session running on the PC. Input is `content-manifest.json`
in this folder. Three uploads, each a long-form video plus a companion Short.

## Stack

| Step | Tool | Notes |
|---|---|---|
| Visuals | Google Flow | one clip per scene, generated at the scene's `duration_seconds` |
| Voiceover | pyxa.ai | reads each scene's `narration`, one render per scene |
| Ambient audio | Flow native | already constrained in every `flow_prompt` — see below |
| Music bed | your choice | one continuous track, direction in `music_direction` |
| Assembly | this session | ffmpeg |
| Upload | manual | see "The upload step" at the bottom |

## The audio rule that matters

Every `flow_prompt` ends with an explicit audio instruction:

> Audio: ambient sound design only: low mechanical hum and soft tonal resonance.
> No speech, no voices, no dialogue, no music.

Both exclusions are deliberate:

- **No speech.** Flow will invent dialogue if you let it. It collides with the pyxa
  voiceover and cannot be separated after generation. Exclude it at prompt time —
  there is no fixing it in the edit.
- **No music.** Flow's music can't be beat-matched across clips and jumps audibly at
  every cut. One continuous bed added in the edit instead.

If you regenerate or write new prompts, carry that line verbatim.

## Per upload

1. **Generate clips.** One Flow clip per scene, using `flow_prompt`, at
   `duration_seconds`. Keep the returned files named by scene id so ordering is
   trivial.
2. **Generate voiceover.** One pyxa render per scene from `narration`. Separate files,
   not one long read — per-scene renders keep timing adjustable without re-rendering
   everything.
3. **Check the fit.** Narration is written at roughly 150 words per minute against
   `duration_seconds`. If a pyxa render overruns its clip, extend the clip rather than
   speeding the voice up; a rushed read is the most obvious tell of an AI video.
4. **Assemble.** Concatenate clips in scene order. Lay voiceover over the top. Duck
   Flow's ambient audio to roughly -18dB under the voice. Music bed lower still.
5. **Overlays.** Titles, chapter cards and captions are added *here*, never inside a
   generated clip — generators cannot render legible text. Captions bottom-third,
   high-contrast white on a dark plate, line-by-line.
6. **Verify chapters.** The `chapters` timestamps were computed from the cumulative
   `duration_seconds`. If any clip's final length differs, recompute them — YouTube
   parses those literally and a drifted timestamp lands mid-sentence.

## Shorts

Each upload has a `short` object: one `flow_prompt`, one voiceover, and `edit_notes`.

Generate **one** vertical clip, not one per line. The cut comes from reframing and
punching into that single clip on the beat. Twelve clips for a 40-second Short costs
twelve generations and looks worse — the visual continuity is what makes it read as
deliberate rather than assembled.

## The upload step

This has to be done from the PC, by you or by this session if it has YouTube API
credentials configured. Either way:

**Set every upload to private.** Review, then flip to public individually.

That is not caution for its own sake — it is the one irreversible step in the whole
chain. Everything else can be regenerated for the cost of a few credits.

Suggested spacing: two to four days between uploads. Post the Short the same day as
its long-form, then two days later as a second push.

## What I could not verify

The Flow prompts are written to what text-to-video models generally respond to —
subject, motion, explicit camera direction, then style, then an audio instruction.
They have not been run through Flow. Expect to iterate on the first two or three and
then reuse whatever phrasing works: the `visual_style` string in the manifest is meant
to be edited once and applied everywhere, so fix it there rather than per scene.
