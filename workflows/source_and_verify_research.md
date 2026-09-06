# Workflow: Source & Verify Research for Soft Strategy Weekly

## Objective

Given this week's belief or behavioral pattern for Soft Strategy Weekly, find well-written, on-tone source material and produce a short list of verified, citable points (or safely-softened points) ready to hand to the writing step.

## Required inputs

- This week's topic/angle (the belief or pattern the issue is built around), and 1–3 relevant tags to search (e.g. `productivity`, `decision-making`, `psychology`, `money-mindset`).

## Tools to use

See [firecrawl-cheatsheet.md](../firecrawl-cheatsheet.md) for full tool details. In order:

1. **`map` or `search`** on Medium for the week's tag(s) to collect candidate article URLs. Use `crawl` (small limit/depth) only if `map` doesn't surface enough candidates from a single tag page.
2. Skim candidates and pick the ones that are well-written, accessible, and on-tone — discard anything video-only or paywalled (Psyche/Aeon, HBR, etc.) per the cheatsheet's exclusions.
3. **`scrape`** (or `batch_scrape` if multiple) the selected article(s) for full clean text.
4. Pull out any specific statistic, study name, or named researcher mentioned in the scraped text.
5. **Verify every one of those specifics** — see [CLAUDE.md](../CLAUDE.md#mandatory-verification-step) for the exact procedure. This step is mandatory, not optional.
6. For each claim: keep it (verified), drop it, or soften it to "research suggests..." framing (unverifiable but the general point still holds).

## Expected output

A short list of source points for the issue, each tagged as one of:
- **Verified** — stat/study/researcher confirmed, safe to cite by name.
- **Softened** — specific claim couldn't be verified, rewritten without false precision.
- **Dropped** — noted as considered and rejected, so it isn't re-surfaced next week.

## Edge cases / notes

- If a Medium tag page returns mostly low-quality or off-tone results, try an adjacent tag before resorting to a deeper `crawl`.
- If verification turns up a claim that's outright wrong (not just unconfirmed but contradicted), drop it — don't soften a false claim into a vague one, drop it entirely.
- Log any recurring Medium authors/publications that reliably cite research well — useful for narrowing future searches (update this workflow if a pattern emerges).
