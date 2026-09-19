# Doodle Symmetry

A tiny kaleidoscope drawing app. Draw a single stroke and watch it repeat around the center, rotated and mirrored.

It's one self-contained file with no dependencies and no build step.

## Run it

Open `index.html` in any modern browser.

## Controls

| Control | What it does |
| --- | --- |
| **Mirrors** | Number of rotational slices (2–16). Each slice is also mirrored. |
| **Brush** | Brush width (1–24 px). |
| **Rainbow** | Cycle the stroke color through the hue wheel as you draw. |
| **Color** | Stroke color used when Rainbow is off. |
| **Undo** | Remove the last stroke (also `Ctrl+Z` / `Cmd+Z`). |
| **Clear** | Wipe the canvas. |
| **Save PNG** | Download the drawing as `doodle-symmetry.png`. |

Changing **Mirrors** only affects new strokes; existing strokes keep the slice count they were drawn with.
