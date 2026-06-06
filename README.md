# Ztar Z6 MIDI Trainer

A single-file web app for learning songs on a **Starr Labs Ztar Z6**. Drop in a
MIDI file and it lights up the notes on a Z6-style **key matrix** (the 6×24 grid
of fret buttons) and plays them back so you can follow along, step note-by-note,
and control the speed.

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

- **MIDI** — drop a `.mid` / `.midi` file.
- **Hooktheory / scale-degree JSON** — paste the clipboard data (or drop a
  `.json` file). Scale degrees (`sd`, e.g. `"1"`, `"3"`, `"#6"`, `"b7"`) are
  resolved against the pasted key (`tonic` + `scale` — major, minor, and the
  church modes are supported), `octave` shifts by octaves, and `beat`/`duration`
  are converted to time using the BPM you set (Hooktheory clips carry no tempo).

## Features

- **Drag & drop** a `.mid` / `.midi` file (or click to browse).
- **Z6 key matrix** — a 6×24 grid of individual fret buttons (like the real
  neck) that light up:
  - 🟢 green = the key(s) playing right now
  - 🟠 orange = a suggested key to play each pitch (lowest practical fret;
    chords are spread across strings)
  - 🔵 blue = every other key on the Z6 that produces the same pitch
- **Play / Pause** continuous playback with a built-in synth.
- **Step mode** — advance one note/chord at a time with **Next / Prev**
  (or the ◀ ▶ arrow keys) so you can learn at your own pace. **Space** toggles play.
- **Speed control** from 0.1× to 2×.
- **Metronome click** on each step (optional).
- **Step strip** — click any step to jump straight to it.
- **Tuning editor** — set each string to match your Z6's actual tuning
  (defaults to standard **E A D G B E**), plus a global transpose and a
  selectable fret count.

## How pitch → fretboard mapping works

Each fret cell's pitch is `openStringNote + fretNumber` (fret 1 = one semitone
above the open string). A MIDI pitch usually exists at several string/fret
positions; the app highlights them all and picks a low, playable suggestion.
For chords it avoids assigning two notes to the same string.

If positions don't line up with your instrument, edit the **Tuning** row to
match how your Z6 is actually configured.

## Notes / limitations

- Parses Standard MIDI Files (format 0 and 1) including tempo changes.
- Playback is a simple triangle-wave synth intended for learning, not a
  realistic instrument sound.
- SMPTE-timed MIDI files fall back to a default resolution.
