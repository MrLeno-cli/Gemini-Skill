# Three proven reel styles

Use whichever is closest to the user's inspiration, then adjust to match it.

## 1. Full-screen talking head (simple, fast to make)
- **Framing:** the speaker fills the screen, cropped in a little so the face sits in the upper half. A slow push-in (about 3-6% over the shot) keeps it alive.
- **Captions:** one or two words at a time, in the middle just under the mouth, so the eyes and the words are close together.
- **Graphics:** simple ones in the lower third. For example a checklist that ticks off as each item is said, or an icon pair (app A → app B). Keep them on a dark rounded panel so they read over any background.
- **First-version trap:** the first version usually has too many flashes, zooms and layout changes. Calm, clean and readable wins here.

## 2. Educational split screen
- **Layout:** a hard 50/50 split. A graphic in the top half (1080×960) changes about every second, matched to the words. The speaker is in the bottom half.
- **Captions:** small, in a black box sitting on the line between the halves.
- **Big words:** cut to full screen on the important words, with large, heavy type over the chest.
- **Top-half graphics:** real-looking screens (an app window, a chat, a timeline), simple charts, single big words, icons.
- **Moving camera:** keep the camera on one continuous, slow move rather than several jumpy ones. Example: a slow scroll left to right while the cursor drags files in one at a time.
- **Positioning:** if graphics are placed in an editor, render them as full 1080×1920 transparent frames with the art in the top half. Editors position smaller frames relative to their own size, which puts them in the wrong place.

## 3. Cinematic (black-and-white podcast look)
- **Look:** black and white, slightly dark, with a vignette and film grain.
- **Text builds a word at a time:**
  - small clean sans-serif for most words
  - an elegant italic serif for keywords
  - heavy condensed capitals for the big words
- **Text inverts what's behind it:** use a difference blend so white text flips dark over light areas.
- **Words behind the person:** the big words sit behind the speaker's head.
  - Cut the person out of every frame with a segmentation model (`rembg`, free, works on macOS and Windows).
  - Layer the words between the background and the person.
  - Use red with a gentle 90-100% flicker and a grainy edge.
- **Black cutaways:** on key words, cut to a black screen with a graphic on a tilted 3D plane. Examples: a timeline, waveforms, film strips, captions. Give it shallow depth of field (far parts soft).
- **Film burns:** warm light leaks on the cuts in and out of black. Vary their speed so they don't move at one constant rate.
- **Inverted flashes:** one-frame inverted flashes, only on transitions.
- **Music:** back-time the track so its ending lands where the video ends.

For any style, the user's Brand folder (logo, fonts, colours) overrides the defaults.
