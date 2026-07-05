---
name: fable-prep
description: Prepare cheap, high-signal inputs BEFORE spending a scarce frontier model (Fable) on an architecture or design session, so its tokens go to judgment instead of codebase exploration. Use when the user is about to prompt Fable (or any expensive/metered top model) to architect a system, design a data layer, or plan a build across one or more codebases — especially phrases like "help me prep for Fable", "before I prompt Fable", "get me ready for the architecture session", "map the codebase for Fable", or "/fable-prep". Produces compact codebase maps (via cheaper models + attached MCPs), a locked decision list, a usage-timing read, and a paste-ready Fable prompt.
---

# Fable Prep

**The scarce model should spend its tokens on judgment, not exploration.** A frontier model pointed at raw codebases burns its budget reading — the lowest-value thing it can do. This skill front-loads the cheap work (mapping, deciding, timing) so the eventual Fable session is pure architecture: it reads compact maps, honors decisions you already locked, and returns a durable plan cheaper models execute.

Run this in the conversation where the user is *about to* open Fable. It does not call Fable. It hands the user everything to paste into Fable and stops.

## The principle it enforces

- **Frontier = mapmaker, not explorer, not laborer.** Fable designs the architecture and the module-sequenced build plan. Cheaper models (or Codex) do the reading beforehand and the building afterward.
- **The bottleneck is usually the human's unknowns, not the model.** A vague "help me architect this" makes Fable spend scarce tokens asking questions the user hasn't answered. Lock the decisions first.
- **Maps, not dumps.** Every map is filtered, structured, ~2 pages — a query surface for Fable, not a data pour.

---

## Steps

### Step 1 — Frame the task: bolt-on or greenfield?

Before anything, cheaply scan what already exists. Use glob/grep and top-level `ls` — never full-directory reads.

- Identify the repos/sources involved and their stack (package.json, framework, DB).
- Grep for the domain the user is building (`grep -rli "<domain>"`), and check DB migrations / schema folders for tables that already cover it.
- State plainly: **is this a new build, or a few modules bolted onto existing scaffolding?** Most "build me an X" requests are the latter. Naming it as a bolt-on is what keeps the Fable session small.

Report the existing scaffolding back to the user in 3-5 lines before proceeding. If the task turns out greenfield, say so — the rest of the skill still applies, the maps are just thinner.

### Step 2 — Lock the decisions (reduce unknowns)

Read the user's request for unresolved forks — the questions Fable would otherwise stop and ask. Typical shapes: which service/portal owns a feature, merge vs. separate data stores, the integration/bridge mechanism, the identity/permission model, which existing contract to reuse.

Present them as a decision table with a **recommendation and one-line reason** for each. Mark the single decision that most changes the architecture as the one the user must own consciously.

```
| # | Decision | Recommendation | Why |
```

Get the user to confirm or override before the maps are finalized. These locked decisions become a direct input to the Fable prompt.

### Step 3 — Generate the codebase maps (cheap work)

One map per repo/source. Run these on a cheaper model at High effort — or dispatch a read-only subagent per source (worker reads, main decides) — never on Fable. Where a project has an attached MCP (Supabase, DB, etc.), instruct the map to pull **live** schema, since migration files drift.

Give every map the **same section structure** so Fable diffs identical formats instead of parsing different ones. Save each as `<DOMAIN>_MAP.md` at that repo's root.

Reusable map-generation prompt (fill the brackets):

```
Produce a compact, <DOMAIN>-focused map of THIS repo as input for a separate
architecture session. This is a map, not a manual — cite file paths and table
names, no code dumps, aim for ~2 pages. Use glob/grep to locate rather than
reading whole directories. If an MCP (Supabase/DB) is attached, read the LIVE
schema — don't trust migration files alone, they drift.

Context: <one paragraph — what's being built, this repo's role in it, and that
the map exists so the architect designs deltas, not a rebuild>.

Write output to <DOMAIN>_MAP.md at repo root, with these sections:
1. Stack & entry points — framework, build, where <domain> code lives.
2. Data model (LIVE) — every relevant table: columns+types, FKs, RLS policies,
   views/functions/triggers. Note which tables are live vs. superseded.
3. How <domain> surfaces today — the pages/components/screens that render it.
4. Auth & roles — how users/roles work now, what gating exists, where a new
   role could attach without exposing protected data.
5. Assets/integrations — image/storage/3rd-party paths relevant to <domain>.
6. Existing pipelines — any working data flow the new work must not break.
7. Integration seams — the exact places a new module would plug in, and
   existing patterns to reuse.
8. Gaps — what's missing to reach the goal.
End with a 5-line "what the architect most needs to know" summary.
```

### Step 4 — Read the usage timing

If the user shares a usage screenshot (or you can infer it), read the two buckets that matter on metered plans:

- **Frontier/Fable weekly bucket** (often capped at ~50% of total). If fresh, the map session barely dents it — spend it now.
- **All-models weekly bucket.** If this is near its cap and resets soon, do NOT plan to route the *execution* to Sonnet/Opus this week — you'll wall out mid-build. Either wait for the reset to execute, or route execution to **Codex** (separate limit).

Always tell the user to **cap Fable effort at High** — never X-high/Max; the quality gain is ~zero and the cost is 10-50x. The ideal cadence is usually: **Fable maps now, cheap models execute after the reset.**

### Step 5 — Emit the Fable prompt

Assemble the paste-ready prompt. It must: assign the mapmaker role, forbid code and self-exploration, list the inputs (the maps + locked decisions + any shared contract file), specify a durable design-doc output plus a module-sequenced build plan, and include a harden clause that makes Fable argue with the decisions.

```
You are architecting <goal> across <N> existing <stack> repos. Do not write
code. Do not explore the repos yourself — everything you need is in the
attached maps, the locked decisions, and <shared contract file>.

<one paragraph of context + the repos and their roles>

[paste: <DOMAIN>_MAP.md for each repo, the locked decisions, the shared contract]

Produce, as a durable design doc: (1) schema deltas per repo, reusing existing
tables where they fit; (2) the role/permission model; (3) the key flow/contract
(UI + data); (4) the cross-service integration/bridge contract; (5) a build plan
broken into independent modules, each small enough for a cheaper model to
execute in one session, ordered by dependency, each with its acceptance check.
Flag every place my locked decisions create risk or a cheaper alternative
exists.
```

### Step 6 — Hand off

Remind the user of the sequence and stop:
1. Run the map prompts (cheap model / Codex, per repo).
2. Paste maps + decisions + contract into Fable at High effort → get the design doc + build plan.
3. Execute modules one per session on cheaper models / Codex, against Fable's plan.

Offer to sanity-check the maps before the user spends any Fable tokens.

---

## Anti-patterns (the whole point is to avoid these)

- Pointing Fable at raw codebases and saying "explore and architect." Read work is the worst Fable spend.
- Opening Fable before the human decisions are locked — it burns tokens asking what you could have answered.
- Letting Fable write the code. It writes the plan; cheaper models build.
- Maps that are dumps: whole files pasted, no structure, no live schema. Keep them filtered and ~2 pages.
- Using X-high/Max effort. Cap at High.
- Different map formats across repos — Fable should diff identical structures.
