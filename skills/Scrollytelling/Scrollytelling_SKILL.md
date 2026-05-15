---
name: scrollytelling
description: This skill should be used when users want to plan, design, review, or implement a scrollytelling experience where scroll progression drives a narrative across a landing page, brand story, editorial feature, report, launch page, or portfolio case study.
---

# Scrollytelling

This skill provides guidance for creating scrollytelling experiences that feel intentional, performant, and strategically justified.

## Define Scrollytelling Correctly

Treat scrollytelling as a narrative technique where **scroll progression advances the story**.

Distinguish true scrollytelling from ordinary scroll-triggered animation:

- Require a **narrative arc** with a beginning, tension, reveal, and resolution.
- Require **scroll-content synchronization**, not just decorative motion as content enters the viewport.
- Require an **experience logic** strong enough that removing the scroll interaction would collapse the story.

Do not label a page as scrollytelling merely because elements fade, parallax, or animate on scroll.

## Use Scrollytelling Selectively

Apply scrollytelling where narrative depth materially improves perception, comprehension, or memorability:

1. Brand manifestos and origin stories
2. Product or service launches that need staged explanation
3. Strategic reports, data narratives, and research-led storytelling
4. Premium editorial features and portfolio case studies

Avoid forcing scrollytelling onto every page. Prefer conventional page structure for:

- search, listing, pricing, checkout, and utility flows
- pages whose primary goal is fast conversion
- audiences likely to be rushed or technically constrained
- brands or offers without a story strong enough to justify the added friction

## Follow the Narrative-First Workflow

### 1. Define the story before designing

Write the story as plain prose before choosing interactions.

Answer these questions first:

- What must the visitor understand or feel by the end?
- What are the three or four core scenes?
- Where is the tension?
- What is the reveal?
- What is the resolution or next step?

If those answers are weak, do not proceed into effect design yet.

### 2. Scope the experience

Limit scrollytelling to the specific page or section where it creates leverage.

Prefer a narrow scope over site-wide novelty. A single strong experience page is usually more effective than repeating the pattern everywhere.

### 3. Storyboard scenes

Sketch the scroll journey like a sequence of film frames.

For each scene, define:

- the narrative purpose
- the content visible on entry
- the scroll action that advances the scene
- the visual transformation
- the exit condition into the next scene

Use low-fidelity storyboards before high-fidelity layout work. Resolve narrative inconsistencies early.

### 4. Map content to interaction

Choose the lightest interaction model that serves the narrative:

- sticky text paired with changing media
- pinned scene with progressive reveals
- scrubbed animation tied to scroll position
- layered parallax for depth
- image, chart, or 3D state transitions
- section-to-section narrative handoffs with breathing space

Treat parallax as only one possible device, not the definition of scrollytelling.

### 5. Keep exit points visible

Preserve user agency throughout the experience.

Always provide:

- clear progress through the page
- breathing space between dense scenes
- visible navigation or skip paths where appropriate
- accessible calls to action without forcing the full narrative first

Do not trap visitors inside a tunnel.

## Design Principles

### Prioritize comprehension over spectacle

Use motion to clarify the story, not to show technical ambition.

For every effect, be able to answer:

- What information becomes clearer because of this movement?
- Why is scroll the correct input for this transition?
- What is lost if the effect is removed?

If the honest answer is only "it looks cool," reduce or remove the effect.

### Build scene hierarchy clearly

Create a strong first viewport signal and preserve visual hierarchy through the entire journey.

Use:

- one dominant focal element per scene
- restrained supporting motion behind content
- clear text containers with reliable contrast
- deliberate spacing between story beats
- strong media framing so visuals read as portfolio artifacts, not inline filler

### Pace the story

Alternate dense sections with quieter moments.

Use the scroll rhythm to control:

- anticipation
- reveal timing
- pause moments
- transitions into the next idea

Avoid making every section equally animated or equally loud.

## Technical Strategy

Choose the stack according to ambition, not trend:

- Use native CSS and Intersection Observer for lightweight reveal-based storytelling
- Use GSAP ScrollTrigger for precise pinning, scrubbing, and section orchestration
- Use Lenis or another smoothing layer only if it improves feel without harming accessibility
- Use Framer Motion for React interfaces with component-level orchestration
- Use Three.js or WebGL selectively for atmospheric layers, product demos, data scenes, or spatial storytelling

Integrate technical planning early. Do not finish visual design first and only then ask development to "make it move."

## Motion and Interaction Guardrails

Keep motion secondary to content.

1. Prefer scroll-linked progression over constant background animation
2. Limit heavy pinned scenes to the moments that earn them
3. Make transitions smooth but not slow
4. Keep decorative layers behind the narrative content
5. Preserve readable HTML content even when JavaScript is unavailable or reduced

When using Three.js, WebGL, video, or complex media:

- render only where the story benefits from it
- pause or reduce work when scenes are offscreen
- cap pixel ratio where practical
- degrade gracefully on mobile or low-power devices

## Performance, Accessibility, and SEO

Treat performance as part of the premium feel.

### Performance

- Keep LCP, CLS, and general responsiveness under control
- Avoid oversized media and unnecessary continuous animation
- Lazy-load non-critical assets
- Test lower-end mobile behavior, not just desktop

### Accessibility

- Respect `prefers-reduced-motion`
- Keep all important text as readable HTML
- Preserve keyboard access and visible focus states
- Avoid relying on motion alone to communicate meaning

### SEO

- Serve content in crawlable HTML, not only through JavaScript scene generation
- Use semantic headings and structure
- Separate keyword-capture articles from brand-experience pages when the goals differ

Do not sacrifice crawlability or usability for cinematic effects.

## Measurement and Business Framing

Evaluate scrollytelling as a brand and comprehension asset, not only as a novelty layer.

Track outcomes such as:

- scroll depth
- dwell time
- qualified lead conversion
- demo requests or contact actions
- downstream engagement from brand or organic traffic

Frame ROI around memorability, differentiation, and stronger perception when direct attribution is imperfect.

## Review Checklist

When planning or reviewing a scrollytelling page, verify all of the following:

1. Confirm the page has a real story arc
2. Confirm scroll progression materially drives the narrative
3. Confirm the scope is justified and not overused across the site
4. Confirm users can still navigate, skip, and convert comfortably
5. Confirm the page stays performant and readable on mobile
6. Confirm reduced-motion handling exists
7. Confirm critical content remains semantic and crawlable
8. Confirm visuals and motion strengthen the message instead of distracting from it

## Default Implementation Approach

When asked to create a scrollytelling experience:

1. Extract the core narrative in prose
2. Break the story into scenes with explicit transitions
3. Decide which sections truly need scroll-driven behavior
4. Choose the lightest technical stack that can deliver the effect
5. Build the page with readable semantic content first
6. Layer interaction, sticky behavior, and media choreography second
7. Add atmospheric or 3D effects only where they support the narrative
8. Validate performance, accessibility, and clarity before polishing

## Anti-Patterns

Do not:

- confuse parallax or reveal-on-scroll with scrollytelling
- add scrollytelling to utility pages by default
- lock users into long pinned tunnels without exits
- bury calls to action at the very end of a forced narrative
- turn the page into a demo reel of effects with no editorial logic
- ship an experience that feels smooth on a high-end laptop but breaks on mobile

## Core Principle

Treat scrollytelling as an editorial and interaction system, not a visual garnish.

Make the visitor feel that scroll is revealing a story that could not be communicated as clearly through static layout alone.
