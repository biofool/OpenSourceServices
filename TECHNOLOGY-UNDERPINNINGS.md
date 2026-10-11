# Technology Underpinnings — OpenSourceServices

This repo is an index, not a product. It contains a README, a mini wiki
(`wiki/`), and agent tooling (`.claude/`). The technical work lives in the
three tool repos it lists. This file summarizes how each is implemented.

## Common design pattern

All three tools follow the same model:

- **Local-first.** Data stays on the machine. No accounts, no telemetry.
- **Thin wrapper over open components.** Each tool is a small layer on
  top of existing open-source software (Whisper, ActivityWatch) or a
  local LLM endpoint.
- **No build systems.** Static HTML, shell scripts, and stdlib-heavy
  Python. No bundlers, no package-lock chains.
- **LLM as a swappable backend.** Where a language model is used, the
  endpoint is a runtime setting — LM Studio, Ollama, or any
  OpenAI-compatible API.

## vibe-proofread

- Single self-contained `index.html` — markup, CSS, and JavaScript in one
  file. No dependencies, no build step.
- `serve.sh` is a thin wrapper around a Python static file server
  (binds `0.0.0.0:8000`).
- Calls an OpenAI-compatible chat endpoint (default
  `http://127.0.0.1:1234/v1`, i.e. LM Studio) for edit suggestions.
- Edits are verified client-side: `parseEdits` drops any suggestion that
  is not a verbatim substring of the source text, which filters LLM
  hallucinations.
- Endpoint, model, and API key are runtime settings in `localStorage` —
  nothing is hardcoded.

## vibe-dictate

- Python daemon + client split:
  - `dictate_daemon.py` holds a faster-whisper model in memory (via
    RealtimeSTT) and listens on a Unix socket
    (`$XDG_STATE_HOME/vibe-dictate/ctl.sock`).
  - `dictate_ctl.py` is a thin client (`ptt` / `live` / `stop` / `status`
    / `quit`) that desktop shortcuts invoke.
- Hotkeys are delivered by the desktop environment, not by key grabbing —
  Wayland userland cannot grab global keys. `install-shortcuts.sh` binds
  F9/F10 as GNOME custom shortcuts; other DEs bind the same commands
  manually.
- Typing/clipboard helpers degrade gracefully:
  `wl-copy` → `xclip`, `wtype` → `ydotool` → `xdotool` → notify-only.
- Defaults are CPU-safe (`small.en`/`tiny.en`, `int8`); GPU paths are
  enabled by environment variables only.
- Transcripts are saved locally as timestamped `.wav` + `.txt` pairs.

## vibe-activity

- Three-stage pipeline (shell + Python, runtime is stdlib-only):
  1. `fetch_aw.py` pulls raw window/AFK events from a running
     [ActivityWatch](https://activitywatch.net/) server →
     `data/events_<date>.json`.
  2. `categorize.py` labels events — regex rules first, a local LLM for
     whatever the rules miss → `data/categorized_<date>.json`.
  3. `build_dashboard.py` renders a self-contained ECharts report →
     `dist/index.html`.
- `run.sh` orchestrates the pipeline (`--days N`, `--offline` to skip the
  LLM). Tests under `tests/` run with pytest; `requirements.txt` exists
  only for the test environment.

## This repo's own tooling

- `wiki/` — markdown mini wiki (Home, Architecture, Development,
  Security) for agent and human reference.
- `.claude/settings.json` + `.claude/hooks/post_edit_lint.py` — a
  PostToolUse hook that lints files after agent edits (ruff for Python,
  per-extension checks elsewhere; no-ops when a linter is absent). The
  canonical copy is maintained in `biofool/magic-compact-devin`.
