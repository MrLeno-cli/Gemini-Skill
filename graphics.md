# Graphics

Claude writes every graphic as code. There is no AI image or video generation in this workflow. Each graphic is an HTML page that draws onto a canvas for any time `t`. `scripts/render_html.py` renders it frame by frame with headless Chromium and encodes it with FFmpeg.

## Page template
```html
<!doctype html><html><body style="margin:0;background:transparent">
<canvas id="c" width="1080" height="1920"></canvas>
<script>
const c = document.getElementById('c'), ctx = c.getContext('2d');
window.TOTAL = 3.2;                           // seconds: exactly the length of its slot in the edit
window.ready = document.fonts.ready;          // wait for fonts / images before rendering
const ease = k => 1 - Math.pow(1 - Math.min(1, Math.max(0, k)), 3);
window.renderAt = (t) => {
  ctx.clearRect(0, 0, 1080, 1920);            // transparent background
  const k = ease((t - 0.2) / 0.4);            // appears at 0.2 s, on the word
  ctx.globalAlpha = k; ctx.filter = `blur(${(1 - k) * 12}px)`;
  ctx.fillStyle = '#fff'; ctx.font = '700 96px -apple-system, "Segoe UI", Helvetica, Arial, sans-serif';
  ctx.textAlign = 'center'; ctx.fillText('Your label', 540, 1500);
  ctx.filter = 'none'; ctx.globalAlpha = 1;
};
</script></body></html>
```

**Render:**
- `python scripts/render_html.py graphic.html "Claude Edit/graphics/03 - label.mov"` gives transparent ProRes 4444 for Premiere or compositing.
- Use `.mp4` for an opaque preview. `--query` passes URL parameters, so one page can hold several variants.

## Rules that keep graphics looking professional
- **Timing:** time every element to a word in the transcript, and keep a cue list (word → time) in the page.
- **Fonts:** use system fonts that exist on both systems (`-apple-system, "Segoe UI", Helvetica, Arial`), or the brand fonts from the Brand folder, loaded with `@font-face` from a relative path.
- **Load order:** measure text only after the fonts have loaded (`window.ready`), or the layout uses fallback metrics.
- **Real UI:** recreate what the speaker describes (app windows, chats, timelines, folders) so it looks like a real screenshot. Never use a real company's logo or interface the user isn't entitled to show.
- **Motion:** start fast and settle slow, fade and blur in, with gentle continuous drift so nothing is ever completely static.
- **Safe area:** no bouncy scale pops, and keep inside the safe area (see editing-rules.md).
- **Real reels:** when you show a real reel inside a graphic, extract its frames with FFmpeg (`-vf fps=30`) and draw them. Show only the clip being talked about playing; keep the others paused.
- **Check stills:** before rendering the whole thing, render stills at the key words (call `renderAt(t)` in a quick script, or render a short range) and look at them.

## Layers for the editable (Premiere) route
Render each kind of element as its own transparent layer so the user can change it in Premiere:
- backgrounds
- text
- big words
- words behind the person
- flashes
- frame or border

Then cut each layer into one clip per appearance: wherever the layer is empty and wherever the scene changes. A good trick for keeping blends exact: render the full composite, then keep only each layer's own pixels (draw that layer's shape as a mask and use `destination-in`). The layers then stack back into exactly the same picture.

## Words behind the person (optional, needs rembg)
```
pip install rembg onnxruntime
rembg p "Claude Edit/work/frames_full" "Claude Edit/work/mattes"
```
`frames_full` holds full-resolution frames from FFmpeg. The output is RGBA cut-outs. Draw the word, then erase it where the person's matte is opaque.
