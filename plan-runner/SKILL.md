---
name: plan-runner
description: >
  Takes an existing plan and runs it: decomposes the plan into an execution DAG, dispatches
  builder agents in parallel, reviews their output with real tools (code analysis, tests,
  screenshots), then dispatches targeted fixer/improver/builder agents to elevate the build,
  and loops until a two-tier pass gate is met. Trigger this skill when the user already has
  a plan and says "run this plan," "execute this," "build from this plan," "implement this
  spec," or hands over any structured plan/PRD/brief that needs to be turned into shipped
  work. Do NOT trigger when the user wants to CREATE a plan — that's a separate task. This
  skill ingests a plan and runs the full build → review → fix → ship loop.
---

# Plan Runner

You take an existing plan and run it to completion. Your job is not to invent the plan —
the user already has one. Your job is to convert it into a parallel execution DAG, dispatch
builder agents, review their output with real tools, dispatch targeted fixers, and gate the
work behind a two-tier pass criterion. You do not ship work you have not verified.

This skill defines the full lifecycle: **ingest → decompose → dispatch → review → fix →
gate → ship**.

---

## Environment Detection

Different environments have different tool surfaces. Before doing anything, decide which
mode you're in:

- **Claude Code mode** — bash, file tools, native sub-agent dispatch (the `Task` tool with
  multiple subagent types), web access via Chrome MCP if installed. Tests run via `npm test`,
  `pytest`, etc. Screenshots via Playwright/Puppeteer headless or Chrome MCP screenshots.
  This is the high-leverage environment — use parallel sub-agents whenever possible.

- **Cowork mode** — file tools, computer-use (real screenshots of the desktop), Chrome MCP
  (DOM-aware browser control), bash sandbox for code execution. No native sub-agent dispatch —
  execute tasks sequentially or via the Task tool. Real screenshots of running apps are easier
  here than in Claude Code.

- **Plain Claude.ai mode** — file tools and conversation only. No bash, no sub-agents, no
  screenshots. Execute everything sequentially. Reviews become "self-review with critic
  persona." Real-tool checks are unavailable; flag this to the user — quality bar drops
  because verification is weaker.

Where this skill says "dispatch a sub-agent" or "run tests," substitute the equivalent for
your environment. If a step requires a tool you don't have, say so — don't fake the check.

---

## Phase 0 — Ingest the Plan

The user is handing you a plan. Read it before you do anything else.

A real plan has: a goal, a list of deliverables, constraints (tech stack, deadline, scope
boundaries), and a definition of done. If the plan is missing any of these, **stop and
ask** — don't guess. Wrong assumptions about scope or done-criteria cascade through every
downstream phase.

Validate the plan against this checklist:

1. **Is the goal explicit?** What does success look like in one sentence?
2. **Are the deliverables enumerable?** You should be able to list them.
3. **Are constraints stated?** Stack, environment, must-haves, no-gos.
4. **Is "done" defined?** What test or check confirms the work is shippable?
5. **Are there gaps or contradictions?** Surface them now, not at review time.

If gaps exist, ask the user **focused** questions and incorporate the answers into a
revised version of the plan that you'll work from. Do not proceed with an incomplete plan.

---

## Phase 1 — Convert the Plan into a Task DAG

A plan is a description. A DAG is an execution structure. Convert one into the other.

### Decomposition

Break each deliverable into tasks. Group tasks into **layers** based on dependencies:

```
LAYER 1 (sequential): Foundation — architecture choices, data model, design tokens, API contract
LAYER 2 (parallel):   Build — backend | frontend | data | docs (each parallelizable)
LAYER 3 (sequential): Integration — wire it together end-to-end
LAYER 4 (parallel):   Review — code review | visual QA | tests | accessibility
LAYER 5 (sequential): Fix — targeted dispatch based on review findings (this layer loops)
LAYER 6 (sequential): Ship — final polish, summary, handoff
```

For each task, specify:

- **Output:** the file, component, endpoint, or artifact this task produces
- **Inputs:** which upstream tasks must complete first (explicit dependencies)
- **Acceptance criteria:** the specific, checkable conditions for "done"
- **Complexity:** S/M/L — drives whether it gets its own sub-agent

### Principles

- **Maximize parallelism.** Independent tasks should never wait. If two tasks have no
  dependency between them, dispatch them in the same layer.
- **Isolate context.** Each builder agent gets only what it needs — task brief, relevant
  architecture decisions, the specific files it will touch. Not the whole project.
- **Cap task scope.** A single sub-agent should produce 5–6 files maximum. If a task feels
  multi-headed, split it before dispatch.
- **2–5 sub-agents per build layer** is the sweet spot. More than that and synthesis
  becomes the bottleneck.

### Output of this phase

A written DAG the user can read and approve. Format like:

```
## DAG: [Plan Name]

### Layer 1 — Foundation (sequential)
  T1: Define data model and API contract
      → produces: schema.ts, api-types.ts
      → criteria: types compile, contract covers all CRUD operations from the plan

  T2: Establish design tokens
      → produces: tokens.css
      → criteria: covers spacing scale, color palette, type scale, breakpoints

### Layer 2 — Build (parallel, after Layer 1)
  T3: Backend endpoints  [needs: T1]  → /api routes, integration tests
  T4: Frontend pages     [needs: T1, T2]  → /app pages, component tests
  T5: Database setup     [needs: T1]  → migrations, seed data

### Layer 3 — Integration (sequential, after Layer 2)
  T6: Wire frontend to backend, end-to-end smoke test  [needs: T3, T4, T5]

### Layer 4 — Review (parallel, after Layer 3)
  T7: Code review
  T8: Visual/design review (delegates to design-reviewer skill)
  T9: Test execution + scenario coverage
  T10: Accessibility audit

### Layer 5 — Fix (sequential, loops until pass gate met)
  T11: Address review findings via targeted agent dispatch (see Phase 4)

### Layer 6 — Ship (sequential)
  T12: Final assembly, summary, deliver
```

Get user approval on the DAG before executing. Adjust if they want to reorder, add, or
remove layers. If they say "just do it," still produce the DAG internally — you need it
to dispatch correctly — but skip the approval step.

---

## Phase 2 — Dispatch Builder Agents

For each parallelizable layer, dispatch one sub-agent per task. Each gets a focused brief.

### Builder agent brief template

```
You are a builder agent for [Task Name] inside [Plan Name].

CONTEXT (only what you need):
- Architecture decisions: [relevant excerpts]
- Schemas/contracts: [relevant excerpts]
- Design tokens: [relevant excerpts]
- Files you may modify: [list]

YOUR TASK:
[Specific deliverable, copied from the DAG entry]

ACCEPTANCE CRITERIA (your work is not done until all are met):
[Bullet list, copied from the DAG entry]

CONSTRAINTS:
[Tech stack, conventions, anti-patterns to avoid]

OUTPUT:
Save your work to [explicit paths]. When done, report:
1. What you built (list of files created/modified)
2. Decisions you made that downstream tasks should know about
3. Anything you punted on or flagged for follow-up
```

### Dispatch rules

- **Never send the full project context.** Send only what this task needs.
- **Set explicit, non-colliding output paths.** Two agents writing to the same file is a bug.
- **Each agent succeeds without knowing about the others.** If two agents need to coordinate,
  that's a Layer-1 architecture decision you missed — go back to Phase 1.
- **After all agents in a layer return,** synthesize their outputs and resolve conflicts
  before moving to the next layer.

### When sub-agents aren't available (Cowork / Claude.ai)

Execute tasks sequentially within each layer, in dependency order. The DAG structure still
prevents you from doing integration work before the pieces exist. You lose parallelism but
keep the structure.

---

## Phase 3 — Review with Real Tools

This is the part most orchestrators skip. You don't ship work you haven't verified, and
verification means using actual tools — not just reading the code and saying "looks good."

Run these reviews in parallel where the environment supports it.

### 3a. Code review

Dispatch a code-review agent (or run sequentially) with the prompt template in
`references/review-patterns.md` (see "Code Review Agent"). The agent flags issues by
severity: 🔴 Critical (blocks), 🟡 Important (fix before ship), 🟢 Polish (defer).

Do not invent the criteria — use the template. It covers security, architecture, performance,
error handling, and code quality.

### 3b. Static analysis & tests (concrete tool invocation)

For each language/stack, run the appropriate checks. Don't skip these — they catch what
human review misses.

| Stack | Lint | Type-check | Tests |
|---|---|---|---|
| TypeScript / JS | `eslint .` or `biome check` | `tsc --noEmit` | `npm test` / `vitest` / `jest` |
| Python | `ruff check .` | `mypy .` | `pytest` |
| Go | `go vet ./...` | (built-in) | `go test ./...` |
| Rust | `cargo clippy` | (built-in) | `cargo test` |

If a stack you don't recognize, ask the user what their lint/test commands are. If the
project has no tests, **flag this** as a Tier-1 risk before proceeding to the pass gate.

In **Claude Code**: run via bash. In **Cowork**: run via the bash sandbox or ask the user
to run them locally and paste output. In **plain Claude.ai**: not available — flag the gap.

### 3c. Scenario / behavior testing

Beyond unit tests, define and run user-flow scenarios:

```
Scenario: User signs up
  GIVEN the homepage is loaded
  WHEN the user clicks "Sign up" and submits a valid email
  THEN they receive a confirmation email AND see the dashboard
```

Write 3–8 scenarios per major feature. In Claude Code, automate with Playwright. In Cowork,
walk through them via Chrome MCP or computer-use, capturing screenshots at each step. In
plain Claude.ai, describe the expected behavior and have the user verify manually.

### 3d. Visual review (delegate to design-reviewer skill)

For any UI work, do not improvise the visual review. Delegate to the `design-reviewer` skill
in this repo. Pass it:

- The list of pages/components to review
- Screenshots at 375px, 768px, and 1440px viewports (capture them first)
- The design tokens or reference design, if any
- Whether it's an audit (review existing) or a plan (design new / improve existing)

The design-reviewer returns a scored audit using the rubric in
`../design-reviewer/references/visual-rubric.md`. This is the input to the Tier-2 pass gate.

### 3e. Accessibility audit

Mechanical: run an automated checker (axe-core, Lighthouse a11y, or Chrome MCP's
`run_accessibility_audit` if available). Holistic: have the design-reviewer skill score the
Accessibility dimension of its rubric. Both are required.

### Review output

Each review produces a report with issues categorized by severity. Aggregate them into one
review log per round. Use the template in `references/review-patterns.md` (see "Iteration
Tracking Template").

---

## Phase 4 — Targeted Dispatch: Fixer, Improver, Builder

When review surfaces issues, do **not** send the work back to the original builder agent
with "fix this." That's how you get ping-pong. Instead, classify each finding and dispatch
the right kind of agent for it.

### Three agent roles

- **Fixer** — addresses a specific bug, regression, or rubric failure. Brief is narrow:
  "Here's the issue, here's the file, here's what good looks like. Fix it."
- **Improver** — takes work that passes correctness but scores low on quality (e.g., design
  score 6/10). Brief is "Here's the existing implementation, here's the rubric, here are the
  weak dimensions. Elevate it."
- **Builder** — review revealed something missing entirely (no error states, no loading
  states, no empty states, no test coverage for X). Brief is the same as Phase 2 — produce
  this new deliverable.

### Dispatch rules

- **One issue, one agent.** Don't bundle "fix the contrast bug AND redesign the hero" — those
  are different jobs.
- **Group by file.** If three issues all live in one file, one agent fixes all three; this
  prevents merge conflicts.
- **Improvers get the rubric.** Send the relevant dimension and threshold so the agent knows
  what "better" means concretely.
- **Builders get acceptance criteria.** Same as Phase 2 — without criteria, the work drifts.

### After the dispatch round

Re-run the relevant reviews from Phase 3. Compare scores round-over-round.

---

## Phase 5 — The Pass Gate

This is binary. The build passes or it doesn't.

### Tier 1 — Table-stakes (non-negotiable, all must be true)

- [ ] Build compiles / app starts without errors
- [ ] All defined test scenarios pass (unit, integration, end-to-end)
- [ ] No 🔴 Critical findings in code review (security, data loss, crashes)
- [ ] No runtime errors in the smoke test
- [ ] All accessibility binary gates pass (see visual-rubric.md)

If any Tier-1 item fails: **the build does not pass.** Loop back to Phase 4 with the failing
items as fixer briefs.

### Tier 2 — Quality (all must be true)

- [ ] Design score ≥ 7.0/10 overall (per visual rubric)
- [ ] No design dimension scoring below 5/10
- [ ] AI-Generic Template Detector flags ≤ 2 of 9 (see visual-rubric.md)

If Tier 2 fails: loop back to Phase 4 with the weak dimensions as improver briefs.

### Iteration cap

Hard cap: **4 review rounds per phase**. If both tiers haven't passed after 4 rounds,
**stop and escalate to the user**:

> "I've run 4 review iterations on [phase]. The build is at: [Tier 1 status, Tier 2 score].
> Remaining issues: [list]. The same issue has appeared in [N] consecutive rounds, which
> usually means a structural problem rather than an implementation bug. Options:
> A) Rethink the approach for [issue], B) Accept as a known limitation, C) Your call."

Do not loop past the cap silently. The cap exists because some problems can't be fixed by
iteration — they need a human decision.

### Other exit conditions (besides cap)

- **Diminishing returns:** if the design score improves <5% between two consecutive rounds,
  exit with the current best (record what was deferred).
- **Regression:** if a fix introduces a new Tier-1 failure, stop the loop and escalate. That
  means the fix logic is wrong, not just incomplete.

---

## Phase 6 — Ship

After both tiers pass:

1. **Integration check** — does everything work together end-to-end at the smoke-test level?
2. **Cross-cutting concerns** — error states, loading states, empty states, edge cases all
   covered (the design-reviewer rubric checks this; verify it scored ≥ 7).
3. **Responsive check** — viewports 375 / 768 / 1024 / 1440 all clean.
4. **Final visual pass** — one last screenshot review of the assembled product.
5. **Deliverable summary** — present to the user with: what was built, decisions made, known
   limitations, and any items deferred from review.

If you escalated mid-loop and the user accepted limitations, log them in the summary so
they're not lost.

---

## Adaptive Behavior

Not every plan needs the full ceremony. Scale the process:

- **Small plan (1–3 tasks):** mental decomposition, sequential build, single review pass
  per dimension. Still run the pass gate.
- **Medium plan (4–10 tasks):** full DAG, 2–3 parallel agents per build layer, standard
  review cycle.
- **Large plan (10+ tasks):** full DAG with multiple build layers, 4–5 agents per layer,
  formal iteration tracking.

If the plan is genuinely small (say, "fix this one bug"), this skill is overkill. Tell the
user it's overkill and just fix the bug.

### Self-Mixture-of-Agents (for elevated quality)

For critical components — hero sections, key interactions, complex algorithms — generate
2–3 variants from different angles, then synthesize the best parts of each. Use this in
Layer 2 (build) for items the user flagged as high-stakes, or in Phase 4 (improver dispatch)
when a single improver can't lift the score.

See `references/review-patterns.md` for the Self-MoA protocol.

---

## Reference Files

- `references/review-patterns.md` — code review agent prompt, visual review delegation
  template, critic-defender-judge protocol, iteration tracking template, Self-MoA protocol.
  **Read this when setting up reviews.**

- `references/pass-gate.md` — full Tier-1 and Tier-2 specification, escalation script,
  iteration cap mechanics. **Read this when checking whether a build passes.**

- `../design-reviewer/SKILL.md` — the design-reviewer skill. Plan Runner delegates all
  visual review to this skill in Phase 3d. Do not duplicate its rubric here.

- `../design-reviewer/references/visual-rubric.md` — the 9-dimension visual scoring rubric,
  binary accessibility gates, AI-Generic Template Detector. Plan Runner's Tier-2 pass gate
  is defined against this rubric.
