# Digesting footage and inspiration

All commands work in macOS Terminal and Windows PowerShell. Quote paths that contain spaces. Write everything to `Claude Edit/work/`.

## Footage

**Probe:**
```
ffprobe -v error -show_entries stream=codec_type,width,height,r_frame_rate,sample_rate:format=duration -of json "Footage/clip.mp4"
```
Phones often record 1080x1920 at 29.97 or 30 fps, sometimes with a rotation flag; check `side_data` for rotation. Edit at 30 fps unless the user says otherwise.

**Audio for analysis:** 16 kHz mono WAV.
```
ffmpeg -v error -y -i "Footage/clip.mp4" -vn -ac 1 -ar 16000 "Claude Edit/work/clip.wav"
```

**Transcript with word timestamps:**
```
python scripts/transcribe.py "Claude Edit/work/clip.wav" "Claude Edit/work/clip.words.json"
```
Use `--model small.en` for speed and `medium.en` (the default) for accuracy. Use `--model medium --language xx` for other languages.

**Pauses:** find them on the waveform (silencedetect). Use these to place cuts precisely; word timestamps alone drift by up to about 0.2 s.
```
ffmpeg -i "Claude Edit/work/clip.wav" -af silencedetect=noise=-38dB:d=0.12 -f null -
```

**Frames to see the framing:** one frame a second, small.
```
ffmpeg -v error -i "Footage/clip.mp4" -vf "fps=1,scale=270:-2" "Claude Edit/work/frames/%04d.jpg"
```
Look at a few of them. Note:
- where the eyes and mouth are
- how much headroom there is
- which areas are empty and could hold graphics
- the light and colour

Captions must not cover the eyes or mouth. Graphics go in the empty space.

## Retakes

People say a line, stumble, and say it again. Walk the transcript and group repeated attempts at the same sentence. Keep the **last complete, clean** attempt and cut the earlier ones. Also cut:
- false starts ("So the, so the…")
- self-talk ("let me do that again", "hang on")
- long thinking pauses

Keep everything that is said only once. When unsure whether a phrase is a retake or a deliberate repeat, keep it and ask.

## Inspiration reels

**Frames at 4 fps as a contact sheet** for each reference:
```
ffmpeg -v error -i "Inspiration/ref.mp4" -vf "fps=4,scale=180:-2,tile=8x6" "Claude Edit/work/ref-sheet-%02d.jpg"
```

**Shot changes,** to measure the pace (cuts per second):
```
ffmpeg -i "Inspiration/ref.mp4" -vf "select='gt(scene,0.3)',showinfo" -f null - 2>&1 | grep pts_time
```
On Windows use `| findstr pts_time` instead of `| grep pts_time`.

**Its audio:** transcribe it the same way, to see how captions track speech (words per caption, when they appear).

From the sheets, describe the layout, captions, pacing, effects, graphics and sound in a short style brief (see SKILL.md section 5). Quote numbers where you can: "a cut every 1.4 s", "captions 2-3 words, white, 7% of frame height, lower third".
