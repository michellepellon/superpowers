# Frontend Design Skill Integration — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Integrate Harper Reed's frontend-design skill into superpowers via shared references architecture, producing two focused skills with no content duplication.

**Architecture:** Extract shared Vibe Discovery and Aesthetic Reference Library into `skills/_shared/references/`. Create new `skills/frontend-design/SKILL.md` for general frontend work. Modify existing `skills/landing-page-design/SKILL.md` to reference shared content instead of inlining it.

**Tech Stack:** Markdown skill files, YAML frontmatter, JSON plugin registry.

**Spec:** `docs/superpowers/specs/2026-03-22-frontend-design-integration-design.md`

---

### Task 1: Create shared Vibe Discovery reference

**Files:**
- Create: `skills/_shared/references/vibe-discovery.md`

**Source material:**
- Existing: `skills/landing-page-design/SKILL.md` lines 19-205 (Vibe Discovery process, example, inspiration starters, anti-convergence rules)
- External: [Harper Reed's frontend-design SKILL.md](https://github.com/harperreed/dotfiles/blob/master/.claude/skills/frontend-design/SKILL.md) — Vibe Discovery section (Steps 1-5, Collision Method, Freshness Check, Minimal Context fallback)

- [ ] **Step 1: Create `skills/_shared/references/` directory**

```bash
mkdir -p skills/_shared/references
```

- [ ] **Step 2: Write `vibe-discovery.md`**

Create `skills/_shared/references/vibe-discovery.md` with this structure:

```markdown
# Vibe Discovery Process

MANDATORY: Run this process before writing any frontend code.

## Step 1: Gather Context (Ask the User)

[Landing-page-design's 4-question approach:]
- Q1: What's one real-world place or object this brand would be?
  - Include the "push away from design language" coaching from Harper
  - Include examples: "A Tokyo convenience store at 2am", etc.
- Q2: What's the ONE emotion someone should feel in the first 3 seconds?
  - Single word pick list
- Q3: Pick TWO unexpected influences to collide
  - Examples: "medical packaging + skateboard graphics", etc.
- Q4: What should this NEVER be mistaken for?
  - Name 2-3 specific anti-patterns

[Include Harper's follow-up questions for deeper dig:]
- "What's a physical space that has the same energy?"
- "What would make someone screenshot this?"
- "What's the worst version of this?"

## Step 2: Invent the Vibe (Collision Method)

[Harper's Collision Method:]
1. Pick a physical texture/material from their answers
2. Pick an energy/movement quality from their description
3. Combine into a vibe seed: "Velvet Frenetic", "Cardboard Surgical"

This is the vibe seed. Everything else derives from it.

## Step 3: Derive the Aesthetic

From the vibe seed, derive each element:
- Colors: What colors exist in the collision? Extract 3-4, invent fresh hex codes, name the palette after the vibe
- Typography: What does text FEEL like? Search Google Fonts with the vibe's energy
- Layout: How does the material organize space?
- Motion: How does the energy quality move? Pick ONE signature motion

## Step 4: Write the Vibe Spec

[Landing-page-design's template, extended with MATERIAL and ENERGY fields:]

VIBE NAME: [invented collision name]
MATERIAL: [physical texture driving visual language]
ENERGY: [movement quality driving interaction]
REFERENCE: [place/object from Q1]
EMOTION: [from Q2]
COLLISION: [from Q3]
ANTI-PATTERNS: [from Q4]

COLORS: [hex codes + why, named palette]
TYPOGRAPHY: [specific fonts + why]
LAYOUT: [density, shapes, signature element]
MOTION: [level, signature animation]
WILDCARD: [one unexpected detail]

## Step 5: Freshness Check

[Harper's 8-item checklist:]
- [ ] Vibe name never used before
- [ ] Did NOT reuse hex codes from previous projects
- [ ] Did NOT default to comfortable fonts (Inter? Space Grotesk? JetBrains Mono?)
- [ ] Material + energy collision is actually visible in choices
- [ ] Could NOT be mistaken for previous work
- [ ] Included a wildcard that surprises even me
- [ ] User's actual words and context are driving the aesthetic
- [ ] Did NOT mentally scan a list of "aesthetic styles" and pick closest

## Anti-Convergence Rules

[From landing-page-design:]
1. No hex code memory — generate colors fresh from the reference
2. Font rotation required — cannot use same display font in consecutive projects
3. Collision must show — if someone can't see BOTH influences, push harder
4. Wildcard is mandatory — every vibe needs one element that doesn't "fit"
5. Name it — an unnamed vibe becomes generic

## When Context Is Minimal

[Harper's generative fallback:]
If user says "just build it" with no specifics, still ask:
1. "What's this for? Even one sentence."
2. "What's the energy — fast and loud, or slow and quiet?"
3. "What would make you hate it?"

If they truly refuse, derive from the PROJECT CONTENT itself — what it does, who it serves, what world it lives in. Never fall back on a preset list.

## Example: Shinjuku Runway

[Landing-page-design's existing example, moved here:]
Q1: "A Japanese train station at rush hour"
Q2: "Confident"
Q3: "Transit signage + haute couture"
Q4: "A meditation app, anything whimsical, startup-bro tech"

[Full vibe spec from landing-page-design lines 130-160]
```

Write the FULL content — the above is the structural outline. Fill in complete text from both source skills.

- [ ] **Step 3: Commit**

```bash
git add skills/_shared/references/vibe-discovery.md
git commit -m "feat: add shared vibe-discovery reference

Merges landing-page-design's structured questions with Harper Reed's
Collision Method and expanded Freshness Check."
```

---

### Task 2: Create shared Aesthetic Reference Library

**Files:**
- Create: `skills/_shared/references/aesthetic-library.md`

**Source material:**
- External: [Harper Reed's frontend-design SKILL.md](https://github.com/harperreed/dotfiles/blob/master/.claude/skills/frontend-design/SKILL.md) — Aesthetic Reference Library (9 categories with fonts, palettes, CSS techniques)
- Existing: `skills/landing-page-design/SKILL.md` lines 164-184 (Inspiration Starters)
- External: Harper Reed's "Generating Fresh References" section

- [ ] **Step 1: Write `aesthetic-library.md`**

Create `skills/_shared/references/aesthetic-library.md` with this structure:

```markdown
# Aesthetic Reference Library

## STOP: Have You Completed Vibe Discovery?

If you haven't completed the Vibe Discovery process and written a Vibe Spec, go back. This library is NOT a menu of vibes to pick from. It is a parts bin — font names, hex codes, and CSS techniques you can cannibalize AFTER you've invented your own direction.

## Rules for Using This Library

- You MUST have a completed Vibe Spec before looking here
- Cherry-pick individual techniques or font pairings that serve YOUR vibe
- NEVER adopt a whole section as your aesthetic
- If your final design maps cleanly to one of these categories, you failed Vibe Discovery — start over

## Categories

### 1. Warm / Organic
[Harper's full content: fonts, palette with hex codes, layout, CSS techniques]

### 2. Luxury / Refined
[Harper's full content]

### 3. Playful / Toy-like
[Harper's full content]

### 4. Art Deco / Geometric
[Harper's full content]

### 5. Soft / Pastel / Dreamy
[Harper's full content]

### 6. Retro-Futuristic / Sci-Fi
[Harper's full content]

### 7. Editorial / Magazine
[Harper's full content]

### 8. Maximalist / Eclectic
[Harper's full content]

### 9. Industrial / Utilitarian
[Harper's full content]

## When Stuck: Inspiration Starters

### Spaces
Night market in Bangkok | Empty museum at closing | Airport lounge at 4am | ...
[Full list from landing-page-design lines 167-170]

### Objects
1980s synthesizer | Surgical instruments | Vintage luggage | ...
[Full list from landing-page-design lines 173-176]

### Eras/Movements
Soviet constructivism | Memphis design | Swiss international | ...
[Full list from landing-page-design lines 179-184]

## Generating Fresh References

[Harper's generative process:]
1. Take user's domain/product — what's a surprising real-world analog for this ACTION?
2. Think about the USER's physical context — where are they? What does that space feel like?
3. Think about TIME — when does this get used? What's the light like?
4. Combine the most interesting answers into your material + energy collision.
```

Write the FULL content — copy Harper's 9 categories verbatim (fonts, hex codes, CSS techniques). The above is structural outline only.

- [ ] **Step 2: Commit**

```bash
git add skills/_shared/references/aesthetic-library.md
git commit -m "feat: add shared aesthetic reference library

Nine categories from Harper Reed's frontend-design skill plus
inspiration starters from landing-page-design."
```

---

### Task 3: Create `frontend-design` skill

**Files:**
- Create: `skills/frontend-design/SKILL.md`

**Source material:**
- External: [Harper Reed's frontend-design SKILL.md](https://github.com/harperreed/dotfiles/blob/master/.claude/skills/frontend-design/SKILL.md) — Frontend Aesthetics Guidelines, Anti-AI-Slop, Choosing an Aesthetic
- Existing: `skills/landing-page-design/SKILL.md` lines 269-295 (Anti-AI-Slop icon alternatives)
- Spec: `docs/superpowers/specs/2026-03-22-frontend-design-integration-design.md` section "New: skills/frontend-design/SKILL.md"

- [ ] **Step 1: Create directory**

```bash
mkdir -p skills/frontend-design
```

- [ ] **Step 2: Write `SKILL.md`**

Create `skills/frontend-design/SKILL.md` with this structure:

```yaml
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
```

Content sections (see spec for details):

1. **Overview** — 2-3 sentences: scope is general frontend (components, pages, apps), not landing pages. Implement real working code with exceptional attention to aesthetic details.

2. **MANDATORY: Vibe Discovery (Do This First)** — Gate: "BEFORE writing any code, run the Vibe Discovery process." Then: "See the full process in `../../_shared/references/vibe-discovery.md`."

3. **Frontend Aesthetics Guidelines** — From Harper's skill, full content:
   - Typography: distinctive, characterful choices; pair display + body; never generic
   - Color & Theme: CSS variables; dominant colors with sharp accents outperform timid palettes
   - Motion: CSS-only for HTML, Motion library for React; one well-orchestrated page load > scattered micro-interactions; scroll-triggering and hover states that surprise
   - Spatial Composition: unexpected layouts, asymmetry, overlap, diagonal flow, grid-breaking, generous negative space OR controlled density
   - Backgrounds & Visual Details: gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders, custom cursors, grain overlays

4. **Anti-AI-Slop Principles** — Merged from both skills:
   - Icons: avoid Lucide (overused). Use Iconify Solar, Heroicons, Phosphor, custom SVGs
   - Fonts: kill Inter/Roboto/Arial/system fonts. No defaulting to comfortable choices
   - Colors: no purple gradients on white
   - Anti-anti-slop: dark+monospace+neon (terminal/hacker aesthetic) is just as overplayed. Not edgy, not distinctive — it's the default fallback when trying too hard to avoid looking generic
   - Layouts: break the grid
   - Complexity matching: maximalist designs need elaborate code; minimalist needs restraint and precision

5. **Aesthetic Reference Library** — "For implementation raw materials, see `../../_shared/references/aesthetic-library.md`."

6. **Choosing an Aesthetic** — Harper's guidance:
   - Let subject matter drive the aesthetic, not your default comfort zone
   - "Roll the dice" — if instinct says dark, go light; if reaching for monospace, pick a serif
   - Fight your own patterns
   - Claude is capable of extraordinary creative work — commit fully to a distinctive vision

- [ ] **Step 3: Commit**

```bash
git add skills/frontend-design/SKILL.md
git commit -m "feat: add frontend-design skill

General-purpose frontend design skill adapted from Harper Reed's
dotfiles. References shared vibe-discovery and aesthetic-library."
```

---

### Task 4: Modify `landing-page-design` to reference shared content

**Files:**
- Modify: `skills/landing-page-design/SKILL.md`

**What to preserve (do NOT touch):**
- Lines 209-265: 50% Rule + Section Composition
- Lines 297-324: Animation Vocabulary
- Lines 326-346: Design Resources
- Lines 348-380: Implementation Workflow
- Lines 382-415: Prompt Patterns
- Lines 417-433: Quality Checklist

**Note:** Steps 1-3 modify the same file. Line numbers refer to the original file. Work bottom-to-top (Step 3 first, then Step 2, then Step 1) to avoid line drift, OR re-read the file between steps.

- [ ] **Step 1: Replace Vibe Discovery section**

Replace lines 19-205 (from `## MANDATORY: Vibe Discovery` through `---` before `## The 50% Rule`) with:

```markdown
## MANDATORY: Vibe Discovery (Do This First)

**BEFORE writing any code, you MUST run the Vibe Discovery process.** This generates a UNIQUE aesthetic direction every time — no two landing pages should look alike.

See the full process: `../../_shared/references/vibe-discovery.md`

The process will guide you through:
1. Gathering context via structured questions (place/object, emotion, collision, anti-patterns)
2. Inventing a unique vibe via the Collision Method (material + energy = vibe seed)
3. Deriving colors, typography, layout, and motion from the vibe seed
4. Writing a Vibe Spec
5. Running the Freshness Check to prevent convergence

---
```

- [ ] **Step 2: Replace Anti-AI-Slop Principles section**

Replace lines 267-295 (from `## Anti-AI-Slop Principles` through the end of that section) with:

```markdown
## Anti-AI-Slop Principles

For the full anti-slop guidelines (fonts, colors, icons, layouts), see the `frontend-design` skill's guidelines.

**Landing-page-specific notes:**
- Icons: Use Iconify Solar (multiple styles), Heroicons, or Phosphor — avoid Lucide (overused in AI-generated pages)
- For icon integration: use Iconify's unified API for access to all sets
```

- [ ] **Step 3: Remove Inspiration Starters**

Remove lines 164-184 (the "Inspiration Starters" subsection) — this content now lives in `skills/_shared/references/aesthetic-library.md` under "When Stuck."

- [ ] **Step 4: Verify preserved sections are intact**

Read the modified file and confirm these sections are unchanged:
- 50% Rule
- Section Composition (all 7 sections)
- Animation Vocabulary
- Design Resources
- Implementation Workflow (5 phases)
- Prompt Patterns
- Quality Checklist

- [ ] **Step 5: Commit**

```bash
git add skills/landing-page-design/SKILL.md
git commit -m "refactor: landing-page-design references shared content

Replace inline Vibe Discovery and Anti-AI-Slop with references to
shared files. Conversion-specific content unchanged."
```

---

### Task 5: Update marketplace.json

**Files:**
- Modify: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Add frontend-design to design-skills plugin group**

In `.claude-plugin/marketplace.json`, change the `design-skills` plugin's `skills` array from:

```json
"skills": ["./skills/landing-page-design"]
```

to:

```json
"skills": ["./skills/frontend-design", "./skills/landing-page-design"]
```

- [ ] **Step 2: Commit**

```bash
git add .claude-plugin/marketplace.json
git commit -m "feat: register frontend-design in design-skills plugin group"
```

---

### Task 6: Final verification

- [ ] **Step 1: Verify file structure**

```bash
find skills/_shared skills/frontend-design -type f
```

Expected:
```
skills/_shared/references/vibe-discovery.md
skills/_shared/references/aesthetic-library.md
skills/frontend-design/SKILL.md
```

- [ ] **Step 2: Verify landing-page-design preserved content**

Read `skills/landing-page-design/SKILL.md` and confirm:
- 50% Rule present
- All 7 Section Composition subsections present
- Animation Vocabulary present
- Implementation Workflow present
- Prompt Patterns present
- Quality Checklist present
- No Vibe Discovery inline content (replaced with reference)
- No Inspiration Starters inline content (removed)

- [ ] **Step 3: Verify marketplace.json**

Read `.claude-plugin/marketplace.json` and confirm `frontend-design` is in `design-skills` group.

- [ ] **Step 4: Verify cross-references resolve**

Confirm these referenced paths exist:
- From `frontend-design/SKILL.md`: `../../_shared/references/vibe-discovery.md` → `skills/_shared/references/vibe-discovery.md` ✓
- From `frontend-design/SKILL.md`: `../../_shared/references/aesthetic-library.md` → `skills/_shared/references/aesthetic-library.md` ✓
- From `landing-page-design/SKILL.md`: `../../_shared/references/vibe-discovery.md` → `skills/_shared/references/vibe-discovery.md` ✓
