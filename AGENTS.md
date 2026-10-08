# AGENTS.md — layer-kokoro-tts

Standalone candy repo owning the `layer-kokoro-tts` candy (plus its `skill:` entity, projected into
the marketplace corpus). The candy entity lives in `charly.yml` at the repo root.

## Load these skills first (R0)

- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema, plan-step verbs).
- `/charly-internals:git-workflow` — before any git/PR action.
- `/charly-tools:kokoro-tts` / `/charly-tools:podcast-audio` — the owning skills.

## Build / validate / test

- `charly box validate` — the structural check.
- The merge gate is the org-wide `charly/pr-validator` (required check `validate / validate`);
  this repo carries no per-repo candy gate.

## Landing

PR-only; `tag-on-merge` writes `CHANGELOG/<CalVer>.md` from the PR body. The authoritative
rulebook is the umbrella `AGENTS.md` in `opencharly/opencharly`.
