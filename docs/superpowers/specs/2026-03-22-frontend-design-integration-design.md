# Frontend Design Skill Integration

**Date:** 2026-03-22
**Status:** Draft
**Source:** [harperreed/dotfiles/.claude/skills/frontend-design](https://github.com/harperreed/dotfiles/tree/master/.claude/skills/frontend-design)

## Problem

The superpowers repo has a `landing-page-design` skill with a Vibe Discovery process and anti-AI-slop principles, but no general-purpose frontend design skill. Harper Reed's `frontend-design` skill covers general frontend work (components, pages, applications) with significant overlap in Vibe Discovery but also unique content: an expanded Collision Method, a 9-category Aesthetic Reference Library with concrete font/palette/CSS recipes, and broader anti-slop rules including the "anti-anti-slop" rule (dark+monospace+neon is equally lazy).

Incorporating it directly would duplicate ~60% of landing-page-design. Merging everything into one skill would bloat it and muddy the scope.

## Solution: Extract Shared Foundation, Specialize Both

### Architecture

```
skills/
├── _shared/
│   └── references/
│       ├── vibe-discovery.md
│       └── aesthetic-library.md
├── frontend-design/
│   └── SKILL.md
├── landing-page-design/
│   └── SKILL.md              (modified)
└── ... (other skills unchanged)
```

### New: `skills/_shared/references/vibe-discovery.md`

Merges the best of both Vibe Discovery processes:

- **Conversational framework:** Landing-page-design's 4-question structured approach (Q1: place/object, Q2: emotion, Q3: collision influences, Q4: anti-patterns). More actionable than Harper's open-ended dig.
- **Vibe synthesis:** Harper's Collision Method as Step 2 — take a physical material + energy quality from the user's answers, combine into a vibe seed ("Velvet Frenetic", "Cardboard Surgical"). More generative than landing-page-design's current Step 2.
- **Derivation process:** From the vibe seed, derive colors, typography, layout, and motion — each traced back to the material/energy collision.
- **Vibe Spec template:** Landing-page-design's existing template, extended with Harper's MATERIAL and ENERGY fields.
- **Freshness Check:** Harper's 8-item version (superset of landing-page-design's 5 items). Adds "user's actual words are driving the aesthetic" and "did NOT mentally scan a list of aesthetic styles."
- **Anti-Convergence Rules:** From landing-page-design — no hex code memory, font rotation required, collision must show, wildcard mandatory, name it.
- **Minimal context fallback:** Harper's generative approach — derive from the project content itself rather than falling back to presets.
- **Example:** Landing-page-design's "Shinjuku Runway" example, which is concrete and compelling.

### New: `skills/_shared/references/aesthetic-library.md`

Framed explicitly as a "parts bin, not a menu" (Harper's framing):

- **Rules for using the library:** Must have a completed Vibe Spec first. Cherry-pick individual techniques. Never adopt a whole section. If final design maps cleanly to one category, restart Vibe Discovery.
- **9 categories from Harper's skill**, each with specific fonts, hex codes, layout principles, and CSS techniques:
  1. Warm / Organic
  2. Luxury / Refined
  3. Playful / Toy-like
  4. Art Deco / Geometric
  5. Soft / Pastel / Dreamy
  6. Retro-Futuristic / Sci-Fi
  7. Editorial / Magazine
  8. Maximalist / Eclectic
  9. Industrial / Utilitarian
- **"When Stuck" appendix:** Landing-page-design's Inspiration Starters (spaces, objects, eras/movements) as supplementary prompts.
- **Generating Fresh References:** Harper's generative process for deriving physical-world anchors from the user's domain, context, and timing.

### New: `skills/frontend-design/SKILL.md`

**Scope:** General frontend — components, pages, applications. Not landing pages.

**Frontmatter:**
```yaml
name: frontend-design
description: >
  Create distinctive, production-grade frontend interfaces with high design
  quality. Use when building web components, pages, or applications.
  Generates creative, polished code that avoids generic AI aesthetics.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  adapted-from: harperreed/dotfiles/.claude/skills/frontend-design
```

**Content sections:**

1. **Overview** — Brief scope statement: distinctive, production-grade frontend interfaces.
2. **Vibe Discovery** — Mandatory gate: "BEFORE writing any code, run the Vibe Discovery process." References `../../_shared/references/vibe-discovery.md`.
3. **Frontend Aesthetics Guidelines** — From Harper's skill:
   - Typography: distinctive, characterful font choices; pair display + body
   - Color & Theme: CSS variables, dominant colors with sharp accents
   - Motion: CSS-only preferred for HTML, Motion library for React; focus on high-impact moments
   - Spatial Composition: unexpected layouts, asymmetry, overlap, grid-breaking
   - Backgrounds & Visual Details: gradient meshes, noise textures, geometric patterns, grain overlays
4. **Anti-AI-Slop Principles** — Merged:
   - Icon alternatives (Iconify Solar, Heroicons, Phosphor, custom SVGs)
   - Font blacklist (Inter, Roboto, Arial, system fonts)
   - Color blacklist (purple gradients on white)
   - Anti-anti-slop rule: dark+monospace+neon is equally lazy
   - Layout: break the grid
   - Complexity matching: maximalist designs need elaborate code; minimalist needs restraint
5. **Aesthetic Reference Library** — References `../../_shared/references/aesthetic-library.md`.
6. **Choosing an Aesthetic** — Harper's "roll the dice" guidance: fight your own patterns, let subject matter drive the aesthetic.

**Explicitly excluded** (stays in landing-page-design):
- Section Composition (hero through footer)
- 50% Rule
- Prompt Patterns
- Implementation Workflow phases
- Quality Checklist (conversion optimization)
- Design Resources

### Modified: `skills/landing-page-design/SKILL.md`

**What changes:**

- **Vibe Discovery section** (~lines 19-205): Replace with a brief mandatory gate + reference to `../_shared/references/vibe-discovery.md`. Keep the "Shinjuku Runway" example inline as a quick reference, or move it to the shared file.
- **Anti-AI-Slop Principles** (~lines 267-295): Replace with reference to `frontend-design` skill's guidelines. Keep a brief note about landing-page-specific icon set recommendations.
- **Inspiration Starters** (~lines 164-184): Remove — now in shared aesthetic library.

**What stays exactly as-is:**
- 50% Rule (line 209)
- Section Composition — all 7 sections (lines 214-265)
- Animation Vocabulary (lines 297-324)
- Implementation Workflow — 5 phases (lines 348-380)
- Prompt Patterns (lines 382-415)
- Quality Checklist (lines 417-433)
- Design Resources (lines 326-346)

### Modified: `.claude-plugin/marketplace.json`

Add `./skills/frontend-design` to the `design-skills` plugin group:

```json
{
  "name": "design-skills",
  "description": "Skills for creating distinctive, high-quality web designs.",
  "source": "./",
  "strict": false,
  "skills": ["./skills/frontend-design", "./skills/landing-page-design"]
}
```

No other plugin groups change.

## Skill Trigger Boundaries

Clear separation to avoid both skills triggering simultaneously:

| Signal | Triggers |
|--------|----------|
| "build a landing page", "marketing page", "conversion page", "SaaS homepage" | `landing-page-design` |
| "build a component", "create a page", "design this app", "frontend for X" | `frontend-design` |
| "build a landing page with custom components" | `landing-page-design` (it can reference frontend-design's aesthetic guidelines internally) |

## Attribution

Harper Reed's skill is the primary source for:
- Collision Method (vibe synthesis)
- 9-category Aesthetic Reference Library
- Anti-anti-slop rule
- Frontend Aesthetics Guidelines section

Credited in `frontend-design/SKILL.md` frontmatter via `adapted-from: harperreed/dotfiles/.claude/skills/frontend-design`.
