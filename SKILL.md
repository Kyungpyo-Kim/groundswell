---
name: groundswell
description: >
  Use this skill whenever a developer wants to solve a problem, plan a feature, design an architecture, improve performance, or think through a complex engineering task. Like a groundswell — the most powerful waves that form deep in the ocean long before they reach the shore — this skill helps you think deeply, choose the right approach, and arrive at the surface ready to execute with full force. Prioritizes: (1) feature development, (2) architecture design, (3) performance improvement. Triggers on phrases like "help me think through", "I need to solve", "brainstorm with me", "let's plan", "deep dive", "how should I design", "아이디어 내줘", "문제 해결", "설계해줘", "브레인스토밍", "성능 개선", or any request that involves ambiguity, strategy, or multi-step engineering work. This skill MUST be used when the request is non-trivial and would benefit from structured alignment, clarification, and ideation. Do NOT skip this skill just because the task seems clear — the most important currents are always beneath the surface.
---

# Groundswell 🌊

> *A groundswell forms deep in the ocean — far from the shore, far from the noise. By the time it arrives, it carries enormous energy with perfect form. The surfer who reads it early, waits patiently, and paddles with purpose is the one who rides it all the way in.*

**Groundswell** helps developers do the same with engineering tasks. It guides you through reading conditions deep beneath the surface, choosing the right wave, preparing until you're truly ready, and dropping in with clarity and force — so every execution is backed by the full energy of deep thinking.

---

## The Groundswell Metaphor

| Ocean | Groundswell Skill |
|-------|------------------|
| 🌊 **Read the deep conditions** | Understand the tech context and task type |
| 🎯 **Choose your wave** | Align on the goal — find the right problem to solve |
| 🚣 **Paddle into position** | Remove ambiguity — prepare until truly ready |
| 🏄 **Drop in** | Synthesize, confirm, and commit to action |

---

## Phases

```
Phase 0: Read the Conditions   — tech context & task classification  [single pass, no gate]
Phase 1: Choose Your Wave      — purpose alignment                   [HARD GATE: score ≥ 9]
Phase 2: Paddle Into Position  — ambiguity removal                   [HARD GATE: score ≥ 9]
Phase 3: Drop In               — synthesis & decision                [confirm → A/B/C/D]
```

All progress is saved to `./{title}.groundswell.json`. This JSON is the **single source of truth** — always load it at the start and update it after every round.

---

## CRITICAL: Hard Gate Rule (Non-Negotiable)

**A surfer who drops in before they're ready wipes out. The gates exist for the same reason.**

If the user attempts to skip Phase 1 or Phase 2 before the score threshold is met:

1. **Do NOT proceed under any circumstances.**
2. Show current score, gap to threshold, and exactly what is missing.
3. Immediately continue with the next round — do not wait for user confirmation.

### Hard Gate Response Template

```
⛔ Not ready to drop in yet.

Current score: {score}/10
Required score: 9/10
Gap: {9 - score} points remaining

The groundswell isn't here yet. What's still forming:
{specific list of what was not answered or was too vague}

Paddling out without this risks:
{wrong direction, wasted effort, building the wrong thing}

Let's keep paddling. Next round:
[immediately present next round questions]
```

**No exceptions. No workarounds. Score must reach 9 before moving forward.**

---

## CRITICAL: Context Persistence Rule

Before doing ANYTHING, check if a session file already exists:

```bash
ls ./*.groundswell.json 2>/dev/null | head -1
```

- **EXISTS** → Load it, read current phase and round, RESUME from where it left off. Summarize state to the user before proceeding.
- **NOT FOUND** → Run Phase 0 to initialize.

**Never restart a session that has already begun. A wave doesn't reset — it keeps rolling.**

---

## CRITICAL: Title Auto-Generation Rule

The agent derives the session title automatically. **Never ask the user for a title.**

1. Extract the core noun phrase from the user's first message (e.g., "add user authentication to the API" → `user-auth-api`)
2. Slugify: lowercase, words joined by hyphens, max 5 words
3. Save as `./{title}.groundswell.json`
4. If a file with that name already exists (different task), append date suffix: `{title}-{MMDD}`

---

## CRITICAL: Insight Accumulation Rule

After every round of Q&A (in any phase), extract and record from the user's answers:

- **Insights**: Non-obvious things that reframe the problem or change solution direction
- **Discoveries**: Facts, constraints, or context that emerged through questioning
- **Signals**: Recurring themes, emphases, or hesitations suggesting hidden priorities

> *Just like how a groundswell carries energy from storms thousands of miles away — every answer carries information beneath its surface. Capture it all.*

Stored in each round record and in the top-level `accumulated_insights` array. These feed directly into Phase 3's synthesis. Nothing learned in Q&A should be lost.

---

## JSON Schema

```json
{
  "title": "<auto-generated slug>",
  "created_at": "<ISO timestamp>",
  "updated_at": "<ISO timestamp>",
  "current_phase": 0,
  "task_type": "feature_development | architecture_design | performance_improvement | bug_fix | refactoring | security | other",
  "tech_context": {
    "language": "<e.g. TypeScript, Python, Go>",
    "frameworks": ["<e.g. NestJS, React, FastAPI>"],
    "infra": "<e.g. AWS ECS, k8s, on-prem>",
    "codebase_size": "small | medium | large | monorepo",
    "test_coverage": "<e.g. 60%, unknown, none>",
    "ci_cd": "<e.g. GitHub Actions, Jenkins, none>",
    "team_size": "<number or range>",
    "existing_constraints": ["<e.g. no new dependencies, must be backward compatible>"]
  },
  "accumulated_insights": [
    {
      "phase": 1,
      "round": 1,
      "insights": ["..."],
      "discoveries": ["..."],
      "signals": ["..."]
    }
  ],
  "phase1_alignment": {
    "status": "in_progress | complete",
    "rounds": [
      {
        "round": 1,
        "questions": ["Q1", "Q2", "Q3"],
        "answers": ["A1", "A2", "A3"],
        "insights_extracted": ["..."],
        "discoveries": ["..."],
        "signals": ["..."],
        "score_breakdown": {
          "goal_clarity": 7,
          "success_criteria_clarity": 6,
          "constraint_clarity": 8
        },
        "score": 7,
        "score_rationale": "...",
        "gaps_identified": ["..."]
      }
    ],
    "final_score": null,
    "aligned_goal": null
  },
  "phase2_ambiguity": {
    "status": "not_started | in_progress | complete",
    "rounds": [
      {
        "round": 1,
        "questions": ["Q1", "Q2", "Q3"],
        "answers": ["A1", "A2", "A3"],
        "insights_extracted": ["..."],
        "discoveries": ["..."],
        "signals": ["..."],
        "metrics_after": {
          "goal_achievability": 0,
          "strategy_clarity": 0,
          "technical_feasibility": 0,
          "resource_clarity": 0,
          "risk_awareness": 0,
          "testability": 0
        },
        "composite_score": 0,
        "score_rationale": "...",
        "remaining_ambiguities": ["..."]
      }
    ],
    "final_score": null,
    "clarified_context": null
  },
  "phase3_synthesis": {
    "status": "not_started | awaiting_confirmation | confirmed | complete",
    "summary": null,
    "user_confirmed": false,
    "user_decision": "plan | execute | back_to_phase1 | back_to_phase2 | null",
    "decision_note": "",
    "output": null
  }
}
```

---

## Scoring: Objectivity Rules

**All scores must be grounded in evidence from the user's actual answers. Never inflate. A wave doesn't pretend to be bigger than it is.**

### Principles

1. **Evidence-based only**: Every point awarded must trace to a specific answer. If not stated, it cannot be assumed.
2. **Missing = zero**: If a sub-dimension was not addressed, score it 0.
3. **Vague = partial**: An incomplete answer earns partial credit only.
4. **No 9+ without specificity**: Requires concrete information — not enthusiasm or intent.
5. **Score what was said, not intended**: Do not infer beyond what was explicitly communicated.
6. **Round down**: When between integers, take the lower one.
7. **No increase without new evidence**: If the user asks for a higher score without new information, decline.

### Calibration

| Score | Meaning |
|-------|---------|
| 0–2   | Flat ocean — nothing to work with |
| 3–4   | Tiny ripple — vague gesture, nothing actionable |
| 5–6   | Choppy chop — some content, key details missing |
| 7–8   | Building swell — mostly clear, one gap remains |
| 9     | Groundswell arrived — complete and specific |
| 10    | Perfect set — exceptionally thorough, edge cases covered |

**When in doubt, score lower. A false 9 is a wipeout waiting to happen.**

---

## Phase 0: Read the Conditions

**Goal**: Classify the task and capture tech context. Single pass — no scoring, no loop.

> *Before entering the water, every good surfer reads the conditions: wave height, period, wind, tide. You can't choose a wave you haven't read.*

### Task Type Priority

1. **Feature Development** — adding new functionality
2. **Architecture Design** — designing or redesigning system structure
3. **Performance Improvement** — resolving bottlenecks

Other types (bug fix, refactoring, security, etc.) are supported with less specialized treatment.

### Instructions

1. Determine task type from the user's first message.
2. Ask up to **4 short questions** for any context not already clear:
   - Language & frameworks
   - Infrastructure / deployment environment
   - Codebase size and test coverage
   - Key constraints (no new deps, backward compat, team rules, etc.)

   If the message already provides this, skip questions and infer.

3. Populate `task_type` and `tech_context` in the JSON.

4. Confirm and immediately proceed to Phase 1:

```
🌊 Conditions read.
📋 Task type: {task_type}
🛠 Tech context: {language} / {frameworks} / {infra}
→ Paddling out to Phase 1: Choose Your Wave
```

---

## Phase 1: Choose Your Wave

**Goal**: Ensure the user and agent are locked onto the same wave. Concretize the goal, success criteria, and constraints.

> *There are many waves. Only a few are worth riding. This phase is about choosing the right one — not the first one.*

### Instructions

1. Tailor questions to the task type:
   - **Feature Development**: real user need, edge cases, integration points
   - **Architecture Design**: current pain points, scale targets, team/org constraints
   - **Performance Improvement**: degrading metric, at what scale, what's been tried

2. Ask exactly **3 targeted questions** per round.

3. After answers, extract insights/discoveries/signals and record in JSON.

4. Calculate `alignment_score`:
   - **goal_clarity** (0–10): Is the core goal specific and unambiguous?
   - **success_criteria_clarity** (0–10): Is success observable or measurable?
   - **constraint_clarity** (0–10): Are limits, non-goals, and scope boundaries defined?
   - `alignment_score` = floor(average of three)

5. A score of 9 is achievable in Round 1 if answers are complete and specific.

6. **If score < 9** → Identify remaining gaps. 3 new questions. Next round.

7. **If user tries to skip with score < 9** → Apply **Hard Gate Rule**. Immediately continue.

8. **If score ≥ 9** → Set `aligned_goal`, mark complete, proceed to Phase 2.

### Output Format (Phase 1)

```
## 🎯 Phase 1: Choose Your Wave — Round {N}

**Q1**: [question]
**Q2**: [question]
**Q3**: [question]

---
[After user answers]

### 🌊 What the Water Told Us This Round
- **Insight**: ...
- **Discovery**: ...
- **Signal**: ...

### 📊 Wave Selection Score: {score}/10
| Dimension            | Score | Evidence basis |
|----------------------|-------|----------------|
| Goal Clarity         | x/10  | [what was/wasn't said] |
| Success Criteria     | x/10  | [what was/wasn't said] |
| Constraint Clarity   | x/10  | [what was/wasn't said] |
| **Wave Score**       | **x/10** | |

Rationale: [objective explanation citing specific gaps]
Wave so far: [summary of aligned goal]
Still forming: [remaining gaps, or "None — paddling to Phase 2"]
```

---

## Phase 2: Paddle Into Position

**Goal**: Ensure the task is actionable. Evaluate 6 readiness dimensions until the groundswell is truly readable.

> *Paddling into position takes effort and patience. You have to get past the break, past the chop, into the right spot — before the wave arrives. Skip this and you'll never be in the right place at the right time.*

### Readiness Metrics (0–10 each)

| Metric | What it measures |
|--------|-----------------|
| `goal_achievability` | Can this goal realistically be achieved given known constraints? |
| `strategy_clarity` | Is there a plausible, concrete path to the goal? |
| `technical_feasibility` | Are technical requirements and capabilities understood? |
| `resource_clarity` | Time, people, budget, and data accounted for? |
| `risk_awareness` | Key risks, unknowns, and blockers identified? |
| `testability` | Can the solution be verified? Are test boundaries and types clear? |

**Composite score** = floor(average of all six)

### Instructions

1. Briefly reference key Phase 1 insights and how they shape this phase's questions.
2. Ask **3 targeted questions** at the lowest-scoring metrics (all start at 0 in round 1).
3. After answers, extract insights/discoveries/signals. Update all six metrics.
4. Show scorecard with evidence basis for each metric.
5. **If composite < 9** → Target weakest metrics, 3 new questions, next round.
6. **If user tries to skip with composite < 9** → Apply **Hard Gate Rule**. Immediately continue.
7. **If composite ≥ 9** → Write `clarified_context`, mark complete, proceed to Phase 3.

### Output Format (Phase 2)

```
## 🚣 Phase 2: Paddle Into Position — Round {N}

[1–2 sentence recap of key Phase 1 insights shaping these questions]

**Q1**: [question targeting weakest metric]
**Q2**: [question targeting second weakest metric]
**Q3**: [question targeting critical ambiguity]

---
[After user answers]

### 🌊 What the Water Told Us This Round
- **Insight**: ...
- **Discovery**: ...
- **Signal**: ...

### 📊 Readiness Scorecard
| Metric                | Score | Evidence basis |
|-----------------------|-------|----------------|
| Goal Achievability    | x/10  | [what was/wasn't said] |
| Strategy Clarity      | x/10  | [what was/wasn't said] |
| Technical Feasibility | x/10  | [what was/wasn't said] |
| Resource Clarity      | x/10  | [what was/wasn't said] |
| Risk Awareness        | x/10  | [what was/wasn't said] |
| Testability           | x/10  | [what was/wasn't said] |
| **Composite**         | **x/10** | |

Still forming: [remaining ambiguities, or "None — ready to drop in"]
```

---

## Phase 3: Drop In

**Goal**: Consolidate everything learned, confirm with the user, and commit to action.

> *The drop-in is the moment of commitment. Before you go, you read one last time — conditions, angle, position. Then you paddle hard, pop up, and ride. This phase is that final read before full commitment.*

This phase has **no scoring loop** and **no hard gate**. It is the structured handoff from thinking to doing.

### Step 1: Generate the Groundswell Summary

Compile everything from the JSON into a structured summary. This is not a plan — it is a faithful consolidation of all that was learned beneath the surface.

```
## 🌊 Groundswell Summary — {title}

### Conditions
- Task type: {task_type}
- Tech context: {language} / {frameworks} / {infra}
- Constraints: {existing_constraints}

### The Wave (Aligned Goal)
{aligned_goal}

### What We Read Beneath the Surface (Clarified Context)
{clarified_context}

### Currents & Depths (Accumulated Insights)
[grouped by phase]
- [Phase 1, Round N] Insight: ...
- [Phase 1, Round N] Discovery: ...
- [Phase 2, Round N] Signal: ...

### The Full Picture
{A concise paragraph (3–5 sentences) synthesizing the goal, context, and top insights into a single coherent picture of the problem and its constraints.}

### Open Questions (if any)
{Remaining unknowns that are acceptable to proceed with, and how to handle them}
```

Save to `phase3_synthesis.summary` in the JSON. Set status to `awaiting_confirmation`.

### Step 2: Confirm Before Dropping In

Present the summary and ask the user to confirm before proceeding.

```
---
Does this summary capture everything accurately?
Confirm, or tell me what's still off — we'll correct it before we drop in.
```

**Do not offer next steps until the user confirms.**

If the user identifies errors:
- Correct the summary and record the correction in the JSON
- Re-present and ask for confirmation again
- Repeat until confirmed

Once confirmed, set `user_confirmed: true` in the JSON.

### Step 3: Choose How to Ride

After confirmation, present the decision options:

```
## 🏄 Summary confirmed. How do you want to ride this wave?

**A) 📋 Write the Task Plan**
   → Full plan: Acceptance Criteria, Definition of Done, Test Strategy,
     implementation options, risks, and next actions.
   → Best when: you want to review or share the plan before paddling in.

**B) ⚡ Drop In Now**
   → Skip the plan — begin implementation directly from the confirmed summary.
   → Best when: the wave is clear and you want to move fast.

**C) 🔁 Back to Phase 1 — Re-read the Wave**
   → Return to Choose Your Wave to re-examine the goal.
   → Best when: the summary revealed the goal itself needs adjustment.

**D) 🔁 Back to Phase 2 — Keep Paddling**
   → Return to Paddle Into Position to resolve remaining gaps.
   → Best when: the summary revealed something important still needs clarifying.

Choose A, B, C, or D — or tell me what you'd like to adjust.
```

Record the user's choice in `phase3_synthesis.user_decision` and notes in `decision_note`.

### Step 4: Execute the Decision

#### Option A — Write the Task Plan

```markdown
# Task Plan: {title}

## Task Type
{task_type}

## Tech Context
- Language/Framework: {language} / {frameworks}
- Infrastructure: {infra}
- Constraints: {existing_constraints}

## The Wave (Goal)
{aligned_goal}

## Conditions (Context)
{clarified_context}

## Currents That Shaped This Plan
- [insight]: [how it influenced the approach]
- [discovery]: [what it constrained or unlocked]
- [signal]: [what it emphasized]

## Acceptance Criteria
- [ ] {specific, observable condition that must be true for completion}
- [ ] {e.g. "existing API contract unchanged", "P99 latency < 200ms at 1000 RPS"}

## Definition of Done
- [ ] Code reviewed and approved
- [ ] All acceptance criteria verified
- [ ] Tests written and passing (see Test Strategy)
- [ ] No regression in existing test suite
- [ ] Deployed to staging / production (as applicable)
- [ ] Relevant documentation updated

## Test Strategy
| Type | What to test | Priority |
|------|-------------|----------|
| Unit | {specific functions/modules} | High |
| Integration | {specific integration points} | Medium |
| E2E | {critical user paths, if applicable} | Low/Medium |
| Performance | {specific metric and threshold, if applicable} | — |

## Approach & Implementation Steps

### Option 1: {name} — Recommended
**Why**: {grounded in insights and constraints}
**Simplicity check**: {pass / ⚠️ fail + reason}
**Steps**:
1. ...
2. ...
**Test approach**: {how this will be tested}

### Option 2: {name} — Alternative
...

## Key Risks & Mitigations
| Risk | Surfaced in | Mitigation |
|------|------------|-----------|
| ...  | Phase X, Round Y | ... |

## Next Immediate Actions
1. ...
2. ...
3. ...
```

After presenting, ask: *"Ready to paddle in? I can start with Step 1 now."*

#### Option B — Drop In Now

Begin implementation directly using the confirmed summary as context. State which step you're starting with and proceed. Do not generate a plan document unless asked.

#### Option C — Back to Phase 1

Reset `phase1_alignment.status` to `in_progress` and `current_phase` to `1` in the JSON.

```
🔁 Paddling back to Phase 1: Choose Your Wave

The summary showed this needs re-reading:
{specific reason from user input}

[Continue with Phase 1, next round]
```

#### Option D — Back to Phase 2

Reset `phase2_ambiguity.status` to `in_progress` and `current_phase` to `2` in the JSON.

```
🔁 Back to Phase 2: Keep Paddling

The summary showed this still needs clarifying:
{specific reason from user input}

[Continue with Phase 2, next round]
```

---

## Saving State

After every round in any phase, immediately write the full updated JSON:

```bash
cat > ./{title}.groundswell.json << 'EOF'
{...full updated json...}
EOF
```

---

## Quick Reference

```
Phase 0: Read the Conditions   [single pass]
  └─► Phase 1: Choose Your Wave     (score < 9 → repeat | skip → BLOCK)
        └─► Phase 2: Paddle Into Position  (score < 9 → repeat | skip → BLOCK)
              └─► Phase 3: Drop In
                    ├── A → Task Plan → offer to paddle in
                    ├── B → Drop in directly
                    ├── C → Back to Phase 1
                    └── D → Back to Phase 2
```

---

## Agent Robustness Notes

- **Don't lose the current**: The JSON is always ground truth. If confused, re-read it and summarize current state before proceeding.
- **Stay oriented**: Begin each response by confirming phase, round, and latest score (Phases 1–2) or decision status (Phase 3).
- **Carry the energy forward**: Phase 3 must re-read all `accumulated_insights` from the JSON — do not rely on context window memory alone.
- **Parallel paddlers**: The JSON is shared context across agents. Always read before writing.
- **Score integrity**: Scores rise only when new answers provide new evidence. Decline any request to inflate without new information.
- **Artifact inputs**: Code snippets, PR links, ERDs, API specs — treat them as answers. Extract insights and discoveries and record in `accumulated_insights`.
- **Simplicity current**: When two approaches have similar merit, always recommend the simpler one. The cleanest line through a wave is usually the best one.
