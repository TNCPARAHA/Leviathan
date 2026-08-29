# TNC Designz — Drop Campaigns (3 uploads)

Three 30-second YouTube Shorts drop campaigns, generated to the spec in
`TNC_Prompt_Generator_System.md` (Drive, version dated 11 Jul 02:53 — the one with
the corrected Suno rules; there's an older 02:36 copy that still describes music
prompts as a narrative paragraph, don't work from that one).

| File | Drop | Tempo / mood |
|---|---|---|
| `drop-01-chrome-monsoon.json` | **CHROME MONSOON** | 90 BPM dark ambient trap — moody, slow-motion, untouchable |
| `drop-02-shatter-protocol.json` | **SHATTER PROTOCOL** | 170 BPM glitchcore — violent, fast, high-impact |
| `drop-03-deep-current.json` | **DEEP CURRENT** | 100 BPM dark synthwave — eerie calm, weightless |

Deliberately spread across tempo and energy so the three don't read as one idea three
times. Post them in that order and the channel has a slow / fast / still rhythm
rather than three back-to-back assaults.

## Briefs used

These are the three example briefs from §4 of the spec doc:

```
Chrome exosuit fused with holographic rain-gear, moody slow-motion aesthetic, 90 BPM dark ambient trap
Liquid-metal bodysuit shattering into neon shards, aggressive cinematic energy, 170 BPM glitchcore
Bioluminescent street armor in a flooded megacity, eerie calm, 100 BPM dark synthwave
```

## Two ways to use these

**A — paste straight into the nodes (no Groq call needed).** Each JSON has the four
keys, one per node:

| Key | Node |
|---|---|
| `voiceover_script` | Voiceover Agent |
| `image_prompt` | Pollinations AI |
| `music_prompt` | Music Generator (Suno) — set duration to 30s in the node config, not the prompt |
| `video_notes` | Video Assembler |

**B — run the batch.** `batch-briefs.txt` holds the three briefs as one `|`-separated
string for the User Input node → Loop Iterator (`max_iterations: 3`) → Groq → the rest
of the graph. Confirm the Loop Iterator delimiter is set to `|`, not comma — the
prompts contain commas internally.

Either way: **YouTube Publisher `privacy_status` = private or unlisted.** Review in
Studio, then flip individually. Per §4 of the spec, and it matters more with three
queued than with one.

## Spec compliance

| Rule | 01 | 02 | 03 |
|---|---|---|---|
| Voiceover ≤ 95 words | 46 | 59 | 53 |
| Structure: hook → escalation → drop name → CTA | ✓ | ✓ | ✓ |
| Music prompt < ~200 chars, tag-style, no narrative arc | 162 | 151 | 156 |
| Image prompt: pose, garment, lighting, lens, environment, palette, mood, quality stack | ✓ | ✓ | ✓ |
| Fictional model, no real person / celebrity / brand logo | ✓ | ✓ | ✓ |
| Negative-prompt line present | ✓ | ✓ | ✓ |
| `video_notes` 3–5 bullets | 5 | 5 | 5 |
| Exactly four keys, valid strict JSON | ✓ | ✓ | ✓ |

Word counts sit under the 95 cap rather than at it — the spec's own worked example
runs ~55 words for ~26–28s, and 01 and 03 are slower reads than that, so padding them
to 95 would overrun 30 seconds. 02 is the fast one and carries the most words.

**Cultural safety:** no culturally-specific motifs or names appear in any of the three,
per the spec's voiceover rule. Nothing here needs `sanitizeForAI()` before it reaches
Gemini, DALL·E or Pollinations — but the guardrail still applies to anything you write
by hand and drop into the pipeline later.

## Note on where these live

These are in the Leviathan repo because that's the branch this task was running on —
it's a 3D-printer hardware repo and has nothing to do with TNC Designz. Say the word
and I'll move them into your Drive workflow folders instead, which is where the rest
of the pipeline reads from.
