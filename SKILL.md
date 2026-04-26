---
name: tbm-brain
description: >-
  Search and explore John Cutler's "The Beautiful Mess" (TBM) newsletter
  archive (~440 posts). Covers product management, org design, strategy,
  prioritization, delivery, metrics, leadership, and systems thinking.
  Use when the user mentions TBM, John Cutler, "Beautiful Mess," or asks
  about product strategy, roadmaps, OKRs, discovery, team health,
  prioritization, operating models, or organizational change.
license: MIT
compatibility:
  - Claude Code
  - Cursor
  - Codex
  - Gemini CLI
metadata:
  author: John Cutler
  version: "1.0"
  posts: 439
  date_range: "2020-2026"
---

# TBM Brain

Searchable index of ~440 posts from "The Beautiful Mess" newsletter
(2020-2026). Three-tier retrieval keeps token usage low.

## Retrieval flow

### Tier 1: Route via clusters

Read [data/clusters.json](data/clusters.json) (~600 tokens).
Each entry has `id`, `label`, and `post_ids`.
Match the user's query to 1-3 clusters by label.

Cluster quick reference:

| ID | Label |
|----|-------|
| 4 | product management, discovery, experimentation |
| 16 | product ops, org effectiveness, agile delivery |
| 14 | product operating model, org transformation, SVPG |
| 1 | roadmaps, planning, convergence, playbooks |
| 10 | org design, leadership, accountability |
| 2 | communication, psychological safety, trust |
| 5 | WIP limits, dependencies, capacity, flow |
| 3 | organizational behavior, systems thinking |
| 7 | change management, continuous improvement |
| 19 | strategy execution, enabling constraints |
| 17 | metrics, measurement, analytics |
| 11 | prioritization, strategy, sequencing |
| 15 | strategy, alignment, decisiveness |
| 20 | overthinking, simplification, recovery |
| 8 | problem framing, decision making, assumptions |
| 12 | goals, OKRs, north star metrics |
| 0 | operating models, shared language |
| 22 | frameworks, mental models |
| 9 | north star, workshops, facilitation |
| 18 | AI, context trap, glue work, overload |
| 6 | narrative, complexity, change framing |

### Tier 2: Scan cluster detail

Read `data/clusters/<id>.jsonl` for each matched cluster.
Each line is one post with `title`, `date`, `url`, `questions`,
`search_terms`, and `summary`.

Match the user's intent against `questions` and `search_terms`.
Return the best 3-5 posts.

### Tier 3: Related posts

If the user wants to explore further, read
[data/content-graph.json](data/content-graph.json).
Look up the post ID to find nearest neighbors (ranked by similarity
score). Load neighbors from their cluster files as needed.

## Response format

- Lead with the most relevant post and a one-sentence reason it matches.
- Always include the Substack URL.
- Group multiple results logically, not by date.
- Search multiple clusters when the query spans topics.
- Say so if nothing matches well; do not force weak results.

Example:

> **TBM 399: 10 Prioritization Traps**
> https://cutlefish.substack.com/p/tbm-399-10-prioritization-traps
> Covers the most common anti-patterns teams hit when prioritizing.
>
> **Related:** TBM 381, TBM 374

## Freshness

Read [data/version.json](data/version.json) for `built_at` and
`post_count`. If the data is more than 30 days old, mention the
index may not include the latest posts.
