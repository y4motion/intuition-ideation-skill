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

### Mode B: Random Idea Picker 🎲
Start with a random idea pulled from the **onchain Intuition ideas list** and refine it:

- Queries the Intuition mainnet GraphQL API for ideas published as atoms
- Shows community backing (stakers, $TRUST staked)
- Jumps directly to Step 2 for brainstorming refinement
- Falls back to GitHub ideas repo if onchain list is empty

## What's New (v2)

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

## Skill Structure

```
.claude/skills/intuition-ideation/
├── SKILL.md                              # Main skill definition (v2 — with random picker & onchain integration)
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
