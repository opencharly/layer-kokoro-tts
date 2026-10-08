# layer-kokoro-tts

Multi-voice offline text-to-speech. `layer-sherpa-onnx` bakes exactly ONE VITS voice; this candy adds the pinned **Kokoro-82M** ONNX model plus its multi-voice pack, driven by that same engine. Voice selection is per utterance via `--sid`.

## Status

Newly created (2026-10-08) to close a measured gap. See `charly.yml` for the candy
entity, its `plan:` acceptance checks and its skill entity.

## Verify

```bash
charly box validate          # the manifest parses and validates
```

The candy's own `check:` steps are the executable acceptance — they synthesise (or
render) real audio and assert the artifact, so they can fail.
