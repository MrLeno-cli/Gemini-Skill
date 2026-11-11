# The Premiere Pro route

Claude can't open or click around in Premiere for the user. Instead it writes a timeline file that Premiere imports: Final Cut Pro 7 XML, which Premiere reads natively on macOS and Windows. Everything arrives as real, separate clips on separate tracks.

## Build it
1. **Write a spec** `Claude Edit/versions/<name> v<N>.json` (format at the top of `scripts/build_premiere_xml.py`):
   - one list of clips per track, V1 at the bottom
   - times in frames
   - optional `scale`, `center`, `scale_keys` (keyframed zooms), `level_db` for audio, and `label` (colour)
2. **Run it:**
   ```
   python scripts/build_premiere_xml.py "Claude Edit/versions/<name> v<N>.json" "Claude Edit/versions/<name> v<N>.xml"
   ```
   It checks every file exists, reads each file's length and format, and warns about overlapping clips on a track. Give each overlapping sound its own track.

## Track layout that works well
| Track | Contents |
|---|---|
| V1 | the footage cuts (each kept take is a clip; punch-in zooms as scale keyframes) |
| V2 | black-screen backgrounds / full-screen graphics |
| V3 | words behind the person |
| V4 | captions and text |
| V5 | big words |
| V6+ | film burns, grain, flashes (one-frame clips), frame/border |
| A1 | voice (the footage audio, cut with the picture) |
| A2 | music |
| A3+ | sound effects, packed so clips don't overlap on a track; trim each clip's silent tail first so they pack tightly |

## Label colours
- **The colours:** Premiere has 16 label colours: Violet, Iris, Caribbean, Lavender, Cerulean, Forest, Rose, Mango, Purple, Blue, Teal, Magenta, Tan, Green, Brown, Yellow.
- **Each round:** give everything new or changed a colour not used before, and tell the user which one. They can then see your changes at a glance.

## Tell the user how to import
1. Open Premiere (any project, or a new one) and go to **File → Import**.
2. Choose the `.xml`. On macOS you can press ⇧⌘G in the file window and paste the folder path.
3. A new sequence appears with the version name. Double-click it.
4. If any clip shows "Media Offline", the files moved. Right-click it → **Link Media** and point at the file.

## When the user has edited the timeline themselves
Ask them to export it: **File → Export → Final Cut Pro XML**, saved into `Claude Edit/`. Read that XML and treat it as the starting point for the next version, so none of their edits are lost. Keep their clip levels and positions exactly as they set them.

## Limits to explain honestly
- **Text and graphics:** they arrive as rendered video layers, not editable Premiere text. Changing a word or colour means asking Claude to re-render that layer, which keeps the same file name so it relinks automatically.
- **Baked blends:** if a layer has a blend baked in (for example text that inverts the footage behind it), moving it far in time breaks the match.
- **Premiere's own effects** (Lumetri colour, masks, Essential Graphics) aren't written by this route. Apply those in Premiere.
