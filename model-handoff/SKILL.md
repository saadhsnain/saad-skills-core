---
name: model-handoff
description: Prepare an evidence-backed, compact handoff for another model or specialist session. Use when work needs to move between agents, models, teams, or contexts; when a task needs architecture, diagnosis, review, research, or planning input; or when the receiver would otherwise need to rediscover the repository and prior reasoning. Classifies the work as a bolt-on or greenfield effort, builds comparable maps across sources, separates facts from inferences and decisions, identifies seams and gaps, defines verification, and produces a receiver-ready prompt.
---

# Model Handoff

Make the next session spend its time on the question at hand, not reconstructing context. Produce a compact, evidence-backed handoff that is useful to any capable receiver; do not assume a particular model, vendor, budget, registry, or routing scheme.

Use this skill for architecture, diagnosis, review, research, and planning. It can support implementation later, but the handoff itself is for clear judgment and an accountable next step.

## 1. Frame the work

First inspect only enough of the relevant sources to establish the existing shape: project guidance, manifests, entry points, the affected domain, tests, and live systems when the task depends on live state.

Classify the work explicitly:

- **Bolt-on:** a bounded change attaches to existing scaffolding, contracts, or flows. Name what already exists and the seams it should reuse.
- **Greenfield:** no meaningful scaffold exists for the target. Name the boundaries that still constrain it: adjacent systems, users, interfaces, data, operations, or delivery constraints.

Do not call something greenfield merely because the requested feature is new. State the classification and its evidence in 2–5 lines before doing deeper mapping.

## 2. Build compact maps

Create one focused map per repo, service, document set, or other source. Keep the same structure across sources so the receiver can compare them directly. A map is a query surface, not a code dump: cite paths, symbols, issue links, commands, tables, or observations; summarize rather than paste files.

Use this structure, omitting sections that truly do not apply and marking them `Not applicable`:

1. **Role and entry points** — what this source owns, its stack or format, and the relevant starting points.
2. **Current behavior or state** — the affected flow, interfaces, data, configuration, or operating behavior.
3. **Constraints and invariants** — contracts, permissions, compatibility requirements, non-negotiables, and known hazards.
4. **Seams to reuse** — exact extension points and nearby patterns that make a bolt-on safe.
5. **Gaps and unknowns** — what is absent, ambiguous, stale, or needs live verification.
6. **Evidence** — concise pointers supporting the map, including what was inspected and when it matters.

For sources with a live database, deployment, API, or external system, distinguish repository evidence from live evidence. Verify volatile claims when practical; otherwise label them as needing verification rather than presenting them as current fact.

## 3. Keep the reasoning legible

Separate statements into three classes throughout the handoff:

- **Facts** are directly evidenced observations. Attach a pointer.
- **Inferences** are conclusions drawn from facts. State the reasoning and confidence or uncertainty.
- **Decisions** are choices already made or choices the receiver must make. Name the owner when known and the consequence of choosing differently.

Do not smuggle a preference into a fact, or represent an unverified assumption as a decision. If a decision is unresolved, give a short recommendation and the tradeoff rather than inventing approval.

## 4. Define the handoff

Deliver a single compact package in this order:

```markdown
# <Task> handoff

## Objective
<one paragraph: desired outcome, mode, and bolt-on or greenfield classification>

## What is established
- Fact — <claim> (<evidence pointer>)

## Maps
### <source>
<the shared six-part map>

## Reasoning and decisions
- Inference — <claim>; because <facts>; <confidence/unknown>
- Decision — <made or needed>; owner: <if known>; consequence: <tradeoff>

## Seams, gaps, and risks
- Seam — <where the work attaches and what it must preserve>
- Gap — <unknown>; resolve by <smallest useful check>

## Verification surface
- <acceptance check, test, inspection, or live-state confirmation>

## Receiver prompt
<paste-ready prompt>
```

The verification surface is part of the design, not an afterthought. Cover the narrowest checks that would disprove the proposed result: affected automated tests, type/build checks, contract or migration checks, user-flow checks, review criteria, research-source criteria, or live-state confirmation. State checks that cannot yet run and why.

## 5. Write the receiver prompt

End with a self-contained prompt tailored to the mode. Give the receiver the objective, classification, maps, established facts, explicit decisions, open questions, and required verification. Ask it to challenge fragile inferences and unowned decisions before proposing work.

Use this core shape:

```text
You are receiving a <mode> handoff for <objective>.

Treat the evidence below as the current working context. Preserve stated
constraints and reuse named seams where they fit. Facts are evidenced;
inferences may be challenged; decisions marked “needed” require an explicit
recommendation rather than an invented approval.

<paste the compact handoff>

Return:
1. <mode-specific result>;
2. the assumptions or inferences you would change, with reasons;
3. the smallest ordered next steps; and
4. the verification results or checks required before this is accepted.
Do not rediscover unrelated parts of the sources or rewrite the system from
scratch unless the evidence shows this is greenfield or the existing seams are
unsafe.
```

Set `<mode-specific result>` as follows:

- **Architecture:** design deltas, boundaries/contracts, and a dependency-ordered build plan.
- **Diagnosis:** ranked root-cause hypotheses, discriminating checks, and a minimal safe fix path.
- **Review:** findings ordered by severity, evidence, regressions, and concrete remediation.
- **Research:** answers with source quality, uncertainty, and implications for the stated decision.
- **Planning:** milestones, dependencies, ownership assumptions, risks, and acceptance checks.

## Guardrails

- Keep the package proportional to the decision. Prefer a few high-signal maps over exhaustive inventories.
- Preserve source ownership and existing project instructions; do not create registries, automatic routing, or a new persistent format unless requested.
- Do not make the receiver explore raw sources when a focused evidence pointer or map can answer the question.
- Stop for user direction when a missing decision materially changes scope, authority, or an irreversible action.
