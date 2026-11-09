# saad-skills-core

A small public collection of portable agent workflows. This repository is the authoring source for these packages. Saad's private runtime hub links to them; it no longer maintains separate mirrored bodies.

## Skills

| Package | Purpose |
|---|---|
| dev-research | Build a grounded technical research prompt, with Library, Architecture and Debug modes |
| model-handoff | Prepare an evidence-backed specialist handoff with facts, inferences, decisions and verification |
| fable-prep | Compatibility alias for model-handoff in Architecture mode; requires model-handoff |
| harry-dry-copywriting | Apply Harry Dry's three tests to persuasive copy |
| question-map | Design or audit a landing page around the visitor's questions |

## Installation

Copy the complete desired package directory into the skill directory supported by your agent, preserving bundled references. For example, `model-handoff/` can be installed as `~/.claude/skills/model-handoff/` or `~/.codex/skills/model-handoff/`.

Install **both `fable-prep` and `model-handoff` as siblings** when using the alias; a single isolated fable-prep folder cannot resolve its dependency. The packages use Markdown instructions; tool availability, invocation flags and model behavior vary by harness. Connection and resource checks on Saad's Mac do not establish behavior on every external setup.

## Maintenance

Edit and verify these sources here. The private hub's `PUBLISHED_SKILLS` is Saad's runtime/distribution allowlist; the copy here must agree. On the originating Mac:

```sh
~/code/personal-infra/cadastre/cadastre skills check
~/code/personal-infra/cadastre/cadastre skills publish
```

The checks do not publish remotely. `publish --apply` skips these public-owned sources and preserves unexpected public packages with an error. Keep private paths, policy and personal context in private adapters.

Attribution and licence review remain required before a new release: this checkout has no root distribution licence. Preserve existing framework attribution and references; don't assume all imported material grants the same rights.
