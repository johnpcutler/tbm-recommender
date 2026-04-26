# TBM Brain

A searchable index of ~440 posts from John Cutler's [The Beautiful Mess](https://cutlefish.substack.com/) newsletter, packaged as an [open SKILL.md](https://agentskills.io/) for AI coding agents.

## What it does

When you ask your AI agent about product management, org design, strategy, prioritization, metrics, delivery, leadership, or systems thinking, this skill searches the TBM archive and returns the most relevant posts with links.

## Quick start

### Cursor

Clone this repo into your skills directory:

```bash
git clone https://github.com/johnpcutler/tbm-brain.git ~/.cursor/skills/tbm-brain
```

The skill activates automatically when you mention TBM, John Cutler, or related topics.

### Claude Code

```bash
git clone https://github.com/johnpcutler/tbm-brain.git ~/.claude/skills/tbm-brain
```

### Other agents

Any agent that supports the [SKILL.md format](https://agentskills.io/) can use this skill. Clone the repo into your agent's skills directory.

## How it works

The skill uses a three-tier retrieval flow to keep token usage low:

1. **Route** -- The agent reads a compact cluster index (~600 tokens) to identify which topic areas match your query.
2. **Scan** -- It loads only the matching cluster's posts and compares your intent against pre-extracted questions and search terms.
3. **Explore** -- Optionally follows a similarity graph to find related posts.

## What's included

```
SKILL.md              # Agent instructions (open SKILL.md format)
data/
  clusters.json       # 21 topic clusters (routing layer)
  clusters/*.jsonl    # Per-cluster post details (questions, search terms, summary)
  content-graph.json  # Post-to-post similarity neighbors
  version.json        # Build metadata
```

## Token cost per query

The tiered design keeps costs low. Approximate input tokens per query:

| Scenario | Tokens |
|----------|--------|
| Tier 1 only (routing) | ~1,600 |
| Tier 1 + 1 small cluster | ~3,300 |
| Tier 1 + 1 average cluster | ~8,800 |
| Tier 1 + 2 large clusters | ~29,000 |

A typical query hits 1-2 clusters and costs **3,000-15,000 input tokens**. The similarity graph (Tier 3) is only loaded when the user asks for related posts.

## Topics covered

Product management, discovery, experimentation, roadmaps, planning, prioritization, strategy, OKRs, metrics, WIP limits, dependencies, delivery, org design, operating models, change management, continuous improvement, leadership, psychological safety, communication, trust, frameworks, systems thinking, AI at work, and more.

## License

MIT
