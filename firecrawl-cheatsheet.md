# Firecrawl MCP Cheatsheet

Firecrawl MCP turns messy web pages into clean, structured text or data an agent can actually reason over. This doc covers each tool, what it's for, when to reach for it, and a short example. Per [WAT Claude.md](WAT%20Claude.md): check this list before writing any new scraping code — these tools cover most of what this project needs.

## `scrape`

**What it does:** Fetches one URL and returns its clean, readable content (markdown/text), stripped of nav bars, ads, and boilerplate.

**When to use it:** You already know the exact URL of the page you want — a single known article, not a search or a listing page.

**Example:**
```
scrape(url="https://medium.com/@someauthor/the-real-cost-of-busyness-1234abcd")
→ returns full article text in clean markdown
```

## `map`

**What it does:** Quickly enumerates the URLs found on a site or section — sitemap-style. Returns links only, no page content.

**When to use it:** You want to see what's available on a site or tag page before committing to scraping anything, and you don't need to follow multiple levels of links to find it.

**Example:**
```
map(url="https://medium.com/tag/decision-making")
→ returns a list of article URLs under that tag
```

## `crawl`

**What it does:** Traverses a site following internal links out to a depth/page limit you set, pulling content as it goes (essentially repeated `scrape` across discovered pages).

**When to use it:** `map` alone doesn't surface enough candidate URLs — e.g. a tag page that paginates, or a section whose article list isn't in one flat page. Keep `limit`/`maxDepth` low; this is for discovery, not a full site mirror.

**Example:**
```
crawl(url="https://medium.com/tag/psychology", limit=15, maxDepth=1)
→ returns content from up to 15 pages one hop from the tag page
```

## `search`

**What it does:** Runs a web search and returns matching URLs with snippets (optionally with page content attached).

**When to use it:** You don't already know which site has what you need — you're searching by topic across the open web, not within one known site.

**Example:**
```
search(query="productivity psychology research medium.com")
→ returns candidate article URLs and snippets
```

## `extract`

**What it does:** Pulls specific structured fields (you define a schema — e.g. `{title, author, publish_date, key_stat}`) out of one or more URLs, instead of returning raw page text.

**When to use it:** You need particular fields out of a page rather than the full text — e.g. batch-pulling author names and publish dates across several candidate articles to decide which to read in full.

**Example:**
```
extract(urls=["https://medium.com/@author/article-1"], schema={title: string, author: string, publish_date: string})
→ returns {title: "...", author: "...", publish_date: "..."}
```

## `batch_scrape`

**What it does:** Runs `scrape` across a list of known URLs in one call instead of one at a time.

**When to use it:** You've already identified several specific article URLs (via `map`, `crawl`, or `search`) and want the full clean text of all of them in one pass.

**Example:**
```
batch_scrape(urls=["https://medium.com/@a/article-1", "https://medium.com/@b/article-2"])
→ returns full clean text for each URL
```

---

## Research sourcing for Soft Strategy Weekly

This is the workflow this project actually runs every week. Use the tools above in this division of labor:

- **`scrape` is for pulling one known article's full clean text**, once we've already identified it as worth reading in full.
- **`map` or `crawl` (limited) is for finding relevant article URLs** on a site or tag page *before* scraping them individually — this is a discovery step, not a content step. Keep crawl depth/page limits small; the goal is a short list of candidate URLs, not a mirror of the site.

**Primary source: Medium (medium.com).** Search by topic/tag relevant to the newsletter's beat — `productivity`, `decision-making`, `psychology`, `money-mindset`, and similar tags — looking for well-written, accessible pieces that match Soft Strategy Weekly's tone. Medium is the default hunting ground because it reliably surfaces personal-essay-style writing on these topics, which is the register this newsletter writes in.

**Do not use as scrape targets:**
- **Video-only sources** — Firecrawl can't extract usable text from embedded video, so a page that's mostly a video player returns nothing worth using.
- **Fully paywalled sources** (e.g. Psyche/Aeon, HBR) — a paywall means the scrape returns a teaser/excerpt at best, not the full article text, so these aren't reliable scrape targets even when the topic fits.

See [CLAUDE.md](CLAUDE.md) for the mandatory verification step that applies to anything sourced this way before it goes in the newsletter.
