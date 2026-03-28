# 🌊 groundswell

> *A groundswell forms deep in the ocean — far from the shore, far from the noise. By the time it arrives, it carries enormous energy with perfect form. The surfer who reads it early, waits patiently, and paddles with purpose is the one who rides it all the way in.*

**groundswell** is a Claude skill that helps developers think deeply before executing. Like a surfer reading the ocean before choosing a wave — it guides you through understanding the conditions, aligning on the right goal, removing ambiguity, and committing to action only when truly ready.

---

## Why groundswell?

Most engineering problems aren't hard because of the code. They're hard because the goal was fuzzy, the constraints were unknown, or the approach was chosen too quickly.

groundswell slows you down *just enough* to speed you up. It asks the questions you didn't know you needed to answer, surfaces hidden constraints, accumulates insights across every round of Q&A, and only lets you execute when the wave is actually worth riding.

---

## How it works

```
Phase 0: Read the Conditions   — tech context & task classification  [single pass]
Phase 1: Choose Your Wave      — purpose alignment                   [score ≥ 9]
Phase 2: Paddle Into Position  — ambiguity removal                   [score ≥ 9]
Phase 3: Drop In               — synthesis, confirm, decide          [A/B/C/D]
```

### Phase 0 — Read the Conditions
Identifies the task type (feature development, architecture design, or performance improvement) and captures the full tech context: language, frameworks, infra, test coverage, constraints, team size.

### Phase 1 — Choose Your Wave
3 targeted questions per round. Scores goal clarity, success criteria clarity, and constraint clarity on a 0–10 scale. **Hard gate: does not proceed until score ≥ 9.** If you try to skip, it blocks you and continues the next round automatically.

### Phase 2 — Paddle Into Position
6 readiness metrics, scored 0–10 each:
- Goal Achievability
- Strategy Clarity
- Technical Feasibility
- Resource Clarity
- Risk Awareness
- **Testability**

Composite score = floor(average of 6). **Hard gate: does not proceed until composite ≥ 9.**

### Phase 3 — Drop In
Synthesizes everything learned across all phases into a **Groundswell Summary**, asks you to confirm it, then presents 4 options:

| Option | What it does |
|--------|-------------|
| **A) Task Plan** | Full plan with Acceptance Criteria, Definition of Done, Test Strategy, implementation options, risks |
| **B) Drop In Now** | Skip the plan — begin implementation directly |
| **C) Back to Phase 1** | Re-examine the goal |
| **D) Back to Phase 2** | Resolve remaining gaps |

---

## Key design principles

**Hard gates are non-negotiable.** Phases 1 and 2 cannot be skipped, even if you ask. The skill blocks, explains what's missing, and immediately continues — no confirmation needed, no exceptions.

**Insights accumulate across every round.** Every Q&A round extracts Insights, Discoveries, and Signals from your answers. These feed directly into the Phase 3 synthesis. Nothing learned is lost.

**Simple is better.** When two approaches have similar merit, the simpler one is always recommended. Proven and boring beats clever and fragile.

**Scoring is objective.** Every score is evidence-based, traceable to specific answers. Vague answers score partially. Missing answers score zero. Scores can only increase when new answers provide new evidence.

**Context persists.** All progress is saved to `./{title}.groundswell.json`. If a session is interrupted — by a restart, agent swap, or anything else — it resumes exactly where it left off.

---

## Context file

Each session creates a `.groundswell.json` file in the current working directory:

```
./user-auth-api.groundswell.json
```

This file contains the full session state: tech context, all Q&A rounds, extracted insights, scores, and the final synthesis. It is the single source of truth — agents read it before writing, never overwrite without reading first.

---

## Installation

### Claude Code / OpenCode

```bash
# Copy SKILL.md into your skills directory
mkdir -p ~/.claude/skills/groundswell
cp SKILL.md ~/.claude/skills/groundswell/SKILL.md
```

Or use the packaged `.skill` file if your environment supports it.

---

## Compatibility

- **Claude Code** ✅
- **OpenCode** ✅
- Works with any agent that supports skill/SKILL.md conventions

---

## Score calibration

| Score | Meaning |
|-------|---------|
| 0–2 | Flat ocean — nothing to work with |
| 3–4 | Tiny ripple — vague, nothing actionable |
| 5–6 | Choppy chop — some content, key details missing |
| 7–8 | Building swell — mostly clear, one gap remains |
| 9 | **Groundswell arrived** — complete and specific |
| 10 | Perfect set — edge cases covered too |

---

## File structure

```
groundswell/
├── README.md       ← you are here
└── SKILL.md        ← the skill definition (Claude reads this)
```

---

## License

MIT
