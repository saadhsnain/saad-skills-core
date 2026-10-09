---
description: |-
  Generate a structured Perplexity Deep Research prompt for a technical/coding question — grounded in project context and session findings. Adapts to three modes: Library (comparing/choosing tools), Architecture (design decisions), and Debug (root-cause investigations). Always includes a cross-domain collision layer to escape the obvious technical literature.
notion_page_id: 3f1c4982-f61e-815e-85ee-fa3ddf8e1741
name: "dev-research"
---

# Dev Research

Generate a Perplexity Deep Research prompt calibrated for a specific technical question — not generic tutorials. The prompt has two layers:

1. **Technical layer** — test current assumptions, compare real tradeoffs, fill named knowledge gaps
2. **Collision layer** — find structural mechanisms from distant domains that apply to the problem (escaping "just read the docs" territory)

Automatically detects mode from the question:

- **Library mode** — choosing or comparing tools/libraries/services
- **Architecture mode** — design decisions, patterns, system structure
- **Debug mode** — root-cause investigation, failure analysis

## Usage

```javascript
/dev-research <question or problem>
```

Examples:

- `/dev-research which state management approach for this React app` ← Library
- `/dev-research how to structure multi-tenant data isolation in Postgres` ← Architecture
- `/dev-research why React Query cache invalidation isn't firing after mutation` ← Debug

---

## Steps

### 1 — Gather project context

Read what's already known about the problem:

**From the project:**

```bash
# If in a project directory — read README for stack/constraints
cat README.md 2>/dev/null | head -80
# Check relevant config files for constraints
cat package.json 2>/dev/null | python3 -c "import sys,json; d=json.load(sys.stdin); print(json.dumps(d.get('dependencies',{}), indent=2))" 2>/dev/null
```

**From the current session:** Review what was already tried, errors seen, approaches rejected.

**Infer the mode** from the question type:

- "which X vs Y", "should I use", "best way to" → **Library mode**
- "how to structure", "pattern for", "design for" → **Architecture mode**
- "why isn't", "broken", "not working", error messages → **Debug mode**

---

### 2 — Extract the three ingredients

From context gathered, pull:

**A. Current assumptions** — what the user is working with or believes. These become "verify or challenge" targets.

**B. Constraints** — stack, team size, scale, performance requirements, non-negotiables. These bound what's actually relevant.

**C. Named gaps** — the specific unknowns that are blocking progress. These become the research questions.

---

### 3 — Generate the Perplexity Deep Research prompt

Output the prompt directly — formatted for copy-paste into Perplexity Deep Research.

---

#### Library Mode prompt:

```javascript
RESEARCH BRIEF: <decision question>

## Stack context

<1–2 sentences on the relevant stack. What's already decided that bounds this choice.>

## What I currently believe

<List 2–4 specific working assumptions about these options. Assertive claims, not questions.>

- [Library A] seems better for X because Y
- My concern with [Library B] is Z
- I currently think the tradeoff is between [P] and [Q]

## Constraints that matter

<Specific constraints this must fit — scale, team, existing patterns, non-negotiables.>

- Scale: ...
- Team: ...
- Must integrate with: ...

## The specific questions I need answered

<3–5 specific, answerable questions — not "compare X and Y" but "under condition Z, does X outperform Y on metric M?">

1. At [scale/team/constraint], which actually wins on [metric]?
2. What are the real migration costs if I choose wrong here?
3. What do teams who regret choosing [option] say they missed?
4. ...

## Cross-domain collisions

I want to escape "just compare the benchmarks" territory. Find structural mechanisms from distant domains — not software — where the same underlying tradeoff appears:

- Identify 2–3 cases from outside software engineering (e.g. urban planning, biological systems, logistics, materials science, economics, game theory) where the same core tradeoff — between [summarize the tradeoff] — plays out
- For each: name the mechanism, explain the structural parallel, and state one implication for this decision the benchmark comparison wouldn't surface
- **Kill signal**: if removing the domain name leaves a generic observation, skip it. The domain must change the claim.

## Output format

1. **Verdict on each assumption** — confirm, refine, or overturn with evidence.
2. **Constraint analysis** — which option fits this specific context best, and why the generic recommendation differs.
3. **Answers to specific questions** — direct answers with citations or real-world examples.
4. **Cross-domain collisions** — 2–3 structural analogues with mechanism explained. One implication per analogy.
5. **The thing most teams miss** — what's underweighted in the standard comparison.
6. **Decision recommendation** — if you had to choose for this context, what and why.
```

---

#### Architecture Mode prompt:

```javascript
RESEARCH BRIEF: <architecture problem>

## Context

<Stack, scale, and the specific thing being designed. What's already decided.>

## Current approach / assumptions

<What the current design looks like or what's being assumed about the right approach.>

- Currently planning to: ...
- Assuming that: ...
- Treating X as: ...

## The constraints this must satisfy

<Non-negotiables, performance requirements, team constraints, existing system boundaries.>

## Where I'm stuck / the real question

<The specific decision point or tension — not "how to do X" but "X vs Y given constraint Z" or "what breaks at scale N?">

## Specific questions

<3–5 specific, answerable questions.>

1. What breaks first in this design at [scale / load / team size]?
2. What does [approach A] get right that [approach B] misses at this layer?
3. Which teams have done something close to this, and what did they change after 12 months?
4. ...

## Cross-domain collisions

Before I read the standard software architecture literature on this, I want structural analogues from outside software:

- Identify 2–3 mechanisms from distant fields (logistics, biology, architecture, city planning, chemistry, military operations) where the same structural challenge — [summarize the challenge] — was solved
- For each: the mechanism, the structural parallel, and one implication for this design that the standard architecture patterns don't surface
- **Kill signal**: if removing the domain name leaves a generic principle, skip it.

## Output format

1. **Assessment of current approach** — what's sound, what's fragile, what breaks first.
2. **Answers to specific questions** with evidence and real cases.
3. **Cross-domain collisions** — 2–3 structural analogues with one implication each.
4. **The failure mode most teams don't see** — what usually goes wrong after 12 months with this pattern.
5. **What I'd change** — specific adjustments to the approach given these constraints.
```

---

#### Debug Mode prompt:

```javascript
RESEARCH BRIEF: <bug or failure description>

## Environment

<Stack versions, relevant config, deployment context. Be specific.>

- Framework/library versions: ...
- Relevant config: ...
- Where it fails (dev/prod/specific env): ...

## What I observe

<Exact symptoms — error messages, unexpected behavior, when it started.>

- Error: `<exact error text>`
- Behavior: ...
- Started after: ...

## What I've already tried

<List approaches attempted and what they ruled out.>

- Tried X → result was Y → ruled out Z
- Tried A → ...

## Current hypothesis

<Best current theory about root cause. Even if uncertain, state it.>

I currently think the root cause is: ...
The evidence for this is: ...
The evidence against is: ...

## Specific questions

<3–5 targeted questions — not "how do I fix X" but "is it possible that Y causes Z in version N?">

1. Can [specific mechanism] cause [symptom] when [condition]?
2. Is there a known interaction between [A] and [B] at version [N]?
3. What does this error actually mean at the source level — what state triggered it?
4. ...

## Cross-domain collisions

Debugging is diagnosis under uncertainty. Find structural parallels from other diagnostic fields:

- Identify 2–3 diagnostic frameworks from outside software (medicine, aviation accident investigation, mechanical failure analysis, epidemiology) that would approach this failure differently
- For each: the framework name, how it would reframe the investigation, and one specific thing it would check that I haven't
- **Kill signal**: if the analogy doesn't change what I'd look at next, skip it.

## Output format

1. **Root cause assessment** — most likely cause based on symptoms and what was tried.
2. **Answers to specific questions** with citations or source-level explanation.
3. **Cross-domain diagnostic reframes** — 2–3 with one new thing to check per analogy.
4. **Elimination checklist** — 4–6 concrete things to verify in order of likelihood.
5. **What would confirm/rule out the current hypothesis** — specific test to run.
```

---

### 4 — Report

After outputting the prompt:

- State which mode was used and why
- Name the strongest assumption it will test
- Name the collision domain it will push Perplexity toward
- Remind: paste into Perplexity Deep Research → copy full output → save findings to `research/` if worth keeping long-term

---

## Quality gates

- Every assumption listed must come from actual session context or project code — don't invent beliefs
- Constraints must be real (from README, package.json, what was said) — not generic
- Collision domains must be named specifically — not "another field"
- Debug mode: if no hypothesis exists, ask "what's your best guess at root cause?" before generating — a prompt without a hypothesis to test produces shallow output
- If the question is too vague (e.g. `/dev-research help with React`), ask one clarifying question: "What's the specific decision or problem you're stuck on?"
