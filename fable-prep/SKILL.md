---
name: fable-prep
description: Compatibility alias for model-handoff in Architecture mode. Use when a user invokes fable-prep, asks to prepare an architecture or design session, map one or more codebases for an architect, or assemble an evidence-backed architecture handoff. Follow model-handoff/SKILL.md and apply its Architecture receiver-prompt variant; do not assume a specific model, budget, or routing workflow.
---

# Fable Prep compatibility alias

`fable-prep` now delegates to [model-handoff](../model-handoff/SKILL.md) in **Architecture** mode.

Use the canonical skill’s workflow and handoff template. Produce the Architecture result: design deltas, boundaries and contracts, plus a dependency-ordered build plan with acceptance checks. Keep its facts, inferences, decisions, seams, gaps, verification surface, and receiver prompt intact.

This alias preserves existing invocations while avoiding provider-specific assumptions.
