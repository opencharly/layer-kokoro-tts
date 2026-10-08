# layer-kokoro-tts

Multi-voice offline text-to-speech. `layer-sherpa-onnx` bakes exactly ONE VITS voice; this candy adds the pinned **Kokoro-82M** ONNX model plus its multi-voice pack, driven by that same engine. Voice selection is per utterance via `--sid`.

## Status

Newly created (2026-10-08) to close a measured gap. See `charly.yml` for the candy
entity, its `plan:` acceptance checks, the `kokoro-tts-app` box and the
`check-kokoro-tts-pod` R10 bed.

## Verify

```bash
charly box validate                    # the manifest parses and validates: 0 warnings, 0 errors
charly check box kokoro-tts-app        # the candy's checks against the built image, no deploy
charly check run check-kokoro-tts-pod  # the R10 gate: build -> check -> deploy -> live -> rebuild -> teardown
```

The candy's own `check:` steps are the executable acceptance — they synthesise real
audio with two speaker ids and assert the artifacts differ, so they can fail. They run
in both the build and runtime contexts, so the disposable pod bed re-proves the same
synthesis live inside a running candybox.

## The pinned artifact

The model is pinned by **sha256**, not by its release tag:
`912804855a04745fa77a30be545b3f9a5d15c4d66db00b88cbcd4921df605ac7`
(319,625,534 bytes). The `kokoro-verify-artifact` plan step enforces the digest before
extracting, so a retagged or tampered upstream asset fails the build.
