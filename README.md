# OpenSourceServices

Own-your-tools replacements for paid SaaS subscriptions. Each repo replaces
one recurring fee with a local, private, dependency-light alternative — the
blueprint: pay an LLM once to build the utility, run it free forever.

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
