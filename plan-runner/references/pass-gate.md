# Pass Gate Specification

The two-tier pass gate that gates Phase 6 (Ship). Read this when checking whether a build
is shippable. The gate is **binary** — either it passes or it doesn't. There are no
"mostly passes."

---

## Tier 1 — Table-Stakes (Non-Negotiable)

Every item must be true. If any is false, the build fails Tier 1 and you loop back to
Phase 4 (targeted fixer dispatch). The items below are not negotiable — the user does
not get to skip them.

### Build & Runtime
- [ ] Build compiles / app starts without errors
- [ ] No unhandled exceptions in the smoke test path
- [ ] All test scenarios defined in Phase 3c pass (unit, integration, end-to-end)
- [ ] No 🔴 Critical findings remaining in code review (security, data loss, crashes)

### Accessibility (binary gates from visual-rubric.md)
- [ ] WCAG AA contrast ratios on all text (4.5:1 normal, 3:1 large) and UI components (3:1)
- [ ] Visible focus states on all interactive elements
- [ ] Touch targets ≥ 44×44px on mobile viewports
- [ ] Form fields have labels (not just placeholders)
- [ ] All meaningful images have alt text
- [ ] No reliance on color alone to convey meaning

### Honesty
- [ ] No placeholder / lorem ipsum / "TODO" content shipping in user-facing surfaces
- [ ] Loading states exist for all async content
- [ ] Error states exist for all forms and async operations
- [ ] Empty states exist for any list/grid that can be empty

If a Tier-1 item is not verifiable in your environment (e.g., you can't run tests in plain
Claude.ai), say so explicitly to the user and ask them to verify it before declaring the
build passed. **Do not invent verification.**

---

## Tier 2 — Quality (Non-Negotiable)

Tier 1 is "the build is functional and honest." Tier 2 is "the build is good." Both must
pass before the build ships.

### Visual quality (per visual-rubric.md)
- [ ] Overall weighted design score ≥ 7.0/10
- [ ] No individual dimension scoring below 5/10
- [ ] AI-Generic Template Detector: ≤ 2 of 9 red flags

### Coherence
- [ ] All review findings have been addressed, deferred (logged), or accepted (logged)
- [ ] The build matches what the plan promised — every deliverable from the plan is present
      and acceptance criteria are met

If Tier 2 fails: loop back to Phase 4 with the weak dimensions as **improver** briefs,
not fixer briefs. Tier-2 work elevates quality; it doesn't fix bugs.

---

## Iteration Cap

**Hard cap: 4 review rounds per phase.** Do not loop past this silently.

If both tiers haven't passed after 4 rounds, stop and escalate to the user with this
exact structure:

```
I've run 4 review iterations on [phase name].

Current state:
  Tier 1: [PASS / FAIL] — [if FAIL: list specific failures]
  Tier 2: [overall score] — [list dimensions below threshold]

Trajectory:
  Round 1 → 2: [score change]
  Round 2 → 3: [score change]
  Round 3 → 4: [score change]

Pattern detected:
  [e.g. "The contrast issue on primary buttons has appeared in Rounds 2, 3, 4. The fix
  attempts have all reverted to the same low-contrast state, which suggests the design
  token itself is wrong, not the application."]

This is usually a structural problem rather than an implementation bug. Options:

  A) Rethink: change the underlying decision causing the regression
     [specific suggestion]
  B) Accept: ship as-is with this as a known limitation
     [what gets logged]
  C) Defer: split the failing piece off as a follow-up; ship the rest
     [what gets shipped now, what's deferred]

Your call.
```

---

## Other Exit Conditions (Besides Cap)

These are early exits, not failures. The build can still pass if Tier 1 + Tier 2 are met
when the exit triggers.

### Diminishing Returns

If the design score improves less than 5% between two consecutive rounds, exit with the
current best — but **only if Tier 1 still passes**. If Tier 1 is failing, you can't exit
on diminishing returns; you have to keep fixing.

```
Round 2: 6.4 → Round 3: 7.1  (11% gain — keep going)
Round 3: 7.1 → Round 4: 7.2  (1.4% gain — exit, current state is the best you'll get)
```

Log the exit reason and any unaddressed issues in the ship summary.

### Regression

If a fix introduces a NEW Tier-1 failure (something that was passing now isn't), stop
the loop immediately and escalate. A regression means the fix logic is wrong, not just
incomplete. Do not auto-retry — show the user what regressed and ask how to proceed.

### Scope expansion

If during review the user asks for new features (not in the original plan), do not absorb
them into the current loop. Log them as deferred and finish the loop on the original plan.
Treat scope expansion as a new plan to be run separately.

---

## After the Pass Gate Fires

Document what passed:

```markdown
## Pass Gate Result

Tier 1 (Table-stakes): [PASS / FAIL]
  Build & runtime: [✓ / ✗ details]
  Accessibility: [✓ / ✗ details]
  Honesty: [✓ / ✗ details]

Tier 2 (Quality): [PASS / FAIL]
  Overall design score: [X]/10
  Lowest dimension: [name] at [score]
  AI-Generic flags: [N]/9

Iterations used: [N] / 4

Deferred items (carry into ship summary):
  🟡 [item — why deferred]
  🟢 [item — why deferred]
```

Hand this to Phase 6 (Ship). The deferred items must surface in the user-facing summary
so they're not silently dropped.
