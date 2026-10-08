# AGENTS.md — layer-kokoro-tts

Standalone candy repo owning the `layer-kokoro-tts` candy (plus its `skill:` entity, projected into
the marketplace corpus), the `kokoro-tts-app` box that composes it, and the `check-kokoro-tts-pod`
disposable R10 bed. All of them live in `charly.yml` at the repo root.

Canonical files:

- `charly.yml` — the `kokoro-tts:` candy, the `kokoro-tts-app:` box, the
  `check-kokoro-tts-pod:` bed, and the `kokoro-tts-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema, plan-step verbs).
- `/charly-check:check` — the check/bed model and the check-verb catalog. Load before
  changing any `check:` step or the bed.
- `/charly-internals:git-workflow` — before any git/PR action.
- `/charly-tools:kokoro-tts` / `/charly-tools:podcast-audio` — the owning skills.

## Build / validate / test

- `charly box validate` — the structural pre-flight; it must report **0 warnings, 0 errors**.
- `charly check run check-kokoro-tts-pod` — **the R10 gate** for this repo: image build →
  check-image → deploy → check live → fresh rebuild → teardown, on a `disposable: true` pod.
  The candy's own `check:` steps run in BOTH contexts, so they execute at `charly check box
  kokoro-tts-app` (no deploy) and again, live, inside the deployed pod.
- The merge gate is the org-wide `charly/pr-validator` (required check `validate / validate`);
  this repo carries no per-repo candy gate.

## Modify this repo

- The model artifact is pinned TWICE, deliberately: `KOKORO_MODEL` is the fetch coordinate
  and `KOKORO_MODEL_SHA256` is the identity, enforced by the `kokoro-verify-artifact` plan
  step. Bump both together, or the build fails at the digest check — which is the point.
- **`run:` steps interpolate `${KOKORO_MODEL}`; `check:` steps do NOT.** A check step's fields
  resolve only the auto-exports (`${HOME}`, `${USER}`, `${ARCH}`); a candy `var:` is refused
  and the step is reported **SKIPPED** — `unresolved variables: KOKORO_MODEL` — which does not
  fail the run. Measured 2026-10-08: pointing the four `file:` probes and the two synthesis
  commands at `${KOKORO_MODEL}` silently disarmed all six acceptances while the bed still
  reported PASS. The literal model name in those steps is therefore deliberate: change
  `KOKORO_MODEL` and you must change them too.
- Edit the `kokoro-tts:` candy AND its `kokoro-tts-skill:` entity together: the skill is the
  projected usage source, so a version, voice-table or invocation change not mirrored there
  leaves the corpus stale.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and in the skill.

## Landing

- PR-only. No CHANGELOG file is staged in a PR: the PR body IS the changelog, and
  `tag-on-merge` writes `CHANGELOG/<merge-time CalVer>.md` from it at merge time. The
  authoritative rulebook is the umbrella `AGENTS.md` in `opencharly/opencharly`.
