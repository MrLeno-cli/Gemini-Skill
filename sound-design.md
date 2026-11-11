# Sound design

Good sound design makes a reel feel premium, but it must sit **under** the voice.
- **Prefer the user's own library** (`Sounds/`). The more they have, the more variety you can use.
- **No library?** FFmpeg can make simple clicks and tones.
- **Never use sounds the user doesn't have the rights to.**

## Layers (build several; each gets its own tracks in Premiere)
1. **Transitions:** a riser peaking exactly on a cut into a new screen, a hit on the cut, and a shutter or soft pop on the cut back.
2. **Clicks:** a quiet click as each word or caption lands, and a real mouse click wherever a cursor clicks on screen.
3. **Typing:** a short typing sound under text that types on letter by letter.
4. **Pops and tiles:** soft UI pops when a new caption block or card appears, and small tile ticks as items scroll past.
5. **Movement:** airy or granular movement sounds for things that actually move inside a shot (a zoom-out, scrolling, a turning 3D plane).
6. **Texture:** film-burn crackle, a low impact as a big word lands, a short swell into the biggest moment.
7. **Music:** a bed at about −20 to −25 dB under the voice, back-timed so the track ends where the video ends.

## Rules
- **No whooshes on a hard cut** between two shots. Nothing moves on a cut, so a whoosh sounds wrong. Whooshes are only for real movement.
- **No repeats within 2 seconds:** never play the same sound file twice within 2 s. Rotate through variants.
- **Vary the risers:** a different riser for each cut. The same riser every time is the first thing people notice.
- **Levels:**
  - clicks peak around −25 dBFS
  - pops and ticks around −30
  - texture −30 to −36
  - hits and risers around −18 to −24
  - voice around −6 to −3 dBFS peaks
  - finished mix about −14 LUFS integrated
- **Alignment:** line each sound's loudest moment up with the event (a riser's peak on the cut, a click on the word's first frame).
- **Shared moments:** don't stack many sounds on the same frame. If a cut already has a hit, skip the click.
- **Event log:** keep a list of every sound you place (file, start time, level, why). It builds the Premiere timeline, and it's how you answer "what's that sound at 0:07?".

## Placing sounds with FFmpeg (inside-Claude route)
Mix with `adelay` (start time in ms) and `volume`, then `amix=normalize=0`:
```
ffmpeg -i voice.wav -i click.wav -i riser.wav -filter_complex "[1:a]adelay=1450|1450,volume=-25dB[c];[2:a]adelay=2380|2380,volume=-20dB[r];[0:a][c][r]amix=inputs=3:normalize=0,alimiter=limit=0.95" mix.wav
```
For many sounds, a short Python script that sums the samples is easier and exact.
