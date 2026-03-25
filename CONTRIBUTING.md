# Contributing to Awesome-KVCache

## What to Add

We welcome:

- Papers
- Open-source systems and libraries
- Benchmarks and evaluation resources
- High-quality design docs or technical blogs from official project teams

## Inclusion Rule

Add an item only if KV cache is part of the **main contribution**.

Good fits:

- KV compression
- KV quantization
- KV eviction
- Prefix reuse
- Cache-aware scheduling
- KV offloading
- KV-aware serving runtimes

Usually not a good fit:

- Generic long-context papers without KV-specific contributions
- General LLM quantization papers with no KV-cache section
- Product announcements with no technical detail

## Formatting Rules

Use the same schema as `README.md` and `data/papers.csv`.

- `Year`
- `Paper`
- `Venue`
- `Core Idea`
- `Setting`
- `Gain`
- `Code`

Keep summaries short:

- Good: `Uses sink tokens and a local window for stable streaming inference.`
- Bad: `This paper proposes a novel and elegant framework that substantially improves multiple aspects of long-context reasoning.`

## Category Rules

- Place each paper in one primary category.
- Add secondary tags in `data/papers.csv` if needed.
- If a paper spans multiple areas, classify it by the source of its main contribution.

## Preferred Sources

Prefer official links:

- arXiv
- conference proceedings
- OpenReview
- official project docs
- official GitHub repositories

## Pull Request Checklist

- Added the item to the right category in `README.md`
- Added or updated the structured row in `data/papers.csv` or `data/systems.csv`
- Used official links where possible
- Kept the summary to one sentence
- Avoided duplicate entries

