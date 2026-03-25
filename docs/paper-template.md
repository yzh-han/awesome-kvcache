# Paper Entry Template

Use this template when adding a new paper.

## Minimal Metadata

- Title:
- Year:
- Venue:
- Link:
- Code:
- Primary category:
- Secondary tags:

## One-Line Summary

Write one sentence that answers:

`What KV-cache problem does this paper solve, and what is the main mechanism?`

Examples:

- Retains heavy-hitter tokens under a fixed KV budget.
- Quantizes keys and values differently to enable 2-bit KV caching.
- Reuses shared prefixes across requests to reduce prefill latency.

## Setting

Choose one or more:

- `prefill`
- `decode`
- `long-context`
- `streaming`
- `serving`
- `disaggregation`

## Gain

Write a short comparable phrase:

- `memory↓`
- `latency↓`
- `TTFT↓`
- `throughput↑`
- `quality retained under 5% KV budget`

## Placement Checklist

- Is KV cache central to the paper, not incidental?
- Is the primary category clear?
- Is the official paper link used?
- Is the code link official?
- Is the summary short and specific?

