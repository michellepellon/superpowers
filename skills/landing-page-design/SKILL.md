---
name: landing-page-design
description: >
  Create high-converting, visually distinctive landing pages. Use when building
  marketing pages, product launches, SaaS homepages, or any single-page
  conversion-focused website. Guides section-by-section composition with
  anti-AI-slop principles.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  adapted-from: 2389-research/claude-plugins/landing-page-design
---

# Landing Page Design

## Overview
Build landing pages that convert AND captivate. This skill combines conversion-focused structure with distinctive visual design to create pages that stand out in an AI-saturated world. The goal: pages worth $50-100 that you'd be proud to sell.

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

## The 50% Rule
**Spend 50% of your time on the hero section.** It's the cover image for social media, the first impression, the hook. Everything else flows from getting the hero right.

## Section Composition (Top to Bottom)

### 1. Hero Section (Primary Focus)
The make-or-break element. Must contain:
- **Headline**: Sharp, benefit-driven hook (reference H1 Gallery for inspiration)
- **Subheadline**: Supporting context, 1-2 sentences max
- **CTA Button(s)**: Primary action + optional secondary
- **Social Proof**: Logo marquee, testimonials, or trust badges
- **Visual Element**: Product shot, illustration, or animated background

**Hero Variations**:
- Split layout (text left, visual right)
- Centered with floating elements
- Full-bleed background with overlay text
- Asymmetric with decorative elements

### 2. Features/Benefits Section
Show what the product does. Options:
- **Bento Grid**: Cards in asymmetric layout (popularized by Apple)
- **Alternating Rows**: Image + text, flipping sides
- **Icon Grid**: Simple icons with short descriptions
- **Interactive Cards**: Hover states, micro-animations

### 3. Social Proof Section
Build trust through:
- Logo carousel (marquee animation)
- Testimonial cards with photos
- Stats/metrics with animated counters
- Case study snippets

### 4. How It Works Section
Step-by-step explanation:
- Numbered steps (01, 02, 03 pattern adds sophistication)
- Sticky scrolling with progressive reveal
- Timeline or flowchart visualization

### 5. Pricing Section (if applicable)
- 2-3 tier comparison
- Highlighted "recommended" tier
- Feature comparison table
- FAQ accordion below

### 6. CTA Section
Final conversion push:
- Repeat value proposition
- Strong headline
- Single focused action
- Urgency elements (if authentic)

### 7. Footer
- Navigation links
- Social icons
- Legal links
- Optional newsletter signup

## Anti-AI-Slop Principles

For the full anti-slop guidelines (fonts, colors, icons, layouts), see the `frontend-design` skill's guidelines.

**Landing-page-specific notes:**
- Icons: Use Iconify Solar (multiple styles), Heroicons, or Phosphor — avoid Lucide (overused in AI-generated pages)
- For icon integration: use Iconify's unified API for access to all sets

## Animation Vocabulary

### Entrance Animations
- `fade-in`: Simple opacity transition
- `blur-in`: Starts blurred, sharpens
- `slide-in`: Direction-based entrance
- `scale-in`: Grows from small to full size
- `stagger`: Sequential reveal of child elements

### Continuous Animations
- `marquee`: Infinite horizontal scroll (logos, testimonials)
- `beam`: Light traveling along a path/border
- `pulse`: Subtle scale/opacity breathing
- `float`: Gentle up/down movement
- `rotate`: Continuous spin (icons, decorations)

### Interactive Animations
- `hover-lift`: Subtle Y translation + shadow
- `hover-glow`: Border/shadow color change
- `hover-reveal`: Hidden element appears
- `click-ripple`: Material-style feedback

### Decorative Elements
- Vertical grid lines (container-size based)
- Noodles/curved connectors between elements
- Gradient orbs/blobs in background
- Grain/noise texture overlay
- Geometric shapes (circles, rectangles with rounded corners)

## Design Resources

### Hero Inspiration
- **Superhero** (superhero.design): Curated hero sections
- **Dribbble**: Search "hero section", "landing page"
- **Awwwards**: Award-winning designs

### Section Patterns
- **Mobin**: Real websites with section breakdowns
- **Bento Grids**: Card layout inspiration
- **CTA Gallery**: Call-to-action patterns

### Typography
- **Google Fonts**: Free, AI-accessible fonts
- **Fontshare**: Free quality fonts
- **H1 Gallery**: Headline inspiration

### Icons & Logos
- **Iconify**: Unified icon API (Solar, Heroicons, etc.)
- **Simple Icons**: Brand logos (SVG)
- **Heroicons**: Tailwind's icon set

## Implementation Workflow

### Phase 1: Research & Collect
1. Gather 5-10 hero screenshots as wireframes
2. Identify section patterns needed
3. Choose icon set and font pairing
4. Define color palette

### Phase 2: Hero Development
1. Generate hero from best reference screenshot
2. Iterate: change colors, fonts, layouts
3. Add animations (beam, fade-in, etc.)
4. Add decorative elements (noodles, grids, numbers)
5. Refine until distinctive

### Phase 3: Section Build-Out
1. Add sections one at a time (not all at once)
2. Reference specific components/screenshots per section
3. Maintain color/font consistency from hero
4. Add section-specific animations

### Phase 4: Polish
1. Fix responsive breakpoints (mobile, tablet, desktop)
2. Replace placeholder images with real/quality assets
3. Optimize animations for performance
4. Test all interactive elements

### Phase 5: Presentation
1. Create cover screenshot with infinity canvas layout
2. Show hero prominently
3. Include mobile and desktop views
4. Add subtle background (blurred gradient, pattern)

## Prompt Patterns

### Hero Generation
```
Create a hero section for [PRODUCT TYPE].
Change text, names, and numbers to fit [BRAND].
Use Iconify Solar icons (duotone style).
Use [FONT] for headlines.
Add vertical container-size grid lines.
Add 01, 02, 03 step indicators for sophistication.
Use [COLOR] as primary, dark mode.
```

### Section Addition
```
Adapt a new [SECTION TYPE] section.
Match the hero's color scheme and typography.
Use marquee animation for logos.
Add fade-in blur-in entrance animation.
Keep the hero exactly as is.
```

### Animation Enhancement
```
Add beam animation to the primary button border.
The beam should be 1px, continuously traveling around the pill shape.
Add a subtle hover-lift effect to feature cards.
```

### Negative Prompts (What NOT to change)
```
Don't change the hero section.
Keep the navbar exactly as is.
Don't modify the existing animations.
```

## Quality Checklist

### Visual Distinction
- [ ] No generic purple gradients
- [ ] Non-default icon set (not Lucide)
- [ ] Distinctive font pairing
- [ ] At least one "memorable" element
- [ ] Consistent color system via CSS variables

### Technical Quality
- [ ] Mobile responsive (no horizontal scroll)
- [ ] All images loading (no broken placeholders)
- [ ] Animations performant (no jank)
- [ ] Accessible color contrast
- [ ] Fast initial load

### Conversion Optimization
- [ ] Clear value proposition above fold
- [ ] Single primary CTA visible
- [ ] Social proof present
- [ ] Logical information hierarchy
- [ ] No friction to main action
