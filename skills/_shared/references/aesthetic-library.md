# Aesthetic Reference Library

## STOP: Have You Completed Vibe Discovery?

This library is NOT a menu of vibes to pick from. It is a parts bin — font names, hex codes, and CSS techniques you can cannibalize AFTER you've invented your own direction.

If you haven't completed the Vibe Discovery process and written a Vibe Spec, go back and do that first. See `skills/_shared/references/vibe-discovery.md` for the full generative process.

## Rules for Using This Library

- You MUST have a completed Vibe Spec before looking here
- Cherry-pick individual techniques or font pairings that serve YOUR vibe
- NEVER adopt a whole section as your aesthetic — that's the exact problem this library exists to prevent
- If your final design maps cleanly to one of these categories, you failed the Vibe Discovery process. Start over.

## Categories

### 1. Warm / Organic

Think: bakery website, ceramics studio, wellness brand, farm-to-table restaurant

- **Fonts**: Fraunces (display) + Libre Baskerville (body), or Playfair Display + Source Serif Pro
- **Palette**: `#D4A373` warm sand, `#FAEDCD` cream, `#CCD5AE` sage, `#E9EDC9` soft olive, `#3D405B` deep slate
- **Layout**: Flowing, asymmetric sections with generous whitespace. Overlapping rounded images. Text wrapping around organic shapes.
- **CSS techniques**: `border-radius: 40% 60% 55% 45% / 60% 40% 45% 55%` for blob shapes, subtle `backdrop-filter: blur()`, warm `box-shadow` with brown tones, `mix-blend-mode: multiply` on overlapping photos

### 2. Luxury / Refined

Think: high-end hotel, jewelry brand, architecture firm, premium spirits

- **Fonts**: Cormorant Garamond (display) + Montserrat 300-weight (body), or Bodoni Moda + DM Sans
- **Palette**: `#1A1A1A` near-black, `#F5F0EB` warm white, `#C9A96E` muted gold, `#4A4A4A` charcoal, `#8B7355` bronze
- **Layout**: Extreme whitespace. Full-bleed hero images. Text set very large and very light-weight. Horizontal scroll sections. Elements placed with mathematical precision.
- **CSS techniques**: `letter-spacing: 0.3em` on uppercase labels, `font-weight: 200`, hairline borders (`0.5px`), smooth `scroll-snap-type`, images with `aspect-ratio: 3/4`, `mix-blend-mode: luminosity`

### 3. Playful / Toy-like

Think: children's app, party supply store, casual mobile game, snack brand

- **Fonts**: Baloo 2 (display) + Nunito (body), or Fredoka One + Quicksand
- **Palette**: `#FF6B6B` coral, `#4ECDC4` teal, `#FFE66D` sunny yellow, `#F7FFF7` mint white, `#2C3E50` grounding navy
- **Layout**: Tilted cards (`transform: rotate(-2deg)`), stacked/overlapping elements, big chunky buttons with visible borders, intentional "messiness."
- **CSS techniques**: `border: 3px solid`, thick `box-shadow: 6px 6px 0 #000` for pop-out effect, `border-radius: 1rem`, bouncy `cubic-bezier(0.34, 1.56, 0.64, 1)` transitions, wiggle `@keyframes` on hover, hand-drawn SVG borders

### 4. Art Deco / Geometric

Think: cocktail bar, jazz venue, museum exhibition, luxury stationery

- **Fonts**: Poiret One (display) + Raleway (body), or Josefin Sans + Lato
- **Palette**: `#0D1B2A` midnight blue, `#D4AF37` true gold, `#1B2838` dark teal, `#F0E6D3` parchment, `#C41E3A` ruby accent
- **Layout**: Strong symmetry. Geometric dividers (chevrons, fan shapes). Layered borders and frames around content. Centered, formal composition.
- **CSS techniques**: Repeating SVG patterns as backgrounds, `clip-path: polygon()` for angular sections, `linear-gradient` used for geometric stripes, gold `border-image`, CSS `columns` for text, ornamental `::before`/`::after` pseudo-elements

### 5. Soft / Pastel / Dreamy

Think: skincare brand, meditation app, baby products, stationery shop

- **Fonts**: Outfit (display) + Karla (body), or Sora + IBM Plex Sans
- **Palette**: `#E8D5F5` lavender, `#F5E6CC` peach, `#D4E8E0` mint, `#FFF5F5` blush white, `#5C5470` muted purple
- **Layout**: Rounded everything. Floating cards with soft shadows. Generous padding. Content feels like it's resting on clouds.
- **CSS techniques**: `box-shadow: 0 20px 60px rgba(0,0,0,0.05)`, large `border-radius: 2rem`, subtle `background: linear-gradient(135deg, ...)` behind sections, `backdrop-filter: blur(20px)` glassmorphism but with pastel tints not dark glass, `filter: saturate(0.9)` for softness

### 6. Retro-Futuristic / Sci-Fi

Think: space tourism, vintage tech revival, synth music, concept car reveal

- **Fonts**: Orbitron (display) + Exo 2 (body), or Audiowide + Rajdhani
- **Palette**: `#0A0E27` void blue, `#FF6F61` retro coral, `#E8D44D` signal yellow, `#2DE2E6` electric cyan, `#F5E6CA` analog cream
- **Layout**: Asymmetric grid with angled dividers. Data-viz inspired decorative elements (arcs, circles, grid overlays). Split-screen compositions.
- **CSS techniques**: `conic-gradient()` for radar/dial effects, animated `stroke-dashoffset` on SVG circles, `clip-path` angled sections, scanline overlay with repeating-linear-gradient, `text-shadow` glow with theme accent, CSS `counter()` for numbering sections

### 7. Editorial / Magazine

Think: longform journalism, fashion lookbook, photo essay, cultural review

- **Fonts**: Playfair Display (headlines) + Charter or Lora (body), or DM Serif Display + Atkinson Hyperlegible
- **Palette**: `#1A1A1A` rich black, `#FAFAF7` warm off-white, `#C41E3A` editorial red, `#6B705C` olive grey, `#E8E0D5` newsprint
- **Layout**: Multi-column text. Pull quotes in oversized italic. Full-bleed images breaking the grid. Drop caps. Sidebar annotations.
- **CSS techniques**: CSS `columns: 2` with `column-rule`, `float` for pull quotes with `shape-outside`, `initial-letter: 3` for drop caps, large `font-size: clamp(3rem, 8vw, 7rem)` headlines, `mix-blend-mode: difference` for text over images, `hyphens: auto`

### 8. Maximalist / Eclectic

Think: street festival, record store, zine, independent fashion label

- **Fonts**: Unbounded (display) + Space Mono (body), or Rubik Glitch + Work Sans
- **Palette**: `#FF3366` hot pink, `#33FF57` electric green, `#FFD700` gold, `#1A1A2E` deep purple-black, `#FF6B35` tangerine — use ALL of them boldly
- **Layout**: Overlapping layers. Rotated elements. Mixed grid sizes. Collage-like composition. Things intentionally "breaking" out of containers.
- **CSS techniques**: `mix-blend-mode: screen` and `difference` on overlapping elements, `transform: rotate()` scattered, CSS `mask-image` for cutout effects, layered `position: absolute` compositions, `animation` running perpetually on decorative elements, clashing `font-size` contrasts (12px next to 120px)

### 9. Industrial / Utilitarian

Think: construction company, warehouse sale, municipal service, maker space

- **Fonts**: IBM Plex Mono (labels) + IBM Plex Sans (body), or Overpass Mono + Overpass
- **Palette**: `#F5F5F0` concrete, `#333333` asphalt, `#FFB800` caution yellow, `#E63946` safety red, `#457B9D` steel blue
- **Layout**: Visible grid structure. Labels and metadata exposed. Numbered sections. Dense but organized information hierarchy.
- **CSS techniques**: Visible `outline: 1px dashed` on grid cells, `text-transform: uppercase` with wide `letter-spacing` on labels, `font-variant-numeric: tabular-nums`, `border-left: 4px solid` accent bars, monochrome photos with `filter: grayscale(1)`, data-attribute-driven `::before` content labels

## When Stuck: Inspiration Starters

These lists are physical-world anchors to spark unexpected collisions during Vibe Discovery. Pick one and ask: "What would a website feel like if it had the energy of this?"

### Spaces

Night market in Bangkok | Empty museum at closing | Airport lounge at 4am |
Vintage record store | Hospital waiting room | Casino floor |
Greenhouse in winter | Subway platform | Observatory dome |
Abandoned factory | Luxury yacht interior | 24-hour laundromat |
Library rare books room | Auto body shop | Space station module

### Objects

1980s synthesizer | Surgical instruments | Vintage luggage |
Racing motorcycle | Antique compass | Industrial loom |
Neon sign | Typewriter | Scientific glassware |
Leather-bound book | Circuit board | Porcelain dishware

### Eras/Movements

Soviet constructivism | Memphis design | Swiss international |
Art nouveau | Bauhaus | De Stijl |
Googie architecture | Streamline moderne | Brutalism |
Japanese metabolism | Scandinavian modernism | Italian futurism
