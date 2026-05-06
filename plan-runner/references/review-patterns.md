# Review Patterns

Prompt templates and protocols for the review and dispatch phases of plan-runner.
Read this when setting up Phase 3 (review) and Phase 4 (targeted dispatch).

---

## Code Review Agent — Prompt Template

```
ROLE: You are a skeptical code reviewer. Your job is to find problems, not to confirm
quality. Favor false positives over false negatives — better to flag something fine than
to miss something broken.

REVIEW TARGET: [list of files / paths]

CONTEXT:
- Project: [name and one-sentence purpose]
- Tech stack: [languages, frameworks, runtime]
- Architecture decisions: [key constraints from the plan, e.g. "no client-side DB", "REST not GraphQL"]
- Conventions: [from CONVENTIONS.md if present]

EVALUATE AGAINST THESE CRITERIA:

1. SECURITY (any finding here = 🔴 Critical, blocks the pass gate)
   - Injection vectors: SQL, XSS, CSRF, command injection, prototype pollution
   - Exposed secrets: API keys, credentials, tokens hardcoded or logged
   - Unsafe deserialization, eval / new Function on user input
   - Missing authentication/authorization on user-facing endpoints
   - Missing input validation on data crossing trust boundaries

2. ARCHITECTURE (🔴 Critical if it violates the plan, 🟡 Important otherwise)
   - Layer violations (UI calling DB directly, business logic in views)
   - Tight coupling between modules that should be independent
   - Circular dependencies
   - God objects / functions doing many unrelated things
   - Pattern inconsistency (mixing async styles, mixing state management approaches)

3. PERFORMANCE (🔴 if severe, otherwise 🟡)
   - N+1 query patterns
   - Unbounded loops or recursion
   - Missing pagination on list endpoints
   - Large synchronous operations blocking the event loop
   - Uncached expensive computations that repeat per request

4. ERROR HANDLING (🔴 if missing entirely, 🟡 if partial)
   - Swallowed exceptions (catch blocks that do nothing)
   - User-facing errors that leak stack traces or internal details
   - Missing error states in UI components
   - No fallback for failed network requests
   - Unhandled promise rejections

5. CODE QUALITY (🟡 or 🟢 — flag for improvement, doesn't block)
   - Functions longer than ~40 lines
   - Unclear naming
   - Duplicated logic (DRY violations)
   - Misleading or stale comments
   - Dead code, unused imports

OUTPUT FORMAT:
For each issue:
- Severity: 🔴 Critical | 🟡 Important | 🟢 Polish
- File: [path:line-range]
- Issue: [clear description]
- Impact: [what goes wrong if unfixed]
- Fix: [concrete suggestion]

End with a summary: total issues by severity, plus a verdict — PASS / PASS WITH NOTES / FAIL.
```

---

## Visual Review — Delegation Template

Plan Runner does not run its own visual review. It hands off to the `design-reviewer`
skill. Use this template to invoke it.

```
DELEGATE TO: design-reviewer skill

MODE: audit  (review existing UI; alternative: plan-create or plan-improve)

TARGET:
- Pages/components: [list]
- Screenshots: [paths to captured screenshots at 375px, 768px, 1440px]
- Live URL (optional): [url]

CONTEXT:
- Project type: [web app / marketing site / dashboard / etc.]
- Target audience: [who]
- Design tokens: [paste or link]
- Reference design: [Figma link / description, if any]

REVIEW DEPTH: full  (alternatives: micro-only, macro-only, feel-only)

OUTPUT EXPECTED:
- Scored audit per the 9-dimension visual rubric
- Binary pass/fail gate results
- AI-Generic Template Detector flag count
- Issues prioritized 🔴 / 🟡 / 🟢
- Quick wins list
- Bigger opportunities list
```

---

## Critic-Defender-Judge Protocol

Use this when a review flags something but it's not obvious whether the finding is real
or over-zealous.

### When to invoke

- The finding conflicts with a deliberate plan-level decision.
- The fix would require significant rework.
- The same issue has appeared in 2+ review rounds without resolution.
- There's a genuine trade-off (performance vs readability, accessibility vs aesthetics).

### Step 1 — Critic

```
You are the Critic. Argue forcefully why [specific finding] is a real problem that must
be fixed. Cover:
- Worst-case impact if unfixed
- Edge cases that make this dangerous
- Standards or best practices violated
- Whether a senior engineer / designer would flag this in a real review

Make the strongest case possible.
```

### Step 2 — Defender

```
You are the Defender. Argue why [specific finding] is acceptable as-is, or why the current
approach is the right trade-off. Cover:
- Constraints that make alternatives impractical
- Costs of fixing this (time, complexity, regressions elsewhere)
- Whether this is an intentional decision with a valid rationale
- Whether a pragmatic senior engineer / designer would ship this

Be honest. If the defense is weak, say so.
```

### Step 3 — Judge

```
You've heard both sides. Decide:
- Which side has the stronger case?
- Verdict: FIX (do it now) | ACCEPT (current approach stands) | DEFER (log for next iteration)
- One-sentence rationale.
```

Do not skip the Judge step — without it, you keep both arguments unresolved and the loop
continues.

---

## Iteration Tracking Template

Log every review round. Visibility prevents thrashing and helps the user see progress.

```markdown
## Review Log: [Phase Name]

### Round 1
- Reviewers run: [code / visual / a11y / tests]
- Tier 1 status: [PASS / FAIL — list failures]
- Tier 2 score: [overall design score]
- Issues found: [X] 🔴 / [X] 🟡 / [X] 🟢
- Key findings: [3–5 bullets]
- Action: [DISPATCH FIXERS / DISPATCH IMPROVERS / DISPATCH BUILDERS / PASS / ESCALATE]

### Round 2
- Changes applied: [list of files modified, what each fixer/improver did]
- Tier 1 status: [PASS / FAIL]
- Tier 2 score: [overall] (Δ from Round 1: +X%)
- Remaining issues: [X] 🔴 / [X] 🟡 / [X] 🟢
- Action: [DISPATCH / PASS / ESCALATE]

### Outcome
- Final Tier 1: [PASS / FAIL with [list of accepted limitations]]
- Final Tier 2 score: [X]
- Iterations used: [X] / 4 max
- Exit reason: [pass gate met / diminishing returns / cap reached / user decision]
- Deferred items: [list 🟡/🟢 issues accepted for later — these need to surface in the ship summary]
```

---

## Self-Mixture-of-Agents (Self-MoA)

For high-stakes components, generate variants and synthesize. Reliably produces ~6–7%
quality improvement over a single-pass output, per published research.

### When to use

- Hero sections, key landing-page layouts
- Complex interactive components (data tables, dashboards, forms)
- Architecture decisions with multiple valid approaches
- Any improver dispatch where a single improver couldn't lift the score above the threshold

### Protocol

1. Generate 2–3 variants of the same deliverable, each with a different bias:
   - Variant A: prioritize visual impact and boldness
   - Variant B: prioritize clarity and usability
   - Variant C: prioritize technical elegance / performance

2. Review all variants against the relevant rubric (visual-rubric.md, code criteria, etc.).

3. Synthesize: take the strongest elements from each variant into a final version.
   - Layout from the variant that scored highest on Visual Hierarchy
   - Interaction patterns from the variant that scored highest on Usability/States
   - Code structure from the cleanest implementation
   - Type/color choices from the most intentional Aesthetic variant

4. The synthesis itself gets ONE review round. If it doesn't beat the best single variant,
   ship the best single variant — don't synthesize for synthesis's sake.

### Don't use Self-MoA for

- Boilerplate (e.g., a CRUD endpoint following an established pattern) — overkill
- Anything where the rubric clearly shows a single right answer
- Time-pressured fixer dispatch (Self-MoA is slow)
