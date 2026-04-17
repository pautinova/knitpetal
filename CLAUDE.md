# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

KnitPetal is a single-file HTML/CSS/JS machine knit calculator for generating custom pattern instructions (stitch counts, row counts, shaping) for a fitted knit top on an LK 150 machine. No build system, no dependencies, no server required — open `knitpetal-v2 (1).html` in any browser.

## Architecture

The entire application lives in one HTML file with three embedded sections:

- **`<style>`** — CSS with design tokens (CSS variables) for colors, spacing, and responsive breakpoints. Print styles (`@media print`) hide UI chrome for PDF export.
- **`<body>`** — Two-column layout: left column = input forms, right column = pattern output.
- **`<script>`** — All logic in vanilla JS with no external libraries.

### JavaScript structure

1. **Gauge calculations** — Converts swatch measurements to stitches/inch (`A1`) and rows/inch (`B1`).
2. **Body calculations** — Derives cast-on counts, stitch shaping (waist-to-bust increases), and row counts for each section (cast-to-underbust, underbust-to-shoulder, strap).
3. **Sleeve calculations** — Computes bicep stitch count (rounded to nearest multiple of 4) and optional long-sleeve row count.
4. **Output renderer** — Dynamically writes six pattern sections into the right column using `innerHTML`. Sleeve toggle re-renders only the sleeve section using the cached `calcResults` object without re-running body calculations.
5. **Validation** — Runs before calculation; surfaces errors in a banner. Checks required fields, numeric ranges, and logical constraints (e.g., bust > waist).

### Key state

`calcResults` is the single state object populated after calculation and reused by the sleeve toggle to re-render without recalculating.

### Rounding rules

- Stitch counts: `Math.round()`
- Row counts: `Math.round()`
- Adjusted stitch differences: round odd values up to even
- Sleeve stitches: round to nearest multiple of 4

### Assumption tags

Knitting assumptions surfaced to the user (e.g., "1 increase per side every 2 rows") are wrapped in `<span class="assumption-tag">`. If calculation logic changes, update both the math and any relevant assumption tags.

## Missing assets

Tooltip images are referenced but not present in the repo:
- `images/stockinette-swatch.jpg`
- `images/body-measurements.jpg`
- `images/short-sleeve-placeholder.jpg`
- `images/long-sleeve-placeholder.jpg`
