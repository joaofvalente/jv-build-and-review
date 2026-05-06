# Visual Rubric

The 9-dimension weighted scoring rubric, binary pass/fail gates, AI-Generic Template
Detector, and scoring formula. Read this before any audit. Plan Runner's Tier-2 pass
gate is defined against this rubric.

---

## The 9 Dimensions (Weighted)

Score each dimension 1–10. Weight determines impact on the overall score.

### 1. Visual Hierarchy (15%)

The most important dimension. If the user can't instantly tell what matters most, nothing
else matters.

- Is the primary CTA the most visually prominent element?
- Do headings descend in size and weight?
- Do size, color, contrast, and position all reinforce the same priority order?
- Is there a single dominant focal point per screen or section?
- Does the eye flow naturally through the layout?

| Score | Meaning |
|-------|---------|
| 9–10  | Instant clarity. Every element knows its rank. Zero confusion. |
| 7–8   | Clear hierarchy with minor ambiguity in secondary elements. |
| 5–6   | Primary element identifiable but some elements compete for attention. |
| 3–4   | Multiple elements fight for dominance. User has to search for the CTA. |
| 1–2   | Everything is the same visual weight. No hierarchy exists. |

### 2. Consistency (15%)

Near-consistency is worse than no consistency — a 1–2px misalignment reads as carelessness.

- Does every instance of the same component look and behave identically?
- Are interactive states (hover, active, focus, disabled) consistent everywhere?
- Is icon style uniform (fill vs outline, stroke weight, size)?
- Are border radii consistent across the design?
- Is the shadow/elevation system consistent?
- Do animation timing and easing match across interactions?

| Score | Meaning |
|-------|---------|
| 9–10  | Pixel-perfect consistency. Feels like a design system was followed rigorously. |
| 7–8   | Consistent with 1–2 minor deviations. |
| 5–6   | Mostly consistent but noticeable differences between screens or sections. |
| 3–4   | Inconsistencies are frequent and distracting. |
| 1–2   | Every component feels like it was designed independently. |

### 3. Typography (12%)

- Maximum 2–3 typeface families
- Mathematical type scale (ratios of 1.20×, 1.25×, 1.333×, or 1.5× between sizes)
- Body text 16px minimum
- Line height 1.4–1.6× for body text
- Line length 45–85 characters
- Font weight used purposefully — bold for emphasis, regular for body
- No arbitrary font size variation

**Red flags:** more than 3 fonts; body text below 14px; line length beyond 100 characters;
light/thin fonts at small sizes; justified text in UI.

### 4. Color System (12%)

- 5–7 core colors with clear purpose (primary, secondary, accent, neutrals, semantic)
- WCAG AA contrast: 4.5:1 normal text, 3:1 large text, 3:1 UI components
- Semantic color usage consistent (red = error, green = success, yellow = warning, blue = info)
- Color is never the *only* means of conveying information
- The palette has a rationale beyond "blue = trust" or "green = growth"

**Red flags:** more than 8 colors with no system; any text failing WCAG AA; color-only
status indicators; random accents with no relationship.

### 5. Spacing & Layout (12%)

- Consistent base unit (4px or 8px grid)
- Spacing between sections > spacing within sections (Gestalt proximity)
- Content fits a recognizable scan pattern (F, Z, center-weighted)
- No orphaned elements floating in space
- Alignment to clear axes — visual columns and rows
- Adequate breathing room; nothing cramped or swimming

**Red flags:** arbitrary spacing values (7px, 13px, 22px); cramped form fields; huge gaps
between elements that belong together; misaligned labels and inputs.

### 6. Interactive States (10%)

- All buttons have visible hover, active, focus, and disabled states
- All form inputs have default, focus, filled, error, and disabled states
- Links visually distinct from body text (color is not enough — consider underlines)
- Loading states for all async content
- Error states with helpful, specific messages
- Empty states designed (not blank screens)
- Transitions between states feel smooth and intentional

### 7. Accessibility (10%)

- All text meets WCAG AA contrast (4.5:1 normal, 3:1 large)
- All UI components meet 3:1 contrast against adjacent colors
- Focus states visible and distinct (not just browser defaults)
- Touch targets ≥ 44×44px on mobile
- Form fields have proper labels (not just placeholder text)
- No reliance on color alone for meaning
- Heading structure is semantic (h1 → h2 → h3, no skipped levels)
- Alt text on all meaningful images

### 8. Responsiveness (7%)

- Layout adapts gracefully across breakpoints (375, 768, 1024, 1440px)
- No horizontal scroll at any standard viewport
- Typography scales appropriately (doesn't stay desktop-sized on mobile)
- Touch targets adequate on mobile
- Navigation adapts to mobile (hamburger, bottom nav, or equivalent)
- Images and media scale properly

### 9. Polish & Craft (5%)

- Border radii, shadows, and gradients handled with care
- Micro-interactions feel purposeful, not decorative
- Loading and transition animations smooth (100–500ms, ease-out for entrances)
- No placeholder content, lorem ipsum, or default alt text in production
- Favicons, page titles, and meta tags are set
- The design has a point of view — it doesn't feel assembled from defaults

---

## Binary Pass/Fail Gates

Non-negotiable. ANY failure is a 🔴 Critical blocking issue. These are also the
accessibility gates in plan-runner's Tier-1 pass criteria.

- [ ] WCAG AA contrast on all text and UI components
- [ ] Visible focus states on all interactive elements
- [ ] Touch targets ≥ 44×44px on mobile
- [ ] No placeholder / lorem ipsum content in shipped work
- [ ] Error states on all form inputs
- [ ] Loading states for all async content
- [ ] No horizontal scroll at standard viewports
- [ ] Same component = same styling everywhere (zero exceptions)
- [ ] All meaningful images have alt text
- [ ] Form fields have labels (not just placeholders)
- [ ] No reliance on color alone to convey meaning

---

## AI-Generic Template Detector

Run this on every design. AI tools and template builders produce technically correct but
aesthetically bankrupt output by default. Detect and call it out.

### Red flags (count them — ≥ 3 means style intervention required)

1. **SaaS beige palette** — off-white background, one blue/purple CTA, gray body text, no personality
2. **Hero section formula** — headline left, image right, one CTA, social-proof logos below
3. **Inter (or system font) everywhere** — no typographic differentiation, no display font
4. **Gradient blobs** — random blurry gradient orbs in the background
5. **Feather/Heroicons at 24px** — outline icons dropped in without visual integration
6. **Card grids for everything** — features, testimonials, team, pricing, all identical cards
7. **Generic stock imagery** — diverse smiling people in offices, overhead laptop flat-lays
8. **No visual tension** — everything equally spaced, equally sized, equally weighted
9. **No art direction** — no clear decision about what *this* product should look like

### Signs of genuine craft (the opposite — these are what good looks like)

- A deliberate, unusual typographic choice
- A color story with a rationale beyond defaults
- At least one layout decision that breaks the expected grid
- Imagery chosen with a clear brief (not stock)
- Motion that feels considered, not default
- A visual moment you'd remember after closing the tab

### Acting on the result

If ≥ 3 red flags fire, the design needs a **style intervention** before scoring matters.
Don't grade it on a normal curve — surface the template signals first, then score.

Style intervention is 2–3 deliberate moves that give the design identity:
1. Pick a distinctive display font
2. Choose one unexpected color or a palette with personality
3. Break one expected layout pattern (asymmetry, overlap, dramatic whitespace)

See `aesthetics-and-motion.md` for the full intervention-points table.

---

## Priority-Ordered Evaluation Sequence

Even a partial review catches the biggest issues. Evaluate in this order:

1. Visual hierarchy — can the user instantly tell what matters?
2. Readability — can everything be read comfortably?
3. Consistency — does it feel like one product?
4. Spacing & alignment — is the grid respected?
5. Color system — is the palette purposeful and accessible?
6. Interactive states — do all elements respond?
7. Responsive behavior — does it work on mobile?
8. Accessibility — does it meet WCAG AA?
9. Typography system — is the type scale clean?
10. Animation & polish — does it feel finished?

---

## Scoring Formula

```
Overall = (Hierarchy × 0.15) + (Consistency × 0.15) + (Typography × 0.12) +
          (Color × 0.12) + (Spacing × 0.12) + (States × 0.10) +
          (Accessibility × 0.10) + (Responsive × 0.07) + (Polish × 0.05)
          - (1 if AI-Generic flags ≥ 3, else 0)
```

### Quality verdicts

- **Ship:** Overall ≥ 7.0 AND no dimension below 5 AND all binary gates pass
- **Iterate:** Overall 5.0–6.9 OR any dimension below 5
- **Rethink:** Overall below 5.0 — fundamental issues, not fixable with iteration

### Calibration

- 5/10 means average — acceptable but unremarkable. Most AI-generated UI lands here.
- 7/10 means genuinely good — clear intent, clean execution, minor issues only.
- 9/10 means exceptional — you'd show this as an example of great work.

Don't cluster scores at 7–8 to avoid making decisions. Spread your scores honestly.
