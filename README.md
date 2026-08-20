# saad-skills-core

A small, public collection of agent skills. This repository is an intentional
publish mirror of selected skills from Saad's private `~/.agents/skills` store;
it is not scanned as a runtime source on the originating Mac.

The [PUBLISHED_SKILLS](PUBLISHED_SKILLS) manifest is the public mirror
allowlist. Changes are authored privately, checked there with
`~/tailgate/cadastre/cadastre skills publish`, then copied here only with its
explicit `--apply` flag. That command never commits or pushes. Do not add every private
skill here, and do not edit a mirror copy expecting the Mac runtime to use it.

## Skills

- **dev-research** — Generate a structured Perplexity Deep Research prompt for technical/coding questions, with Library, Architecture, and Debug modes plus a cross-domain "collision layer."
- **model-handoff** — Prepare compact, evidence-backed handoffs for architecture, diagnosis, review, research, and planning; includes shared maps, facts/inferences/decisions, seams/gaps, verification, and a receiver prompt.
- **fable-prep** — Compatibility alias for **model-handoff** in Architecture mode.
- **harry-dry-copywriting** — Write copy using Harry Dry's framework, for headlines, ads, landing pages, taglines, and other persuasive copy.
- **question-map** — Design or audit a landing page by mapping each section to the single visitor question it must answer (Design, Audit, and Panel modes).

## Usage

Drop a skill folder into your agent's skills directory (for example,
`~/.claude/skills/`) to make it available. This is appropriate for external
consumers; the originating Mac uses its private canonical store instead.

The originating Mac uses Cadastre for skill health and operations:

```sh
~/tailgate/cadastre/cadastre skills schema
~/tailgate/cadastre/cadastre skills check
~/tailgate/cadastre/cadastre skills sync --apply
~/tailgate/cadastre/cadastre skills publish --apply
```
