---
name: scientific-visualization
description: Create or export publication figures, or audit scientific figures on explicit request. Do not invoke for exploratory plots or routine data visualization.
license: MIT
metadata:
  version: "1.4"
  last-reviewed: "2026-10-01"
  skill-author: K-Dense Inc.
---

# Scientific Visualization

Use this skill only for publication figures, explicit scientific-figure audits, or publication export. Preserve scientific meaning and source data while improving readability.

## Core requirements

- Preserve raw data and images, transformations, exclusions, missing-value codes, normalization, binning, image adjustments, and random seeds. Never hide, invent, or selectively enhance data.
- Make units, sample structure, uncertainty, missingness, exclusions, and transformations clear where they affect interpretation. Keep modeled values distinguishable from observed values and show gaps where connecting points would imply observations that do not exist.
- Use readable labels and accessible encodings; do not rely on color alone. Inspect the figure at its rendered size.
- When delivering an exported figure, inspect the actual output for clipping and the properties required by the format or destination, such as dimensions, resolution, embedded rasters, or fonts.
- For venue-specific delivery or compliance checks, verify current official requirements for the venue, article/figure type, and submission phase. Treat dated profiles and planner output as advisory.

## References and tools

- Consult [publication guidelines](references/publication_guidelines.md) for design decisions or audits involving scientific encoding, accessibility, or figure integrity.
- For venue-specific delivery, use [journal requirements](references/journal_requirements.md) to identify the relevant checks and official sources.
- Use [Matplotlib, Seaborn, and Plotly examples](references/matplotlib_examples.md) for implementation and export patterns.
- See [color palettes](references/color_palettes.md) for palette semantics and contrast caveats.
- For publication export or validation, the [export planner](scripts/export_plan.py), [figure exporter](scripts/figure_export.py), [metadata inspector](scripts/image_metadata.py), and [palette audit](scripts/palette_audit.py) provide optional local checks. Run the relevant tool with `--help` for its arguments.
