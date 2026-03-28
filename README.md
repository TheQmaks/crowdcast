# Crowdcast

**Multi-agent social simulation as a Claude Code skill.**

Drop in a document, describe what you want to predict — Crowdcast spawns dozens of AI agents that argue, post, react, and evolve on a simulated platform. Then it writes you a report.

No APIs to configure. No databases to run. No web server. Just `/crowdcast simulate`.

## Why Crowdcast over MiroFish?

[MiroFish](https://github.com/666ghj/MiroFish) is the original multi-agent prediction engine that inspired this project. It's a great tool — but it comes with friction:

| | MiroFish | Crowdcast |
|---|---------|-----------|
| **Setup** | Python + Node.js + Docker + env vars | `git clone` into `~/.claude/skills/` |
| **External APIs** | Zep Cloud ($25/mo) + LLM API ($5-15/run) | None — included in Claude subscription |
| **Rate limits** | Zep free tier: 1 simulation/month | Unlimited |
| **Infrastructure** | Flask server + Vue.js frontend + Zep + OASIS | Zero — runs inside Claude Code |
| **Resume after crash** | Manual — restart from scratch | `/crowdcast resume` picks up where it stopped |
| **Agent scale** | Up to millions (via OASIS) | Up to ~100 (practical limit) |
| **Agent depth** | Uniform — all agents use same LLM prompt | Hybrid — key agents think deeply, crowd is batched |
| **Creative mode** | Limited (primarily forecast-oriented) | Full narrative simulation with character arcs |
| **Interview agents** | Requires running server | `/crowdcast interview` — instant, in terminal |

**TL;DR:** MiroFish is more powerful at massive scale. Crowdcast is simpler, cheaper, and more accessible for most use cases. If you need to simulate a million agents, use MiroFish. If you need a quick prediction from 50 agents with zero setup, use Crowdcast.

## What It Does

Upload a document (news article, research report, novel) and describe what you want to simulate:

1. **Analyze** — extract entities, relationships, and context from your documents
2. **Profile** — create AI agents with distinct personalities, stances, and behaviors
3. **Simulate** — run rounds of agent interaction on a simulated platform
4. **Report** — produce an analytical prediction or narrative retelling

## Two Modes

| | Forecast | Creative |
|---|---------|----------|
| **Input** | News, reports, policy docs | Fiction, scripts, scenarios |
| **Agents** | Stakeholders + social groups | Characters + background |
| **Simulation** | Social media (posts, comments, likes) | Free-form world (dialogue, actions, events) |
| **Output** | Analytical prediction with trends | Narrative retelling with character arcs |

Mode is auto-detected from your document, or set explicitly with `--mode=forecast` / `--mode=creative`.

## Installation

**Option A: Clone directly into skills (simplest)**

```bash
git clone https://github.com/TheQmaks/crowdcast.git ~/.claude/skills/crowdcast
```

**Option B: Symlink (keep repo separate)**

```bash
git clone https://github.com/TheQmaks/crowdcast.git
ln -s $(pwd)/crowdcast ~/.claude/skills/crowdcast
```

**Option C: Install as Claude Code plugin**

```bash
# If you have a marketplace that includes crowdcast:
/plugin install crowdcast@<marketplace-name>
```

That's it. No `npm install`, no `pip install`, no Docker, no `.env` files.

Verify installation — type `/crowdcast` in Claude Code and you should see the help menu.

## Usage

```bash
# Full simulation — forecast mode
/crowdcast simulate ./news_report.pdf "How will the public react to this policy change?"

# Full simulation — creative mode
/crowdcast simulate ./chapter1.txt ./chapter2.txt "Continue the story with these characters"

# Analyze documents only (no simulation)
/crowdcast analyze ./report.pdf

# Resume an interrupted simulation
/crowdcast resume sim_a3f8b2c91d04

# Regenerate report from completed simulation
/crowdcast report sim_a3f8b2c91d04

# Interview a simulated agent in character
/crowdcast interview sim_a3f8b2c91d04 mayor_chen
```

## How It Works

```
Document → [Analyzer] → Knowledge Graph → [Profiler] → Agent Personas
                                                            ↓
Report ← [Reporter] ← Round Logs ← [Simulator] ← Personas + Config
```

- **Claude is everything** — the LLM, the NER engine, the simulator, and the report writer
- **Subagent orchestration** — each phase runs as an isolated Claude Code subagent with clean context
- **File-based state** — all data stored as JSON in `.crowdcast/simulations/`
- **Resumable** — every phase saves progress; interrupted simulations continue from the last checkpoint
- **Hybrid depth** — key agents (leaders, influencers) think individually; crowd agents are batched for efficiency

## Typical Scale

| Parameter | Forecast | Creative |
|-----------|----------|----------|
| Agents | 50+ (8-10 key + crowd groups) | 5-20 characters |
| Rounds | 50-100 | 30-50 |
| Wall time | 20-40 min | 15-30 min |
| Cost | Included in Claude subscription | Included in Claude subscription |

## Requirements

- [Claude Code](https://claude.ai/claude-code) (CLI, desktop app, or IDE extension)
- Claude subscription (Pro, Team, or Enterprise)

## Project Structure

```
SKILL.md                              # Main orchestrator — loaded when you type /crowdcast
references/
  data-schemas.md                     # All JSON schema definitions
  phase1-analyzer.md                  # Document analysis subagent prompt
  phase2-profiler.md                  # Persona generation subagent prompt
  phase3-simulator-forecast.md        # Forecast simulation subagent prompt
  phase3-simulator-creative.md        # Creative simulation subagent prompt
  phase4-reporter.md                  # Report generation subagent prompt
test_seeds/
  sample_news.md                      # Sample document for testing
```

## Inspired By

[MiroFish](https://github.com/666ghj/MiroFish) by the MiroFish team at Shanda Group, powered by [OASIS](https://github.com/camel-ai/oasis). Crowdcast reimagines the concept as a zero-dependency Claude Code skill.

## License

MIT
