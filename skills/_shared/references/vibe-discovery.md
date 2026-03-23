# Vibe Discovery Process

**BEFORE writing any code, you MUST run the Vibe Discovery process.** This is not a lookup table or a style picker. It is a creative process that generates a UNIQUE aesthetic direction every time by grounding design decisions in real-world references and personal context.

The goal: No two pages should look alike, even for similar products.

---

## Step 1: Conversational Dig (Ask the User)

**Do NOT skip this.** Have an actual back-and-forth with the user. Ask questions one or two at a time, not as a wall of text. Use their answers to generate follow-ups. The goal is to extract something *specific and personal* -- not a generic category.

### Core Questions

**Q1: What's one real-world place or object this brand would be?**
Push them away from design language. "A Tokyo convenience store at 2am" is better than "modern and clean." If they say something generic ("professional", "sleek"), push back: *"Professional like a law firm lobby, or professional like a SpaceX launch room? Those are very different vibes."*

**Q2: What's the ONE emotion someone should feel in the first 3 seconds?**
Pick ONE: Calm. Energized. Curious. Trusted. Delighted. Impressed. Rebellious. Nostalgic. Inspired. Amused. Sophisticated. Welcomed. Intrigued. Confident.

**Q3: Pick TWO unexpected influences to collide:**
Examples: "medical packaging + skateboard graphics", "spreadsheets + street art", "luxury hotel + punk zine", "NASA mission control + kindergarten", "Japanese convenience store + Victorian library"

**Q4: What should this page NEVER be mistaken for?**
Name 2-3 specific things to actively avoid. "A crypto project", "A wellness app", "Something made by a bank", "Anything with purple gradients"

### Follow-Up Questions (Pick 2-3 Based on Their Answers)

Use these to dig deeper once you have initial answers. Choose the ones most relevant to what the user has already said:

- "Who is the actual human using this, and what are they doing right before they open it?" Context matters more than demographics. "A tired nurse checking schedules at 6am" gives you more than "healthcare professionals aged 25-45."
- "What would make someone screenshot this and send it to a friend?"
- "Is there a movie, album, restaurant, or store that has the feel you want?"
- "What's something you've seen recently that made you think 'yes, THAT energy'?"
- "What's the worst version of this? What would make you cringe?"

---

## Step 2: Invent the Vibe (The Collision Method)

**You must CREATE a new aesthetic direction every time.** Do not pattern-match to categories you have seen before. Do not mentally scan a list of "aesthetic styles" and pick the closest one.

Take TWO things from the user's answers that don't obviously go together and smash them:

1. Pick a **physical texture or material** from their world (concrete, linen, chrome, cardboard, velvet, terracotta, acrylic, denim, lacquer, moss, brushed steel, washi paper, raw silk)
2. Pick an **energy or movement quality** from their description (frenetic, glacial, bouncy, surgical, tidal, flickering, heavy, weightless, staccato, molten, crisp, restless)
3. Combine them into something that doesn't exist yet: "Velvet Frenetic", "Cardboard Surgical", "Chrome Tidal"

This is your **vibe seed**. Everything else derives from it.

The material comes from the physical reference in Q1 and the collision in Q3. The energy comes from the emotion in Q2 and the overall feel of the conversation. Don't force it -- let the most interesting tension between the user's answers drive the combination.

---

## Step 3: Derive the Aesthetic From the Vibe Seed

Take your invented vibe and derive EACH element from it. Do NOT consult any reference library yet -- that is for implementation details later, not for direction-setting.

**COLOR INVENTION** (Derive from the collision, don't use memorized palettes)
- What colors exist in the collision you invented? "Velvet Frenetic" leads to deep burgundy meeting electric yellow. "Cardboard Surgical" leads to kraft brown meeting clinical white with a sharp teal accent.
- Extract 3-4 colors that feel authentic to the material + energy tension.
- Invent specific hex codes fresh -- do not reuse codes from previous projects.
- Name your palette after the vibe, not a generic mood ("Scrubbed Kraft", "Bruised Neon", "Soft Concrete", "Platform Edge").

**TYPOGRAPHY INVENTION** (Match the voice to the collision)
- What does text FEEL like in this vibe? Heavy and planted? Thin and nervous? Hand-drawn and loose?
- Search Google Fonts with the vibe's energy, not with a category. If your first instinct is a font you have used before, discard it and keep looking.
- Consider: weight, width, contrast, quirks. Find a display font that embodies the collision from Q3.

**LAYOUT INVENTION** (Derive from the physical space)
- How does the vibe's material organize space? Velvet drapes -- layered, overlapping. Cardboard -- flat, stacked, visible edges. Chrome -- reflective, minimal, precise.
- Is it cramped or expansive? Grid-like or organic? Vertical or horizontal?
- What unexpected layout choice would embody the collision from Q3?

**MOTION INVENTION** (Match the emotion)
- How does the vibe's energy quality move? Frenetic = jittery micro-movements. Glacial = slow parallax. Surgical = precise snap-to. Tidal = rhythmic ease-in-out.
- What is ONE signature motion that defines this page?

---

## Step 4: Write Your Vibe Spec

Before coding, write this out explicitly:

```
VIBE NAME: [Your invented collision name, e.g. "Velvet Frenetic"]
MATERIAL: [The physical texture/material driving the visual language]
ENERGY: [The movement quality driving interaction and motion]
REFERENCE: [The place/object from Q1]
EMOTION: [From Q2]
COLLISION: [From Q3]
ANTI-PATTERNS: [From Q4]

COLORS:
- Primary: [hex] - [why this color, traced to the material/energy]
- Secondary: [hex] - [why]
- Background: [hex] - [why]
- Accent: [hex] - [why]
- Palette name: [evocative name derived from the vibe seed]

TYPOGRAPHY:
- Display: [specific font name] - [why it fits THIS vibe]
- Body: [specific font name] - [why]
- Character: [describe the voice]

LAYOUT:
- Density: [sparse/balanced/dense]
- Shapes: [sharp/rounded/organic/mixed]
- Signature element: [one unusual layout choice derived from the material]

MOTION:
- Level: [still/subtle/moderate/dynamic/chaotic]
- Signature animation: [one specific animation derived from the energy]

WILDCARD:
- One unexpected detail that doesn't "match" but makes it memorable
```

---

## Step 5: The Freshness Check

Before proceeding, verify ALL of these:

- [ ] My vibe name is something I have NEVER used before
- [ ] I did NOT reuse hex codes from previous projects
- [ ] I did NOT default to my comfortable fonts (Inter? Space Grotesk? JetBrains Mono? Nunito? Monospace-on-dark? If yes, start over.)
- [ ] The material + energy collision is actually visible in my design choices
- [ ] Someone could NOT mistake this for previous work
- [ ] I included a wildcard that surprises even me
- [ ] The user's actual words and context are driving the aesthetic, not a category I pattern-matched to
- [ ] I did NOT mentally scan a list of "aesthetic styles" and pick the closest one

---

## Example: Shinjuku Runway

**Q1 - Place/Object:** "A Japanese train station at rush hour"

**Q2 - Emotion:** "Confident"

**Q3 - Collision:** "Transit signage + haute couture"

**Q4 - Never mistaken for:** "A meditation app, anything whimsical, startup-bro tech"

**Generated Vibe Spec:**

```
VIBE NAME: Shinjuku Runway
MATERIAL: Enameled steel -- the smooth, hard surface of train signage panels
ENERGY: Precise -- the exact timing of a Yamanote Line departure
REFERENCE: Japanese train station at rush hour
EMOTION: Confident
COLLISION: Transit signage + haute couture
ANTI-PATTERNS: No soft gradients, no playful illustrations, no rounded friendly shapes

COLORS:
- Primary: #1a1a1a - the black of train doors
- Secondary: #f5f5f0 - platform concrete, worn smooth
- Background: #fafaf8 - fluorescent-lit white
- Accent: #e60012 - JR line red, commanding attention
- Palette name: "Platform Edge"

TYPOGRAPHY:
- Display: Darker Grotesque - confident, slightly condensed, European edge
- Body: Noto Sans JP - clean utility, transit-inspired
- Character: Authoritative but not cold. Clear. Directional.

LAYOUT:
- Density: Rich but organized - like a station map
- Shapes: Sharp with intentional rounded exceptions (like train windows)
- Signature element: Strong horizontal bands that divide sections like train schedules

MOTION:
- Level: Subtle but precise
- Signature animation: Elements slide in from the side like arriving trains - horizontal, smooth, with exact timing

WILDCARD:
- One element uses a fabric-like texture overlay - the haute couture collision
```

---

## The Anti-Convergence Rules

1. **No hex code memory** -- Generate colors fresh from the reference. Do not recall "my usual blue."
2. **Font rotation required** -- Cannot use the same display font in consecutive projects.
3. **Collision must show** -- If someone cannot see BOTH influences from Q3, push harder.
4. **Wildcard is mandatory** -- Every vibe needs one element that does not "fit" but makes it unique.
5. **Name it** -- An unnamed vibe becomes generic. A named vibe has identity.

---

## Minimal Context Fallback

If the user just says "build me a page" with no context, you STILL do the conversational dig. Ask:

1. "What's this for? Even one sentence helps."
2. "What's the energy -- fast and loud, or slow and quiet, or something else entirely?"
3. "What should this NEVER look like?"

If they truly refuse to engage, derive from the **project content itself**:

- What does the product DO? Find an unexpected real-world analog for that action.
- Who uses it? Imagine their world. What textures, colors, sounds exist there?
- What era or subculture would claim this product as their own?

Never fall back on a preset list of vibes. Generate a vibe seed from whatever you have, even if it is just a project name and a README.

### Generating Fresh References (When You Need Physical-World Anchors)

Do NOT use a static list. Instead, use this generative process:

1. Take the user's domain/product and think: "What's a surprising real-world analog for this ACTION?" A task manager could be an air traffic control tower, a short-order cook's ticket rail, a librarian's card catalog.
2. Think about the USER's physical context: "Where are they when they use this? What does that space smell/sound/feel like?"
3. Think about TIME: "When in the day/week/year does this get used? What's the light like? The mood?"
4. Combine the most interesting answers into your material + energy collision.
