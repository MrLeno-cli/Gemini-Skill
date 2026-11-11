# Editing rules

These are the notes that come up again and again on short-form edits. Apply them before showing anything.

## Cuts
- **Frame grid:** every cut sits on the sequence frame grid (30 fps = 1/30 s steps). Round in the direction that keeps audio, never into a word.
- **Clip edges:** end a speech clip about 0.08-0.12 s after the last word's energy dies away. Start the next one about 0.06-0.08 s before the next word begins.
- **Pauses:** tighten every pause longer than about 0.45 s down to about 0.25 s, both inside clips and at joins. Short-form viewers leave during dead air.
- **Edge check:** measure the loudness of the first and last 60 ms of every speech clip. Louder than about −40 dBFS means a word is clipped, so widen the cut.
- **Verify:** stitch the cut audio and transcribe it again. Read it start to finish: every sentence complete, no doubled words, no stray "and"/"so" left over from a retake.
- **Jump cuts on the same shot:** they're fine for this format. Make them feel deliberate with a small punch-in (scale +8-15%) on alternate cuts, or hide them under a flash at a section change.

## What's on screen must match what's said
- **Timing:** time every graphic, label and highlight to the word it illustrates. If they say "take a screenshot", the screenshot happens on "screenshot", not a second before.
- **Wording:** labels use the speaker's own words. Never add text they don't say.
- **Clutter:** one idea on screen at a time. If a graphic is still up after the speaker has moved on, take it off.
- **Empty frames:** never show an empty frame while they're talking about something that could be shown.

## Captions
- **Safe area:** stay inside it. On 1080×1920, keep text out of the top ~200 px (account name, close button) and the bottom ~350 px (description, buttons). Keep it clear of the right edge's action buttons (about 140 px).
- **Face:** never over the eyes or mouth. Under the chin works well for full-face shots. On split screens, put them on the line between the two halves.
- **Style:** follow the reference. Common defaults:
  - 1-3 words per caption
  - heavy sans-serif
  - white with a subtle shadow, or a small black box behind
  - the keyword in a highlight colour or an italic serif
- **Sync:** captions appear on the word, not before it.

## Effects
- **Flashes:** only on transitions (a change of layout or screen), never in the middle of a shot. Use few; they're jarring.
- **Opening:** start slightly zoomed in (about 120%) under a white flash and ease out to 100% over about 1-1.5 s.
- **Motion:** text and graphics fade and blur in on the spot; avoid bouncy scale-up pops. Moves start fast and settle slowly.
- **Direction:** keep it consistent within a transition; don't zoom in and then straight back out.

## Versions
- **Numbering:** every round of changes is a new version file (v1, v2, …). Never overwrite.
- **Changelog:** keep `Claude Edit/CHANGELOG.md` with what changed in each version and which label colour marks it.
- **Scope:** change only what the notes ask. Mention anything else you noticed as a suggestion, without doing it.
