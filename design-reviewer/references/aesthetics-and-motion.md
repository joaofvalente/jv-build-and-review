# Aesthetics, Style & Motion Reference

Grounds the Feel pass and the Polish & Craft / Motion considerations in the rubric.
Also contains the full AI-generic pattern library referenced from `visual-rubric.md`.

---

## Aesthetic Quality — What to Look For

### The "Point of View" Test

A design has a point of view when you can describe its aesthetic in two adjectives without
saying "clean" or "modern." Those words mean nothing — they're design's equivalent of
"nice personality."

Good adjectives to listen for: **raw and confident**, **warm and editorial**, **technical
and precise**, **playful and bold**, **quiet and considered**, **restless and layered**.

If the only words that come to mind are "clean," "simple," "professional," or "modern" —
the design has no point of view.

### Craft Signals — Where Senior Designers Look First

1. **Shadow quality** — physically plausible? Consistent light source? Or generic
   box-shadows copy-pasted from a tutorial?
2. **Border radius consistency** — radius scale, or random rounding?
3. **Color mixing** — neutrals at the right temperature? Warm gray on warm brand, cool gray
   on cool brand. Mixed temperatures create uncanny valley.
4. **Image art direction** — selected for design role (composition, palette, tone), or
   just for content?
5. **Typographic micro-decisions** — letter-spacing on all-caps labels, tabular figures
   in data tables, optical alignment of icons next to text, ligatures.
6. **Hover & focus refinement** — thoughtful treatments, or just `opacity: 0.8`?
7. **Empty states** — designed, or like error messages?

---

## Style Modes — Recognize and Evaluate

| Style | Done well | Done poorly |
|---|---|---|
| Minimal | Intentional emptiness, precise type, restrained palette | Empty because nothing was added; cold, unfinished |
| Bold/editorial | Strong type hierarchy, color used with confidence, dramatic scale shifts | Loud for loudness' sake; no reading order |
| Glassmorphic | Coherent light logic, subtle blur, works on all backgrounds | Frosted everything; fights legibility; theme rather than design |
| Organic / warm | Consistent irregular forms, earthy palette, humanist type | Random squiggles; mismatched warmth levels |
| Brutalist | Deliberate rawness, grid-breaking, typographic weight | Just unstyled HTML; not intentional |
| Corporate / enterprise | Clear IA, dense but organized, trust signals | Soulless; zero brand; indistinguishable from competitors |

---

## AI-Generated / Template Design — Full Pattern Library

### The 10 most reliable AI-template tells

1. **Inter + Tailwind default palette** — appears in 60%+ of AI-assisted web projects.
   Not wrong — invisible.
2. **Hero layout formula** — left: headline + subhead + 2 CTAs + social-proof logos. Right:
   product screenshot or illustration. Found on 80% of SaaS homepages.
3. **Three-column feature grids** — icon → heading → 2-sentence description, repeated 3 or
   6 times. Sometimes with gradient icon backgrounds.
4. **Gradient blobs / mesh gradients** — colorful amorphous blobs, typically purple/blue/pink.
   Once atmospheric, now a cliché.
5. **unDraw / Storyset-style illustrations** — generic vector illustrations of vaguely
   diverse people doing abstract things. Universally recognizable as stock.
6. **Grayscale "trusted by" logo strip** — under the hero. Always grayscale.
7. **Testimonial card grids** — avatar + name + company + star rating. Identical across the
   internet for 5+ years.
8. **Navbar formula** — logo left, links center/right, one CTA button. Universal default.
9. **Section-by-section alternating layout** — text left/image right, then image left/text
   right. Template logic, not design thinking.
10. **"We help [persona] [verb] [outcome]" hero headline** — written by AI, design built
    around the words with zero visual hierarchy differentiation.

### Intervention Points — How to Escape Generic

| Area | Template default | Intervention |
|---|---|---|
| Typography | Inter, all one weight family | Add one display or variable font with personality. Use only at large sizes. |
| Hero layout | Text left, media right | Full-bleed image + text overlay; center-weighted type; diagonal split; typographic-only hero |
| Color | Tailwind/Bootstrap palette | Custom 3-color palette from the brand brief. Name the colors. Give them purpose. |
| Section layout | Alternating left/right blocks | Asymmetric grid; full-bleed moments; varying density (sparse → dense) |
| Illustrations | unDraw / stock | Custom iconography; photography with art direction; abstract graphics; data viz as art |
| Hover states | Opacity / background change | Transform; reveal; underline draw; color shift; micro-animation |
| Cards | Identical card grids | Card hierarchy (hero card + supporting); editorial mixing; breaking the card border |

---

## Motion — Timing & Easing Reference

### Duration Scale

| Category | Duration | Examples |
|---|---|---|
| Instant feedback | 80–120ms | Button press, toggle flip, checkbox |
| Micro-interactions | 150–200ms | Hover state, tooltip appear, focus ring |
| Component transitions | 200–350ms | Modal open, dropdown expand, tab change |
| Screen / page transitions | 300–500ms | Route change, slide-in panel, wizard step |
| Deliberate reveals | 500–800ms | Staggered content entrance, feature spotlight |
| Narrative / onboarding | 600–1200ms | First-load animations, empty state reveal |

**Rule:** if duration > 500ms and it's not a first-load or narrative moment — question it.
Every millisecond of animation is a millisecond the user is waiting.

### Easing Reference

| Easing | Curve | Use case |
|---|---|---|
| ease-out | Starts fast, ends slow | Elements *entering*. Feels decisive and responsive. |
| ease-in | Starts slow, ends fast | Elements *exiting*. Feels like they're rushing away. |
| ease-in-out | Slow / fast / slow | Cross-fades, moving elements across the screen |
| spring (bouncy) | Overshoots then settles | Playful products, success states, game-like interactions |
| linear | Constant speed | Progress bars, loading spinners (continuous time only) |

**Never use linear for UI transitions** — it feels robotic and lifeless.

### Motion Purpose Framework

Every animation should pass at least one of these tests:

1. **Feedback** — confirms the user's action was registered
2. **Spatial relationship** — shows where something came from or went to
3. **Hierarchy** — choreographed stagger shows what's more important
4. **Perceived performance** — makes loading feel shorter (skeletons, progressive reveal)
5. **Delight** — a moment of craft that makes the product feel alive (max 2–3 per product)

If an animation doesn't serve at least one — cut it.

### Motion Accessibility Rules

- Always implement `prefers-reduced-motion` — substitute cross-fades for transforms
- No flashing content > 3 times per second (seizure risk)
- Avoid scroll-jacking — only with caution and a clear UX rationale
- Parallax should be subtle (max 20% speed differential) or off by default with opt-in

### Motion Language Red Flags

- Animations that fire on every scroll event, constantly competing for attention
- Inconsistent easing across components, with no rationale
- Entrance animations on above-the-fold content (delay access to content)
- Loading animations that loop > 2–3 seconds before content appears
- Hover effects that require precision to trigger (too small / too fast)
- Same component animating differently in different contexts

---

## Brand Feeling — Emotional Design Vocabulary

### Mapping Emotion to Visual Language

| Target emotion | Type direction | Color direction | Motion direction |
|---|---|---|---|
| Trust / stability | Serif or clean sans, medium weight, generous spacing | Navy, deep green, neutral gray, no bright primaries | Slow, deliberate, ease-in-out |
| Energy / excitement | Bold weight, tight spacing, display fonts | High saturation, bright primaries, strong contrasts | Fast, snappy, spring easing |
| Calm / wellness | Light to regular weight, wide tracking, rounded forms | Warm neutrals, sage, dusty rose, sky blue | Gentle, long ease-outs, soft spring |
| Premium / luxury | Elegant serif or restrained sans, light weight at large sizes | Near-black, cream, single gold or warm accent | Slow, cinematic |
| Technical / precision | Monospace accents, tabular figures, structured grid | Cool grays, electric blue, muted palette | Crisp, minimal, mechanical |
| Playful / creative | Variable fonts, expressive type mixing, illustrated | Unexpected combinations, hand-crafted palette | Bouncy spring, staggered, delightful |

### The 3-Second Brand Test

Show the hero section without the logo. Which does a new viewer say?
- "I know exactly what kind of company this is" ✅
- "Could be a lot of things, hard to tell" ⚠️
- "Another SaaS company" ❌

If the answer isn't the first one — the brand feeling is not coming through the design.

### Brand Consistency Across Screens

The emotional character should be consistent from the first screen to the deepest settings
page. Common failure modes:

- Strong brand on the marketing homepage; vanilla UI inside the app
- Different emotional registers across breakpoints (bold on desktop, timid on mobile)
- Brand identity confined to the header, absent from content and interactive elements
