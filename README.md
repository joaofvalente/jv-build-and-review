# jv-build-and-review

Two skills for Claude that work together as a build-and-review system: one runs an
existing plan to completion, the other reviews UI/UX work or proposes design plans.

## What's here

```
jv-build-and-review/
├── plan-runner/         — takes a plan, runs it, reviews, fixes, ships
│   ├── SKILL.md
│   └── references/
│       ├── review-patterns.md   — code review prompts, dispatch templates, MoA protocol
│       └── pass-gate.md         — two-tier pass gate spec
└── design-reviewer/     — audits UI/UX or produces design plans
    ├── SKILL.md
    └── references/
        ├── visual-rubric.md           — 9-dimension scoring + binary gates
        ├── accessibility-standards.md — WCAG AA reference
        ├── typography-scale.md        — type scales, line height, weight, pairing
        ├── design-system-checklist.md — consistency checklist
        └── aesthetics-and-motion.md   — feel pass + motion + AI-generic detector
```

## When to use which

**plan-runner** — you already have a plan, brief, or PRD and you want it built. The skill
decomposes the plan into a parallel execution DAG, dispatches builder agents, runs reviews
with real tools (linters, tests, screenshots), dispatches targeted fixers/improvers, and
gates the work behind a two-tier pass gate (Tier 1: build compiles + tests pass + a11y;
Tier 2: design score ≥ 7/10). Loops until both tiers pass or the iteration cap fires.

Trigger phrases: "run this plan," "build from this spec," "execute this PRD," "implement
this brief."

Don't trigger for: "make me a plan." That's a different task — plan-runner ingests plans;
it doesn't write them.

**design-reviewer** — you have a UI to review, OR you want a design plan (new design or
improvement plan). Reviews at four zoom levels (pixel → component → composition → flow/IA)
plus a separate Feel pass for aesthetic/brand/motion. Outputs scored audits or structured
plans depending on the mode.

Trigger phrases: "review this design," "audit this UI," "is this accessible?", "what's
wrong with this screen?", "design a hero for X," "make a plan to improve this."

## How they work together

plan-runner does NOT do its own visual review. In Phase 3 of its lifecycle, it delegates
to design-reviewer — passing screenshots, context, and the requested mode (audit, plan-create,
or plan-improve). Design-reviewer returns a scored audit using the rubric in
`design-reviewer/references/visual-rubric.md`. Plan-runner's Tier-2 pass gate is defined
against that rubric.

This is the intentional split: plan-runner owns build orchestration; design-reviewer owns
visual judgment. Don't duplicate their concerns.

## Installation

These are Claude skills — folders containing a SKILL.md with frontmatter and an optional
`references/` directory. Where to drop them depends on the Claude product you're using.

- **Claude Code:** copy each skill folder into `.claude/skills/` at your project root, or
  into `~/.claude/skills/` for global availability across all projects.
- **Cowork:** copy into the user-skills location for your Cowork environment (typically
  `~/Library/Application Support/Claude/skills/` on macOS — check the Cowork docs for the
  current path on your version).
- **Plain Claude.ai:** skills aren't natively installable in claude.ai chat. You can paste
  the contents of a SKILL.md into a project's custom instructions, but you lose the
  reference-file structure and the trigger-on-description behavior. Best used in Claude Code
  or Cowork.

## Customization notes

The skills have been adapted from upstream defaults to fit a specific build/review model:

- **plan-runner** assumes you're handing it an existing plan, not asking it to plan from
  scratch. Phase 0 validates the plan; it does not write one.
- **plan-runner** specifies a two-tier pass gate (Tier 1 = table-stakes: compiles, tests
  pass, a11y. Tier 2 = quality: design score ≥ 7/10, no dimension below 5). Edit
  `plan-runner/references/pass-gate.md` if you want to change the bar.
- **plan-runner** distinguishes three dispatch roles in Phase 4: fixer (bug), improver
  (low-quality but correct), builder (something missing). Don't collapse them — the brief
  is different per role.
- **design-reviewer** has three modes (audit, plan-create, plan-improve). If the user's
  request is ambiguous, the skill is instructed to ask which mode they want.
- Both skills detect their environment (Claude Code, Cowork, plain Claude.ai) and adapt
  what tools they assume. If a tool isn't available, they flag the gap rather than faking
  the check.

## Editing the rubric

`design-reviewer/references/visual-rubric.md` is the canonical scoring source. Plan-runner
references it. If you change weights, dimensions, or thresholds there, plan-runner's pass
gate inherits the change automatically.

If you want to keep the current rubric stable but experiment with a stricter or looser bar
just for plan-runner's pass gate, edit `plan-runner/references/pass-gate.md` instead — it
references rubric output but applies its own thresholds on top.
