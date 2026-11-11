# Rendering the finished video (inside-Claude route, and the preview for the Premiere route)

## Picture
Build the edit as a list of segments (source file, in, out) plus overlay layers, then render with FFmpeg. For a few segments, `concat` the trimmed parts:
```
ffmpeg -ss 8.20 -t 3.10 -i "Footage/clip.mp4" -ss 16.17 -t 1.17 -i "Footage/clip.mp4" -filter_complex "[0:v]scale=1080:1920,setsar=1,fps=30[v0];[1:v]scale=1080:1920,setsar=1,fps=30[v1];[v0][v1]concat=n=2:v=1:a=0[v]" -map "[v]" -c:v libx264 -crf 16 cut.mp4
```
For many segments or keyframed zooms, write a small Python script that builds the filter graph, or render frame by frame. Overlay transparent graphics with `overlay` (ProRes 4444 keeps alpha):
```
ffmpeg -i cut.mp4 -i "Claude Edit/graphics/03 - label.mov" -filter_complex "[0:v][1:v]overlay=0:0:enable='between(t,2.0,4.5)'" ...
```
Use `-itsoffset` on an input to place a graphic later in the timeline.

## Audio
Cut the voice with exactly the same in/out points as the picture (this keeps sync). Then mix in the music and sound effects (see sound-design.md). Normalise the final mix to about −14 LUFS:
```
ffmpeg -i mix.wav -af loudnorm=I=-14:TP=-1:LRA=11 mix-14.wav
```

## Final file
```
ffmpeg -i picture.mp4 -i mix-14.wav -map 0:v -map 1:a -c:v libx264 -preset slow -crf 16 -pix_fmt yuv420p -c:a aac -b:a 256k -ar 48000 -movflags +faststart "Claude Edit/versions/<name> v<N>.mp4"
```
1080×1920, 30 fps, H.264 + AAC. This uploads cleanly to Instagram, TikTok and YouTube Shorts.

## Before showing it
- **Length and streams:** run `ffprobe` and check the duration is right and both video and audio are present.
- **Contact sheet:** make one and look at it.
  ```
  ffmpeg -i out.mp4 -vf "fps=1,scale=180:-2,tile=8x4" sheet.jpg
  ```
- **Audio check:** transcribe the final audio once more, so no word is clipped and no line repeats.
- **Report:** tell the user the file path, the length, and what changed since the last version.
