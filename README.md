# AI Search Readiness Grader

Paste a URL and get a 0–100 score for how ready the page is to be **found and cited by AI search engines** (ChatGPT, Perplexity, Google AI Overviews, Claude). It grades the structural signals that decide whether an LLM can find, parse, and quote your content — answer-first structure, self-contained sections, schema, freshness, crawlability, and more — and returns a concrete fix for every gap.

No build step, no framework, no server. One HTML file.

**[Live tool →](https://cpl-marketing.github.io/ai-readiness-grader/)**

Same design principle as its sibling, the [Account Scoring Console](https://github.com/cpl-marketing/account-scoring-console): **Claude only classifies; the score is deterministic in JavaScript.**

## How it works

1. Enter the URL you want AI engines to cite (optionally the query you want to rank for).
2. Claude fetches the live page with its web search tool and judges each signal as **pass / partial / fail**, with one line of evidence and one concrete fix.
3. Plain JavaScript maps pass/partial/fail to points, applies each signal's weight, and sums to a 0–100 score. The number never comes from the model, so it's identical for the same inputs and traceable to the checks.

It runs from a static page with no backend, so **Claude does the fetching** — a browser can't fetch arbitrary URLs (CORS). This means web search must be enabled on your Anthropic account.

## What it checks

| Signal | Weight | Why it matters for AI search |
|---|:--:|---|
| Answer-first structure | 3 | LLMs lift self-contained answers stated near the top |
| Self-contained sections | 3 | A retrieved snippet has to stand alone |
| Clear heading hierarchy | 2 | Question-shaped H2/H3s help retrieval chunk the page |
| Structured data (schema.org) | 2 | JSON-LD tells machines what the page is |
| Scannable formatting | 2 | Extractable facts beat walls of prose |
| Concrete examples / code | 2 | Specifics get quoted; generic claims don't |
| Freshness signals | 2 | Models favor content that looks maintained |
| Citations & authority | 2 | Named sources and specific claims signal trust |
| Crawlable to AI bots | 2 | HTML content and robots rules that don't block GPTBot/ClaudeBot |
| Title & meta clarity | 1 | Title/meta should match the page's actual answer |
| llms.txt present | 1 | Emerging, low-cost positive signal for AI crawlers |

## Scoring

```
points = pass → full · partial → half · fail → zero,  each × the signal's weight
score  = round(earned / max possible × 100)
```

Bands: **80–100 AI-ready**, **55–79 needs work**, **0–54 largely invisible** to AI search.

## Connect Claude (bring your own key)

The grade calls the Anthropic API directly from your browser, so it needs your own key, entered under "Connect Claude."

- The key is used only for calls from your browser to `api.anthropic.com`. It is not stored or transmitted anywhere else.
- **Never commit a key into the repo.** It goes in the running page at use time, not in the source.
- Web search must be enabled on your account (it's how Claude fetches the page).
- For anything public and production-grade, proxy the request through your own backend so the key never reaches the client.

## Caveats

It's a readiness heuristic, not a guarantee of citation. AI engines are opaque and change often; this grades the fundamentals that reliably help. Because web search grounds Claude on the live page, results can vary slightly between runs.

## Stack

Vanilla HTML, CSS, and JavaScript. State lives in the page for the session. Fonts load from Google Fonts; swap the link tag and font stack for fully offline.

## License

MIT.
