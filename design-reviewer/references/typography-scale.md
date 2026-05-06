# Typography Scale Reference

Patterns for type scales, line heights, weight usage, and pairing — used to ground the
Typography dimension of the rubric.

---

## Common Type Scales

### Minor Third (1.200 ratio) — compact UI

| Level | Size | Use |
|---|---|---|
| Display | 48px | Hero / marketing headline |
| H1 | 40px | Page title |
| H2 | 32px | Section heading |
| H3 | 24px | Card / sub-section heading |
| H4 | 20px | Component heading |
| Body Large | 18px | Lead paragraph, intro text |
| Body | 16px | Default body text |
| Body Small | 14px | Secondary text, labels |
| Caption | 12px | Meta, footnotes (sparingly) |

### Major Third (1.250 ratio) — more dramatic hierarchy

| Level | Size |
|---|---|
| Display | 63px |
| H1 | 49px |
| H2 | 39px |
| H3 | 31px |
| Body Large | 20px |
| Body | 16px |
| Caption | 13px |

### 8px Grid — practical/pragmatic

Sizes: 10, 12, 14, 16, 20, 24, 32, 40, 48, 64.
- Use 12px only for captions/legal with caution
- 16px baseline body
- Jump to 20/24 for subheadings
- 32+ for section headings, 48+ for display only

---

## Line Height Rules

| Text type | Line height |
|---|---|
| Display / large headlines | 1.1–1.2× |
| Headings | 1.2–1.3× |
| Subheadings | 1.3–1.4× |
| Body text | 1.5–1.6× |
| Captions | 1.4× |
| Code / monospace | 1.6–1.8× |

---

## Font Weight Usage

| Weight | Use |
|---|---|
| 900 / Black | Rarely — heavy display only |
| 700 / Bold | Headlines, CTAs, emphasis |
| 600 / Semibold | Subheadings, UI labels |
| 500 / Medium | Navigation, secondary headings |
| 400 / Regular | Body text |
| 300 / Light | Decorative only (large size, high contrast) |

**Rule:** never use weights below 400 for body or small text — fails contrast and readability.

---

## Font Pairing Patterns

### Pattern 1 — single family, weight contrast

- Headings: Inter 700–900
- Body: Inter 400
- Caption: Inter 400, muted color
- Safest choice for product UI

### Pattern 2 — serif headline + sans-serif body

- Headings: Playfair Display / Merriweather / Fraunces (700)
- Body: Inter / DM Sans / Nunito (400)
- Good for editorial, brand, marketing

### Pattern 3 — geometric + humanist

- Headings: Neue Haas Grotesk / Aktiv Grotesk / Circular (700)
- Body: Source Sans / IBM Plex Sans (400)
- Tech products, dashboards, SaaS

### Anti-patterns

- 3+ typefaces in one product
- Decorative script fonts for body or UI labels
- Two serif families mixed
- All-caps body text
- Italic body text (acceptable for 1–2 sentences, not paragraphs)

---

## Common Typography Mistakes to Flag

| Mistake | Why it's bad | Fix |
|---|---|---|
| No typographic hierarchy | Everything looks the same importance | Introduce 3-level scale (heading/body/caption) |
| Body text < 14px | Fails readability and accessibility | Minimum 16px for web body |
| Line height = 1.0 | Cramped and illegible | Set to 1.5× for body |
| Line length > 90 chars | Eye fatigue, hard to track | Max 65–80 chars; cap container width |
| Tight letter-spacing on body | Hurts readability | Default tracking; only adjust for ALL CAPS labels |
| Placeholder used as label | Disappears on focus, low contrast | Persistent labels above inputs |
| Mixed font styles in same role | Visual inconsistency | One style per hierarchy level, applied everywhere |
