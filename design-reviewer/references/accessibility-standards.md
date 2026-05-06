# Accessibility Standards (WCAG 2.2 AA)

Baseline reference for the Accessibility dimension of the rubric. Every binary gate in
visual-rubric.md is grounded here.

---

## Contrast Ratios

| Text type | Minimum (AA) | Minimum (AAA) |
|---|---|---|
| Normal text (<18px regular / <14px bold) | 4.5:1 | 7:1 |
| Large text (≥18px regular / ≥14px bold) | 3:1 | 4.5:1 |
| UI components, icons, focus indicators | 3:1 | n/a |
| Decorative / incidental images | no requirement | — |

Tool: https://webaim.org/resources/contrastchecker/

---

## Typography Accessibility

- Minimum body font size: **16px** for web (14px acceptable for dense data UI with caveats)
- Line height: **1.4–1.6× font-size** for body text
- Max line length: **60–80 characters** per line
- Avoid all-caps for body text
- Avoid justified alignment (creates "rivers" that harm readability, especially for dyslexia)
- Avoid light fonts (weight < 300) for body text
- Avoid pure white (#FFFFFF) on pure black (#000000) — try off-white (#F5F5F5) on
  near-black (#1A1A1A) to reduce glare

---

## Touch & Click Targets

- Mobile: minimum **44×44px** (Apple HIG) or **48×48dp** (Google Material)
- Desktop: minimum **32×32px** for icon buttons; 24px height absolute floor
- Spacing between adjacent targets: minimum **8px**

---

## Focus & Keyboard Navigation

- All interactive elements reachable via keyboard Tab key
- Focus indicator must be **visible** — WCAG 2.4.11 requires minimum 3:1 contrast between
  the focus indicator and surrounding pixels
- Focus order must be **logical** — matches visual reading order
- No focus traps except in modals (and modals must have an escape mechanism)
- Skip-navigation links recommended for long pages

---

## Color & Meaning

- **Never** use color as the **only** means to convey information
  - ❌ Red border = error (no icon, no label)
  - ✅ Red border + error icon + "This field is required" text
- Color blindness affects ~8% of males. Test with Deuteranopia (red-green) simulation.
- Tools: Stark (Figma plugin), Coblis, Color Oracle.

---

## Form Accessibility

- Every input needs a **visible, persistent label** (not just placeholder text — placeholders
  disappear on focus and have insufficient contrast)
- Error messages must be specific and appear near the field
- Required fields must be indicated with more than just an asterisk (or the asterisk's
  meaning must be explained)
- Group related inputs with `<fieldset>` / `<legend>`

---

## Images & Icons

- All meaningful images need **alt text** that describes the meaning, not the literal image
- Decorative images: empty `alt=""` to skip in screen readers
- Icon buttons need accessible labels (aria-label or visually hidden text)
- Avoid conveying information through images without a text equivalent

---

## Motion & Animation

- Respect `prefers-reduced-motion` — substitute cross-fades for transforms when reduce is set
- Avoid flashing content (>3 flashes per second can trigger seizures)
- Animations that serve UX purposes should be pausable

---

## POUR (WCAG Foundation)

- **Perceivable** — info available to the senses (sight, sound, touch alternatives)
- **Operable** — all functions usable via keyboard or assistive tech
- **Understandable** — content and UI predictable and readable
- **Robust** — works with current and future assistive technologies
