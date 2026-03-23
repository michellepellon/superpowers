---
name: frontend-design
description: >
  Create distinctive, production-grade frontend interfaces with high design
  quality. Use when building web components, pages, or applications.
  Generates creative, polished code that avoids generic AI aesthetics.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  adapted-from: harperreed/dotfiles/.claude/skills/frontend-design
---

# Frontend Design

## Overview

This skill guides creation of distinctive, production-grade frontend interfaces for general-purpose work: components, pages, and applications. It is not for landing pages — use the `landing-page-design` skill for those. Implement real working code (HTML/CSS/JS, React, Vue, etc.) with exceptional attention to aesthetic details and creative choices that make every project feel genuinely designed for its context.

## MANDATORY: Vibe Discovery (Do This First)

**BEFORE writing any code, run the Vibe Discovery process.** Do not skip this step, do not pick from a menu, and do not fall back on comfortable defaults. The Vibe Discovery process ensures every project starts with a unique aesthetic direction derived from the real world rather than from pattern-matched AI defaults.

See the full process in `../_shared/references/vibe-discovery.md`.

## Frontend Aesthetics Guidelines

### Typography

Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial, Inter, and Roboto — opt instead for distinctive choices that elevate the interface's aesthetics. Unexpected, characterful font choices create immediate visual identity. Pair a distinctive display font with a refined body font: the display font sets the tone and personality, while the body font ensures readability and complements without competing. Never settle for the first font that comes to mind; if it feels familiar, it is probably overused.

### Color & Theme

Commit to a cohesive aesthetic rather than hedging across multiple directions. Use CSS variables for consistency so that your palette propagates cleanly through every component. Dominant colors with sharp accents outperform timid, evenly-distributed palettes — a bold primary with one or two precise accent colors creates stronger visual hierarchy than five colors used in equal measure. Let the color story reinforce the vibe: warm, cool, muted, saturated, monochromatic, or high-contrast should all be deliberate choices traced back to the project's identity.

### Motion

Use animations for effects and micro-interactions that make the interface feel alive. Prioritize CSS-only solutions for HTML projects; use the Motion library for React when available. Focus on high-impact moments rather than sprinkling animations everywhere: one well-orchestrated page load with staggered reveals (using `animation-delay`) creates more delight than scattered micro-interactions competing for attention. Scroll-triggered animations and hover states that surprise — an unexpected scale, a color shift, a subtle rotation — turn passive browsing into active discovery. Every animation should feel intentional and connected to the overall aesthetic.

### Spatial Composition

Embrace unexpected layouts. Asymmetry. Overlap. Diagonal flow. Grid-breaking elements that draw the eye and create visual tension. Generous negative space OR controlled density — both are valid tools depending on the vibe, but the mushy middle ground of "mostly evenly spaced" is almost never interesting. Let the content's nature dictate spatial relationships: a dense data interface can be beautiful through rigorous alignment, while a creative portfolio might thrive on deliberate chaos. Break the expected grid when the design calls for it.

### Backgrounds & Visual Details

Create atmosphere and depth rather than defaulting to solid colors or plain white. Gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders, custom cursors, and grain overlays all contribute to a sense of place and craft. These details should match and reinforce the overall aesthetic — a warm organic vibe calls for soft textures and natural gradients, while a brutalist direction might use harsh shadows and raw surfaces. The background is not empty space; it is the stage on which everything else performs.

## Anti-AI-Slop Principles

Generic AI output has a look: you have seen it a thousand times and it is instantly recognizable. These principles exist to ensure your work never triggers that recognition.

### Icons

Avoid Lucide — it is overused to the point of being a visual fingerprint for AI-generated interfaces. Use instead:

- **Iconify Solar**: Multiple styles available (outline, broken, duotone) that give you range within a single family.
- **Heroicons**: When you need Apple-like simplicity and clarity.
- **Phosphor**: A flexible weight system that adapts to different densities and moods.
- **Custom SVGs**: For brand differentiation when no icon set captures the right feel.

### Fonts

Kill Inter, Roboto, Arial, and system fonts. These are the typographic equivalent of beige carpet — functional, invisible, and utterly devoid of character. No defaulting to comfortable choices. If a font name is the first one that comes to mind, it is probably too common. Every project deserves a typographic identity that someone actually chose with intention.

### Colors

No purple gradients on white backgrounds. This specific combination has become the calling card of generic AI output. If your instinct reaches for violet-to-indigo on a light field, stop and reconsider. There are millions of color combinations in the world; use one that was not generated by default.

### Anti-Anti-Slop

Dark backgrounds with monospace fonts and neon green, cyan, or magenta accents — the terminal/hacker aesthetic — is just as overplayed and lazy as purple-on-white. It is not edgy or distinctive; it is the default fallback when trying too hard to avoid looking generic. Avoiding one cliche by falling into another is not progress. If your "edgy" design looks like every other developer's dark-mode portfolio, you have simply traded one form of sameness for another.

### Layouts

Break the grid. Overlapping elements, diagonal sections, asymmetric spacing, container-breaking hero elements, and negative space used as a deliberate design element all push beyond the predictable card-grid-with-rounded-corners that dominates AI output. Not every layout needs to be radical, but every layout should feel like someone made a conscious choice about spatial relationships.

### Complexity Matching

Match implementation complexity to the aesthetic vision. Maximalist designs need elaborate code with extensive animations, layered effects, and rich interactive details — cutting corners on a maximalist vision produces something that looks half-finished rather than intentionally simple. Minimalist or refined designs need restraint, precision, and careful attention to spacing, typography, and subtle details — elegance comes from executing the vision well, not from writing less code. The amount of effort should match the ambition of the aesthetic.

## Aesthetic Reference Library

For implementation raw materials — font pairings, hex codes, CSS techniques, and layout patterns organized by aesthetic category — see `../_shared/references/aesthetic-library.md`. This is a parts bin for cherry-picking after you have completed Vibe Discovery, not a menu of styles to adopt wholesale.

## Choosing an Aesthetic

When selecting a direction, consider what the content actually IS. A restaurant does not need to look like a SaaS dashboard. A personal blog does not need to look like a developer portfolio. A community forum does not need to look like a fintech app. Let the subject matter drive the aesthetic, not your default comfort zone or the last project you worked on.

Roll the dice. If your instinct says "dark background," go light. If you reach for monospace, pick a serif. If you are about to use a card grid, try a magazine layout. Fight your own patterns — the first idea is usually the most predictable one, and predictable is the enemy of distinctive.

Remember: Claude is capable of extraordinary creative work. Do not hold back. Do not settle for "good enough" or "clean and modern." Commit fully to a distinctive vision and show what can truly be created when thinking outside the box. Every project is an opportunity to make something that has never existed before.
