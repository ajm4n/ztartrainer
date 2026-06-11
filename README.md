# Ztar Z6 MIDI Trainer

A single-file web app for learning songs on a **Starr Labs Ztar Z6**. Drop in a
MIDI file (or paste Hooktheory data) and it lights up the notes on a Z6-style
**key matrix** — 6 strings × 24 frets of long, narrow key buttons, like the real
neck — and plays them back so you can follow along, step note-by-note, and
control the speed.

## Use it

Just open `index.html` in any modern browser (Chrome/Edge/Firefox/Safari).
No build step, no server, no dependencies — everything runs locally and
**nothing is uploaded**.

```
open index.html        # macOS
xdg-open index.html    # Linux
# or double-click the file
```

## Input formats

- **MIDI** — drop a `.mid` / `.midi` file. Handles format 0/1, tempo changes,
  and running status.
- **Hooktheory / scale-degree JSON** — paste the clipboard data (or drop a
  `.json` file). Scale degrees (`sd`, e.g. `"1"`, `"3"`, `"#6"`, `"b7"`) are
  resolved against the key active at each note's beat (`tonic` + `scale` —
  major, minor, and the church modes; **mid-clip key changes are honored**).
  `octave` shifts by octaves, and `beat`/`duration` are converted to time
  using the clip's embedded tempo when present, otherwise the BPM you set.

Songs whose pitches fall outside the neck's range are automatically shifted by
whole octaves to fit (the shift is shown next to the file name).

## Features

- **Z6 key matrix** — 6 rows × 24 frets of individual key buttons that light up:
  - 🟢 green = the key(s) playing right now
  - 🟠 orange = a suggested key to play each pitch — chosen to minimize hand
    travel (each note picks the position closest to where your hand already is;
    chords cluster into one playable shape, one string per note)
  - 🔵 blue = every other key on the Z6 that produces the same pitch
    (hidden by default via **Single key per note** for a piano-like 1:1 view)
- **Tap any key to hear it** — the board is live even before a song is loaded.
- **Play / Pause** continuous playback with a built-in synth; changing speed
  mid-playback keeps the song position.
- **Step mode** — advance one note/chord at a time with **Next / Prev**
  (or the ◀ ▶ arrow keys) so you can learn at your own pace. **Space** toggles play.
- **Speed control** from 0.1× to 2×.
- **Metronome click** on each step (optional).
- **Step strip** — click any step to jump straight to it.
- **Tuning editor** — set each string to match your Z6. Defaults to the
  **Z6 tuning: standard guitar intervals one octave down (E1 A1 D2 G2 B2 E3)**,
  with one-click presets for Z6 and concert-pitch EADGBE, plus a global
  transpose and selectable fret count. Low E is the top row.

## How pitch → fretboard mapping works

Each fret key's pitch is `openStringNote + fretNumber` (fret 1 = one semitone
above the open string). A pitch usually exists at several string/fret
positions; the suggested fingering is precomputed for the whole song and every
note takes the position with the **least physical finger distance** from the
previous one, using real neck geometry (one fret of travel ≈ three string
crossings). Chord notes cluster around one hand position (highest pitch
assigned first, one string per note), and the song's first note — which has no
previous position — simply prefers a low fret. All the alternative positions
can still be shown by unchecking "Single key per note".

If positions don't line up with your instrument, edit the **Tuning** row to
match how your Z6 is actually configured.

## Notes / limitations

- Playback is a simple triangle-wave synth intended for learning, not a
  realistic instrument sound.
- SMPTE-timed MIDI files fall back to a default resolution.
- Hooktheory chord tracks aren't rendered yet — melody notes only.
