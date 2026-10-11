# OpenSourceServices

Own-your-tools replacements for paid SaaS subscriptions. Each repo replaces
one recurring fee with a local, private, dependency-light alternative — the
blueprint: pay an LLM once to build the utility, run it free forever.

This repo is the index. The tools live in their own repos:

| Repo | Replaces | ~Annual fee avoided | Stack |
|---|---|---|---|
| [vibe-proofread](https://github.com/biofool/vibe-proofread) | Grammarly | $144 | Static HTML + any OpenAI-compatible LLM (LM Studio) |
| [vibe-dictate](https://github.com/biofool/vibe-dictate) | Wispr Flow | $144 | RealtimeSTT + faster-whisper, global hotkeys via DE shortcuts |
| [vibe-activity](https://github.com/biofool/vibe-activity) | Rize | $120 | ActivityWatch + local LLM + ECharts dashboard |

**Total: ~$408/yr in subscriptions → $0 recurring.**

Inspired by Dibakar Ghosh's article *"I canceled over $400 in yearly
subscriptions by spending $20 to vibe code my own alternatives."*

All repos are MIT-licensed. Everything runs locally — no data leaves the
machine unless you point a tool at a hosted LLM endpoint.

## Using the tools

There is nothing to install in this repo — it contains only this index and
a `wiki/` folder. Clone each tool repo and follow its own README. Quick
reference:

### vibe-proofread

Static page + `serve.sh`. No build, no dependencies.

```bash
git clone https://github.com/biofool/vibe-proofread
cd vibe-proofread && ./serve.sh          # open http://127.0.0.1:8000
```

Point it at any OpenAI-compatible endpoint (LM Studio at
`http://127.0.0.1:1234/v1` by default) in its Settings panel.

### vibe-dictate

Linux dictation daemon. Needs Python 3, `portaudio19-dev`, and desktop
shortcuts for hotkeys (Wayland cannot grab global keys from userland).

```bash
sudo apt install portaudio19-dev python3-dev
python3 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
./install-shortcuts.sh                   # GNOME only; other DEs: bind manually
python3 dictate_daemon.py                # then F9 push-to-talk, F10 live
```

### vibe-activity

Requires [ActivityWatch](https://activitywatch.net/) running for data
collection. Optional LM Studio endpoint improves categorization.

```bash
python3 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt          # tests only; runtime is stdlib-only
./run.sh [--days N] [--offline]          # open dist/index.html
```

## Deployment

None. These are local desktop tools, not services. Each runs under your
user account on your own machine. There is no server, container, or cloud
component in this index repo.

## Caveats

- **You are the ops team.** No vendor support. Each repo's README covers
  its own requirements.
- **Linux-first.** vibe-dictate targets Linux desktops (GNOME tested;
  KDE/Sway/Hyprland need manual key bindings).
- **LLM-dependent tools need an endpoint.** vibe-proofread and
  vibe-activity need an OpenAI-compatible server for full functionality.
  vibe-activity falls back to regex rules with `--offline`.
- **Hosted endpoints change the privacy story.** If you configure a
  hosted LLM instead of a local one, your text leaves the machine.

## Repo contents

```
README.md                  this file
wiki/                      mini wiki (Architecture, Development, Security)
.claude/hooks/             post-edit lint hook (agent tooling, not part of the tools)
```
