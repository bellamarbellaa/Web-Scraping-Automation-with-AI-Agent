# Sourcing & Verifying Research from Medium

A small AI-agent workflow for finding well-written, on-topic source material on Medium, pulling out the ideas and evidence worth citing, and verifying every specific claim before it's used — built for a weekly newsletter's research process, but the pattern generalizes to any content-from-Medium sourcing task.

## Why this exists

Medium is a mixed-quality platform: some writers cite real research properly, others fabricate or misattribute statistics for engagement. Scraping it for source material is only useful if there's a hard verification gate before anything gets quoted or cited downstream.

## How it works

This follows the **WAT framework** (Workflows, Agents, Tools) — see [WAT Claude.md](WAT%20Claude.md):

- **Workflows** are plain-language SOPs the agent reads before acting (this repo: [workflows/source_and_verify_research.md](workflows/source_and_verify_research.md))
- **Agents** (the LLM) read the workflow, run the right tools in order, and make judgment calls
- **Tools** here are [Firecrawl](https://firecrawl.dev)'s MCP tools for search, mapping, and scraping — see the full breakdown in [firecrawl-cheatsheet.md](firecrawl-cheatsheet.md)

[CLAUDE.md](CLAUDE.md) is the project brief the agent reads on every run: what this project is for, which sources to avoid (paywalled sites, video-only pages), and the mandatory verification step.

## The verification gate

Any specific statistic, study name, or named researcher pulled from a Medium article must be independently confirmed before use — either by web-searching the study/author to confirm it says what the article claims, or by tracing back to the original source directly.

If a claim can't be verified, it's dropped or rewritten without the false precision (e.g. "a 2019 Stanford study found X" → "research suggests X" — only when the softer version is still something the article can stand behind).

## Files

| File | Purpose |
|---|---|
| [workflows/source_and_verify_research.md](workflows/source_and_verify_research.md) | The step-by-step SOP: search → filter → scrape → verify → classify |
| [firecrawl-cheatsheet.md](firecrawl-cheatsheet.md) | Reference for which Firecrawl tool to use for discovery vs. scraping, and which sources to skip |
| [CLAUDE.md](CLAUDE.md) | Project brief and the mandatory verification rule |
| [WAT Claude.md](WAT%20Claude.md) | The general Workflows/Agents/Tools framework this project runs on |
