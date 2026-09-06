A Firecrawl-based automation system developed for a personal newsletter brand to source and verify research from Medium before it's cited in an issue. The project demonstrates web scraping automation, mixed-quality-source verification, and AI-assisted workflow execution using Claude Code.

## How it works

Each issue starts from a single topic or tag and moves through the same repeatable flow:

- **Search** — Medium is searched/mapped for the week's topic or tag, surfacing candidate articles
- **Filter** — candidates are skimmed for tone and quality; video-only and paywalled sources (Medium exclusions, plus sites like Psyche, Aeon, HBR) are dropped
- **Scrape** — the selected article(s) are scraped for full, clean text
- **Verify** — every specific statistic, study name, or named researcher pulled from the text is independently confirmed against the original source or a fresh search — never taken on the article's word
- **Classify** — each point is tagged **verified** (safe to cite), **softened** (rewritten without false precision), or **dropped** (couldn't be confirmed, discarded)

## How to use

No install step — this runs on [Firecrawl's](https://firecrawl.dev) MCP tools inside Claude Code, not a local Python environment.

Give Claude a topic and ask it to source research for the newsletter. It reads [workflows/source_and_verify_research.md](workflows/source_and_verify_research.md) and follows the search → filter → scrape → verify → classify flow automatically, rather than needing the process re-explained each time.

## Files in this repository include

**Workflow Documentation** — [workflows/source_and_verify_research.md](workflows/source_and_verify_research.md): the step-by-step SOP covering search, filtering, scraping, and the mandatory verification gate.

**Firecrawl Tool Reference** — [firecrawl-cheatsheet.md](firecrawl-cheatsheet.md): which Firecrawl tool to reach for (search vs. map vs. scrape vs. crawl), and which sources to skip entirely.

**Project Brief** — [CLAUDE.md](CLAUDE.md): what this project is for and the mandatory verification rule every claim has to clear before it's used.

**Framework Doc** — [WAT Claude.md](WAT%20Claude.md): the general Workflows/Agents/Tools framework this project runs on — plain-language SOPs, an agent that executes them, and deterministic tools underneath.

**Sourcing Dossier & Sample Output** — [results/medium_content_dossier.html](results/medium_content_dossier.html) / [.pdf](results/medium_content_dossier.pdf): a visual walkthrough of one real run — screenshots of the pages the workflow touches, Medium's colour/type system, a map of its site structure — plus [results/medium_top_performing_content.xlsx](results/medium_top_performing_content.xlsx), the actual 30-article sourcing-stage result, pending independent verification.
