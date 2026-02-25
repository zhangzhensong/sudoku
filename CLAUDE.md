# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

Open `index.html` directly in a browser — no build step, server, or dependencies required.

## Architecture

This is a single-file browser app (`index.html`) containing all CSS and JavaScript inline. There is no framework, bundler, or package manager.

**State variables** (all module-level globals in the `<script>` block):
- `solution` — the fully solved 9×9 grid (2D array)
- `board` — the current player-visible grid (0 = empty)
- `fixed` — boolean 9×9 array marking pre-filled clue cells
- `hinted` — boolean 9×9 array marking cells revealed by the hint button
- `notes` — 9×9 array of `Set` objects holding pencil-mark candidates
- `cells` — 9×9 array of DOM `<div>` elements
- `selectedCell` — `{row, col}` or `null`
- `notesMode`, `won` — flags

**Puzzle generation flow:**
1. `generateSolution()` — fills a blank grid via backtracking with randomized number order
2. `createPuzzle(sol, cluesPerBox)` — removes cells box-by-box, verifying unique solution via `countSolutions()` after each removal; `cluesPerBox=4` means 4 clues kept per 3×3 box (36 total clues)

**Rendering:** `renderBoard()` is a full re-render — it rebuilds all cell classes and inner content on every state change. No partial updates.

**CSS highlighting classes** applied by `renderBoard()`:
- `.selected` — the chosen cell
- `.highlighted` — same row/col/box as selected
- `.same-number` — cells sharing the selected cell's value
- `.conflict` — cells violating Sudoku constraints
- `.hint-cell` — green, used for hint-revealed cells

**Input entry points:** keyboard (`keydown` listener), numpad buttons, and board clicks all funnel into `enterNumber(num)` or `moveSelection(dr, dc)`.
