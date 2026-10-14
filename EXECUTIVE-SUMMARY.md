# OpenSourceServices — Executive Summary

OpenSourceServices is an index repo. It tracks a set of small open-source
tools that each replace one paid SaaS subscription with a local,
MIT-licensed alternative. The tools themselves live in their own repos:

| Repo | Replaces | ~Annual fee avoided |
|---|---|---|
| [vibe-proofread](https://github.com/biofool/vibe-proofread) | Grammarly | $144 |
| [vibe-dictate](https://github.com/biofool/vibe-dictate) | Wispr Flow | $144 |
| [vibe-activity](https://github.com/biofool/vibe-activity) | Rize | $120 |

**Total: ~$408/yr in subscriptions → $0 recurring.**

## What this project is for

The blueprint: pay an LLM once to build the utility, then run it free.
All three tools run locally. No data leaves the machine unless you
deliberately point a tool at a hosted LLM endpoint.

## Benefit to somatic studios and related fields

The direct benefit is cost. Studios run on thin margins; writing,
dictation, and time-tracking subscriptions are common overhead. These
tools remove roughly $400/yr per user of that overhead.

The indirect benefit is privacy and control. Studio notes, client
communications, and personal activity data stay on the local machine.
There are no accounts to manage, no telemetry, and no vendor that can
raise prices or shut down the service. The trade-off: you run and
maintain the tools yourself.
