# Intuition Ideation Skill

A [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) skill that guides users through brainstorming, structuring, and publishing product ideas for the [Intuition Protocol](https://intuition.systems) ecosystem.

![Workflow Overview](assets/workflow-overview.png)

## What It Does

This skill supports **two modes** for non-technical community members:

### Mode A: Full Ideation (5 Steps)
Walk through the complete idea-to-publication pipeline:

1. **Describe & Search** — Capture the idea and search for similar concepts in **both** the Intuition knowledge graph (onchain) **and** GitHub
2. **Brainstorm & Draft** — Structure the idea into a standardized template through guided conversation
3. **Challenge** — Stress-test the idea from feasibility, market, protocol-fit, and UX angles
4. **Publish to GitHub** — Create a PR on the [intuition-box/ideas](https://github.com/intuition-box/ideas) repo
5. **Publish on Intuition** — Create an on-chain atom, link it to the curated ideas list, and stake on it

### Mode B: Smart Idea Discovery 🎲
Discover ideas from the **onchain Intuition ideas list** using three strategies:

- 🔥 **Popular** — the most backed ideas, sorted by $TRUST staked
- 💎 **Hidden Gems** — ideas with community support but no recent activity (staleness-weighted rediscovery)
- 📈 **Rising** — ideas gaining momentum, ranked by stakers-per-day growth rate
- 🎲 **Random** — pure surprise mode

All modes query the Intuition mainnet GraphQL API, compute momentum scores and activity age, and fall back to GitHub if onchain data is unavailable.

## What's New

### v2.2 — Configurable Intelligence (feedback from [@danielamodu](https://github.com/danielamodu))
- **🎛️ Configurable `dormancy_days`** — Hidden Gems threshold is now tunable (default: 14 days). Power users can set custom values for seasonal ideas (e.g., infrastructure ideas dormant 30+ days)
- **⏱️ Rising mode minimum age filter** — New `min_rising_age_days` parameter (default: 3 days) prevents brand-new ideas with 1 staker from falsely appearing as "rising"
- **🧠 Selection reasoning** — Each picked idea now includes a `SELECTION_REASON` explaining WHY it was chosen (e.g., "Gaining 2.45 stakers/day — fastest growing in the pool")
- **💬 Interactive threshold prompt** — When entering Hidden Gems mode, the skill proactively asks the user if they want to adjust the dormancy window
- **📊 Enhanced output** — Presentations now show `DORMANCY_THRESHOLD` and `MIN_RISING_AGE` for full transparency

### v2.1 — Smart Discovery Modes
- **🔥 Popular / 💎 Hidden Gems / 📈 Rising** — Three discovery strategies for the Random Idea Picker, each with tailored presentation
- **📊 Momentum scoring** — Computes stakers-per-day growth rate to identify rising ideas
- **⏰ Staleness-weighted rediscovery** — Surfaces ideas with community backing (positionCount > 0) but no activity for 14+ days
- **🔄 Mode switching** — Users can switch between discovery modes mid-session
- **📈 Activity metrics** — Each idea now shows days since last activity and momentum score

### v2.0 — Onchain Integration
- **🎲 Random Idea Picker** — Pull random ideas from the onchain knowledge graph for inspiration
- **🔍 Dual-state checking** — Search both GitHub AND onchain state before brainstorming to prevent duplicates
- **🔗 Onchain ideas list integration** — New ideas are automatically linked to the curated list via the `[Idea] → [top project ideas for] → [Intuition]` triple pattern
- **💰 Streamlined publish flow** — Pre-flight cost checks, batch triple creation, deposit previews, and clear error recovery
- **🛡️ Duplicate prevention** — Automatically detects existing atoms and reuses them instead of creating duplicates
- **📊 Cost transparency** — Shows exact $TRUST costs upfront before any transaction

## Installation

```bash
npx skills add intuition-box/intuition-ideation-skill
```

## Dependencies

| Dependency | Required For | Install |
|-----------|-------------|---------|
| [Intuition Protocol Skill](https://github.com/0xIntuition/agent-skills) | Step 5 (on-chain publishing) | `npx skills add 0xintuition/agent-skills --skill intuition` |
| [GitHub CLI](https://cli.github.com/) | Step 4 (GitHub PR) | `brew install gh` + `gh auth login` |

> Steps 1–4 work without the Intuition Protocol skill. The minimum viable path is Step 2 (draft) → Step 4 (GitHub publish).

## Usage

Once installed, the skill triggers automatically when you say things like:

- *"I have an idea for something that could use Intuition"*
- *"Let's brainstorm a product concept"*
- *"I want to submit an idea to intuition-box"*
- *"New idea for the protocol"*
- *"Random idea"* / *"Inspire me"* / *"What should I build?"* / *"Pick an idea for me"*
- *"Show me hidden gems"* / *"What's trending?"* / *"Find forgotten ideas"*

## Skill Structure

```
.claude/skills/intuition-ideation/
├── SKILL.md                              # Main skill definition (v2.1 — smart discovery modes + onchain integration)
├── assets/
│   └── workflow-overview.png             # Visual overview of the 5-step workflow
└── references/
    ├── intuition-protocol-skill.md       # Full Intuition Protocol context (loaded at bootstrap)
    ├── intuition-basics.md               # Plain-English protocol explainer
    ├── idea-template.md                  # Structured idea template
    └── github-submission-format.md       # GitHub PR format spec
```

## How It Works

The skill bootstraps by loading the full [Intuition Protocol skill](https://github.com/0xIntuition/agent-skills/blob/main/skills/intuition/SKILL.md) as context, giving it deep knowledge of atoms, triples, vaults, GraphQL queries, ABI fragments, and network configuration. This enables it to:

- **Search** the Intuition knowledge graph for similar ideas (Step 1) — now checks both onchain AND GitHub
- **Discover** existing onchain ideas via the Random Idea Picker (Mode B)
- **Suggest** realistic protocol integration patterns (Step 2)
- **Evaluate** feasibility against actual protocol capabilities (Step 3)
- **Publish** with streamlined pre-flight checks, batch triple creation, and cost previews (Step 5)
- **Link** new ideas to the curated onchain list for discoverability

All technical details are translated into plain English for non-technical users.

## Contributing

Ideas and improvements welcome! Open an issue or PR.

## License

MIT
