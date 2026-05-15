# Nocturne Narrative

A cinematic, high-contrast design system for long-form editorial scrollytelling, inspired by technical GIS analysis and investigative journalism.

## Design Principles
- **Atmospheric Depth**: Uses a deep charcoal and near-black palette to create a "Nocturne" environment that makes data and imagery pop.
- **Authoritative Typography**: Combines classical serif headings for narrative weight with high-readability sans-serif for technical copy.
- **Spatial Layering**: Employs background fixed-position stages and blurred glass containers to create a sense of physical depth during scrolling.
- **Technical Precision**: Accents of vibrant teal and indigo provide a "digital-first" feel to data-driven sections.

## Color Palette
- **Surface (Primary)**: `#131313` — The core narrative background.
- **Surface Dim**: `#0e0e0e` — Used for depth and section differentiation.
- **Surface Bright**: `#3a3939` — Used for borders and subtle highlights.
- **Accent (Teal)**: `#00ffc2` — Used for key data points, highlights, and status indicators.
- **Accent (Indigo)**: `#6366f1` — Used for primary actions and secondary data layers.
- **Text (High Emphasis)**: `#f7f8fc` — Pure clarity for headings.
- **Text (Medium Emphasis)**: `#e0e5f4` (74% opacity) — For body copy and technical analysis.

## Typography
- **Headings**: `Playfair Display`
  - *Weights*: 400 (Regular), 700 (Bold)
  - *Style*: High contrast, elegant, classic editorial feel.
- **Body & Interface**: `Inter`
  - *Weights*: 400, 500, 600
  - *Purpose*: Maximum legibility for dense urban planning data.
- **Monospace**: `JetBrains Mono`
  - *Purpose*: Metadata, kicker text, and GIS coordinate labels.

## Scrollytelling Patterns
- **Immersive Stages**: Full-viewport sections (`100vh`) with fixed backgrounds.
- **Content Cards**: Semi-transparent blurred containers (`backdrop-filter: blur(12px)`) that float over imagery to maintain text contrast.
- **Reveal Animations**: 560ms easing transitions on scroll to pace the delivery of information.