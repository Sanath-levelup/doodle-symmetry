# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Doodle Symmetry is a kaleidoscope drawing app. It is a single self-contained `index.html` (inline CSS and JS) with no dependencies, no build step, no linter and no tests. To run it, open `index.html` in a browser. To verify a change, reload the page and draw on it. Edit `README.md` if you change the controls.

## Architecture

All logic is in the one `<script>` block, built around a stroke model that is replayed to render:

- **State**: `strokes` is an array of `{slices, size, points: [{x, y, color}]}`. `current` is the stroke being drawn.
- **Coordinates are center-relative.** `pos()` subtracts the canvas center, so points are stored relative to it. `drawSegment()` re-adds the center via `ctx.translate`, so drawings stay centered when the window is resized.
- **Symmetry is applied at render time, not stored.** `drawSegment()` draws each segment `slices` times, rotated by `2π·i/slices`. Each rotation is also drawn with `scale(1, -1)` for the mirror. Only the original points are kept.
- **Each stroke stores its own `slices` and `size`**, captured on `pointerdown`. Changing the sliders only affects new strokes, which the README states.
- **Color is stored per point.** `nextColor()` advances the global `hue` on every point in Rainbow mode, and the result is saved in the point. `redraw()` then reproduces the same colors exactly.
- **`redraw()` replays every segment from `strokes`.** Undo (button and Ctrl/Cmd+Z), Clear and `resize()` all mutate `strokes` (or the canvas size) and then call `redraw()`.
- **HiDPI**: `resize()` sizes the backing store by `devicePixelRatio` and sets a matching transform. `drawSegment()` recomputes the center from `canvas.width / devicePixelRatio`.

## Gotchas

- The canvas is transparent. Save PNG composites onto a hardcoded `#0e0e16` fill, which duplicates the CSS `--bg` value, so change both together.
- Adding a per-stroke option means capturing it in the stroke object on `pointerdown` and reading it in `drawSegment`/`redraw`. Reading a live control value at draw time would change old strokes on redraw.
