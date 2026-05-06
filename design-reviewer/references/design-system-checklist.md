# Design System Consistency Checklist

Use this to assess whether a design has a coherent system behind it, or whether it's
"designed by accident." Grounds the Consistency dimension of the rubric.

---

## Color System

- [ ] Defined primary brand color
- [ ] Defined secondary/accent color
- [ ] Neutral/gray scale (minimum 3 stops: light / mid / dark)
- [ ] Semantic colors: success (green), warning (amber), error (red), info (blue)
- [ ] Hover/active/disabled states defined for each interactive color
- [ ] Colors documented with HEX/RGB values — not "visually approximated" per component
- [ ] Palette not over-specified (>12 colors usually means inconsistent or noisy)

## Spacing System

- [ ] Spacing scale defined (e.g., 4px base: 4, 8, 12, 16, 24, 32, 48, 64, 96)
- [ ] Spacing applied consistently — no arbitrary values
- [ ] Internal component padding consistent (all cards: 16px; all buttons: 12×20)
- [ ] Section/block spacing larger than component spacing (creates visual rhythm)

## Typography System

- [ ] One defined type scale (not ad-hoc sizes per screen)
- [ ] H1–H6 (or equivalent) styles defined and applied consistently
- [ ] Font families limited to 1–2
- [ ] Body / label / caption styles clearly distinguished
- [ ] Font sizes consistent across screens

## Button System

- [ ] Defined primary, secondary, and tertiary button styles
- [ ] Defined danger/destructive button style
- [ ] All buttons of the same type look identical across screens
- [ ] All hover, active, focus, disabled states defined
- [ ] Button sizes consistent (e.g., small / medium / large with documented padding)

## Form Components

- [ ] All text inputs styled identically
- [ ] Checkboxes, radios, toggles consistent
- [ ] Dropdowns / selects consistent
- [ ] All inputs have visible, persistent labels
- [ ] Error states consistent across all inputs

## Card / Container Components

- [ ] Border radius consistent (no 4px on some, 12px on others without reason)
- [ ] Shadows consistent (same elevation = same shadow)
- [ ] Card padding consistent

## Iconography

- [ ] Single icon library (all Heroicons, all Phosphor, all Lucide)
- [ ] Icon sizes consistent within context (all inline 16px, all nav 24px)
- [ ] Icon weights consistent (no mixing filled / outlined / duotone without purpose)
- [ ] Interactive icons have accessible labels

## Responsive / Breakpoints

- [ ] Breakpoints defined and consistent (e.g., 375 / 768 / 1024 / 1280 / 1440)
- [ ] Layout degrades gracefully on mobile
- [ ] Touch targets ≥ 44×44px on mobile
- [ ] Font size readable on mobile without zooming
- [ ] Images scale without distortion or overflow

## Interaction Design

- [ ] Transitions consistent in easing and duration (typically 150–300ms)
- [ ] Loading states for async actions
- [ ] Empty states (first-time use, no results, no data)
- [ ] Error states for forms, network failures, etc.
- [ ] Confirmation dialogs for destructive actions

---

## Red Flags Indicating No Design System Exists

- Buttons differ in border-radius across screens
- Multiple shades of "blue" that don't match
- Font sizes like 13px, 15px, 17px, 22px (not on a scale)
- Spacing values of 7px, 11px, 18px (not on a grid)
- Some icons outlined, some filled, different stroke widths
- Colors that "look right" but differ by 10–15% across screens
- No definition of hover or focus states

When ≥ 3 of these are present, recommend establishing a design system or, at minimum, a
shared component library and token file before the product scales further.
