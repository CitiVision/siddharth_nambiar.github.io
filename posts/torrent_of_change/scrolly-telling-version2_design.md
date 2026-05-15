# Nocturne Narrative — Version 2

A cinematic, high-contrast design system for long-form editorial scrollytelling, refined through the production of *Torrents of Change*. Inspired by technical GIS analysis, investigative journalism, and immersive data storytelling.

---

## Design Principles

- **Atmospheric Depth**: A deep charcoal and near-black palette creates a "Nocturne" environment that makes data and imagery pop.
- **Authoritative Typography**: Classical serif headings for narrative weight; high-readability sans-serif for technical copy; monospace for metadata.
- **Spatial Layering**: Sticky background stages and glass-morphism cards create physical depth during scrolling.
- **Technical Precision**: Vibrant teal and indigo accents provide a "digital-first" feel to data-driven sections.
- **Narrative Rhythm**: Content is broken into distinct Acts — each with its own visual grammar — so the reader feels a progression from problem → analysis → crisis → resolution.

---

## Color Palette

| Token | Hex | Usage |
|---|---|---|
| `--surface` | `#131313` | Core body background |
| `--surface-dim` | `#0e0e0e` | ⚠️ Nearly identical to surface — **avoid** as sole differentiator for sections (see note below) |
| `--surface-bright` | `#3a3939` | Borders, subtle dividers |
| `--accent` (Teal) | `#00ffc2` | Key stats, highlights, status indicators |
| `--accent-2` (Indigo) | `#6366f1` | Secondary data layers, supporting actions |
| `--text-high` | `#f7f8fc` | Headings, primary emphasis |
| `--text-mid` | `#e0e5f4` at 74% | Body copy, analysis text |
| `--text-low` | (muted) | Metadata, footnotes, source labels |
| `--border` | (subtle) | Card and section boundaries |
| `--border-accent` | (teal-tinted) | Accent card borders |

> **⚠️ Surface Dim Note:** `#0e0e0e` vs `#131313` is a difference of only ~5 brightness points — visually imperceptible on most screens. **Never use `--surface-dim` as the sole background treatment to differentiate a section.** Always pair it with a border, a bleed image, or a gradient to make the section boundary legible.

---

## Typography

| Role | Font | Weights | Notes |
|---|---|---|---|
| Headings / Narrative pull-quotes | `Playfair Display` | 400, 700 | High contrast, italic for emotional/narrative moments |
| Body & UI | `Inter` | 400, 500, 600 | Maximum legibility for dense analytical content |
| Metadata / Eyebrows / Labels | `JetBrains Mono` | 400 | Kicker text, GIS references, footnote sources |

### Type Scale Conventions
- **Stat numbers**: `clamp(4rem, 12vw, 8rem)` — impactful, responsive
- **Narrative interstials**: `clamp(1.1rem, 2vw, 1.45rem)` — readable at cinematic scale
- **Card eyebrows**: `0.68rem`, `letter-spacing: 0.14em`, uppercase, monospace
- **Captions / footnotes**: `0.65rem`, monospace, uppercase, `--text-low`

---

## Page Structure — Act Framework

Structure every piece as a sequence of named **Acts**, each with a **Chapter Marker** entry point and a distinct visual mode.

```
[Hero / Title Screen]
   ↓
[Chapter Marker — Act I]
   ↓
[Scrolly-Act — sticky background + scrolling panels]
   ↓
[Rain-graph / Full-width split panel]  ← optional data break
   ↓
[Chapter Marker — Act II]
   ↓
[Scrolly-Act]
   ↓
[Chapter Marker — Act III]
   ↓
[Scrolly-Act]
   ↓
[Apocalyptic / Narrative Interstitial]  ← story beat re-entry
   ↓
[Panorama Image Sequence]
   ↓
[Chapter Marker — Act IV]
   ↓
[Framework Split — two-column content]
   ↓
[Full-width resolution section]
   ↓
[References / Footer]
```

---

## Component Patterns

### 1. Chapter Marker
Full-viewport section that names the act and sets the scene. Uses a bleed image + dark overlay.

```html
<div class="chapter-marker" data-chapter="[name]">
  <div class="chapter-marker-bg" aria-hidden="true">
    <img src="assets/[image].jpg" alt="..." loading="lazy" />
    <div class="chapter-marker-bg-overlay"></div>
  </div>
  <div class="chapter-marker-content">
    <p class="chapter-number">Act [N]</p>
    <h2 class="chapter-title">[Title]</h2>
    <p class="chapter-desc">[One or two scene-setting sentences.]</p>
  </div>
</div>
```

---

### 2. Scrolly-Act (Sticky Background + Scrolling Panels)

The core scrollytelling unit. A sticky full-viewport background image fades between states as the user scrolls through foreground panels.

**Key CSS rules:**
```css
.scrolly-act { position: relative; }

.act-bg {
  position: sticky;
  top: 0;
  height: 100vh;
  margin-bottom: -100vh; /* pulls panels up to overlay the bg */
  z-index: 0;
  overflow: hidden;
}

.act-panels {
  position: relative;
  z-index: 2;
}
```

**Key HTML structure:**
```html
<section class="scrolly-act" data-chapter="[name]">
  <div class="act-bg" aria-hidden="true">
    <div class="bg-layer is-active" data-bg="[id-1]">
      <img src="assets/[img1].jpg" ... />
      <div class="bg-overlay overlay-right"></div>
    </div>
    <div class="bg-layer" data-bg="[id-2]">
      <img src="assets/[img2].jpg" ... />
      <div class="bg-overlay overlay-center"></div>
    </div>
    <!-- For diagrams/technical images that must NOT be cropped: -->
    <div class="bg-layer contain white-bg" data-bg="[id-diagram]">
      <img src="assets/[diagram].png" ... />
      <!-- NO bg-overlay here — overlay hides labels/annotations -->
    </div>
  </div>

  <div class="act-panels">
    <div class="scroll-panel align-right" data-bg-target="[id-1]">
      <article class="glass-card reveal">...</article>
    </div>
    <div class="scroll-panel align-left" data-bg-target="[id-diagram]">
      <article class="glass-card reveal">...</article>
    </div>
  </div>
</section>
```

**Background layer variants:**

| Class | `object-fit` | Background | Use for |
|---|---|---|---|
| `.bg-layer` (default) | `cover` | dark overlay | Photographs, maps, scene-setting imagery |
| `.bg-layer.contain` | `contain` | `--surface-dim` | Diagrams, infographics where full image must show |
| `.bg-layer.contain.white-bg` | `contain` | `#ffffff` | Technical drawings, parametric analysis, labelled diagrams — **removes overlay too** |

> **⚠️ Diagram panels:** Always remove `<div class="bg-overlay ...">` from `.contain.white-bg` bg-layers. The overlay is designed for photographic scenes; it will obscure labels on technical drawings.

---

### 3. Sticky Globe Overlay (Act I pattern)

A sticky oversized decorative element (globe, map, illustration) that persists across multiple foreground panels then cleanly stops — without bleeding into the next section.

**The critical rule: `overflow-x: clip` not `overflow-x: hidden`**

`overflow: hidden` creates a scroll container, which scopes `position: sticky` to that element (the globe won't stick to the viewport — it becomes invisible). `overflow-x: clip` clips visually without creating a scroll container.

**Scope the sticky element's lifecycle with a wrapper:**
```css
.act-i-scope {
  position: relative;
  overflow-x: clip; /* NEVER overflow-x: hidden — it breaks sticky */
}

.act-i-globe-wrap {
  position: sticky;
  top: 0;
  height: 100vh;
  pointer-events: none;
  z-index: 1;
}

.act-i-panels {
  position: relative;
  margin-top: -100vh; /* overlays panels on top of the sticky globe */
  z-index: 2;
}
```

```html
<!-- Wrapper scopes the globe — it stops sticking when this div ends -->
<div class="act-i-scope">
  <div class="act-i-globe-wrap" aria-hidden="true">
    <img class="act-i-globe-img" src="assets/globe.png" ... />
  </div>
  <div class="act-i-panels">
    <!-- Panels 1, 2, 3 -->
  </div>
</div>
<!-- Globe stops here. Next section (e.g. rain-graph) is unaffected. -->
<div class="rain-graph-split">...</div>
```

---

### 4. Card Components

#### Glass Card (narrative panels)
Semi-transparent, blurred, floats above background imagery.
```css
.glass-card {
  background: rgba(13,13,13,0.78);
  backdrop-filter: blur(24px);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 44px 48px;
  max-width: 520px; /* scale up to 1040px for 2× size */
}
```

#### Stat Callout (large-number fact card)
Use for a single dominant statistic with supporting body text below.
```html
<div class="stat-callout reveal">
  <div class="stat-number">40%</div>
  <p class="stat-label">of the region is already at risk of flooding<sup>1</sup></p>
  <p class="stat-desc">
    Supporting narrative text with city names, flood types, context.
    <strong>City Name</strong> (flood type), <strong>City Name</strong> (flood type).
  </p>
  <p class="stat-source">¹ Source reference</p>
</div>
```
> **Pattern note:** Always pair a large stat with a `stat-desc` paragraph. A naked number lacks persuasive context. The `stat-source` footnote builds credibility.

#### Dual Stat Card
For side-by-side comparative numbers (e.g. two cities, two metrics).
Use `.dual-stat-number.teal` and `.dual-stat-number.indigo` for visual differentiation.

---

### 5. Narrative Interstitial (Apocalyptic / Story Beat)

A full-bleed image section used to **re-enter the narrative** after a data-heavy analysis section. Bridges the analytical and emotional registers.

**Do not use `--surface-dim` as the background** — it blends invisibly into the dark page. Always use a photographic bleed with overlay.

```html
<div class="apoc-text-block">
  <div class="apoc-text-block__bg" aria-hidden="true">
    <img src="assets/[scene-image].jpg" alt="" loading="lazy" />
  </div>
  <div class="apoc-text-block__content">
    <p>[Narrative paragraph in italic Playfair Display — 2–5 sentences, emotional register, present tense, vivid imagery.]</p>
  </div>
</div>
```

```css
.apoc-text-block {
  position: relative;
  min-height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.apoc-text-block__bg {
  position: absolute; inset: 0; z-index: 0;
}
.apoc-text-block__bg img {
  width: 100%; height: 100%; object-fit: cover;
}
.apoc-text-block__bg::after {
  content: '';
  position: absolute; inset: 0;
  background: rgba(8, 8, 8, 0.72);
}
.apoc-text-block__content {
  position: relative; z-index: 1;
  padding: 80px 8vw; max-width: 860px; text-align: center;
}
.apoc-text-block__content p {
  font-family: 'Playfair Display', Georgia, serif;
  font-size: clamp(1.1rem, 2vw, 1.45rem);
  line-height: 1.82;
  color: rgba(255,255,255,0.88);
  font-style: italic;
}
```

> **Writing guide:** Interstitial text should be in the present tense, second or third person, vivid and specific (real city names, sensory details). It serves as a dramatic "breath" between analysis sections. Keep to one paragraph — under 100 words.

---

### 6. Panorama Image Sequence

Full-width images displayed sequentially after a narrative moment. Each has an overlaid caption.

```html
<figure class="media-panorama" aria-label="[accessible description]">
  <img src="assets/[image].jpg" alt="[descriptive alt]" loading="lazy" />
  <figcaption class="media-caption-overlay">
    <p class="media-caption-label">[Short label · e.g. Scenario · 2050]</p>
    <p class="media-caption-text">[One sentence description.]</p>
  </figcaption>
</figure>
```

**Recommended image sequence rhythm:** existing condition → speculative/future → analysis/data → animation/GIF (always end with motion if available).

---

### 7. Two-Column Framework Split (Act IV / Resolution)

Used for the intervention/resolution section — cards on the left, a sticky portrait image on the right.

```css
.framework-split {
  display: grid;
  grid-template-columns: 58% 42%;
  min-height: 100vh;
}
.framework-right {
  position: sticky;
  top: 0;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.framework-right img {
  width: 100%; height: 100%;
  object-fit: contain;
}
```

> Place only the paired content inside `.framework-split`. Resolution text, comparison images, and footnotes go in a `.framework-fullwidth` div **after** the split — they read as independent conclusions, not sidebars.

---

## Scrollytelling JavaScript — bg-layer swap

The JS intersection observer pattern that swaps `is-active` on bg-layers as the user scrolls through panels:

```js
// Each scroll-panel has data-bg-target="[id]"
// Each bg-layer has data-bg="[id]"
// When a panel enters the viewport, activate its target bg-layer.

const acts = document.querySelectorAll('.scrolly-act');
acts.forEach(act => {
  const panels = act.querySelectorAll('.scroll-panel');
  const layers = act.querySelectorAll('.bg-layer');

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (!entry.isIntersecting) return;
      const target = entry.target.dataset.bgTarget;
      layers.forEach(l => l.classList.toggle('is-active', l.dataset.bg === target));
    });
  }, { threshold: 0.5 });

  panels.forEach(p => observer.observe(p));
});
```

---

## Reveal Animation

All cards and major content blocks use `.reveal` for a scroll-triggered fade-in:

```css
.reveal {
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 600ms ease-out, transform 600ms ease-out;
}
.reveal.is-visible { opacity: 1; transform: none; }
```

```js
const revealObserver = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('is-visible'); });
}, { threshold: 0.15 });
document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));
```

---

## Common Pitfalls & Fixes

| Problem | Cause | Fix |
|---|---|---|
| Sticky element doesn't stick | Ancestor has `overflow: hidden`, creating a scroll container | Change to `overflow-x: clip` on that ancestor |
| Sticky element bleeds past its section | No scoped containing block | Wrap sticky + panels in a `position: relative` div — sticky stops at that wrapper's bottom edge |
| Section looks invisible | `--surface-dim` background is indistinguishable from `--surface` body | Use a bleed image + overlay, or a border + gradient, to make section boundaries visible |
| Diagram image is cropped | `.bg-layer` uses `object-fit: cover` | Add `.contain` class: `object-fit: contain` |
| Diagram labels hidden | Dark `bg-overlay` sits on top | Remove `<div class="bg-overlay">` from that specific bg-layer; add `.white-bg` for technical drawings |
| Card feels cramped at desktop widths | Default `max-width` is calibrated for mobile-first | Double `max-width` and `padding` for desktop emphasis (e.g. 520px → 1040px) |
| Resolution text sits inside two-column split | Placed inside `.framework-split` | Move to a `.framework-fullwidth` div after the split — full-width sections read as independent conclusions |

---

## Responsive Breakpoints

At `max-width: 768px`:
- Hide globe (`.act-i-globe-wrap { display: none }`)
- Remove globe margin offset (`.act-i-panels { margin-top: 0 }`)
- Stack framework split (`.framework-split { grid-template-columns: 1fr }`)
- Right column becomes relative height (`.framework-right { position: relative; height: 60vw }`)
- Cards expand to `90vw` with reduced padding

---

## Asset Checklist for a New Project

Before building, confirm you have:
- [ ] **Globe / map** — large circular or geographic image for Act I sticky overlay
- [ ] **Chapter marker images** — one atmospheric photo per act (16:9 or wider)
- [ ] **Scrolly-act bg images** — 2–3 per act (cover crops acceptable)
- [ ] **Technical diagram** — PNG/SVG with white/light background for `.contain.white-bg` panel
- [ ] **Panorama images** — wide aspect ratio (≥ 3:1), landscape orientation
- [ ] **Narrative interstitial image** — dramatic, scene-setting; used as full-bleed bg for text
- [ ] **Resolution diagram** — portrait orientation works well for sticky right column
- [ ] **Comparison image** — before/after or side-by-side, for full-width closing section
- [ ] **Animated GIF** — optional, powerful as the final media moment before Act IV
