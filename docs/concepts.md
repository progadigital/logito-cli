# Concepts

Logito is easiest to use when you think in terms of behavior, not only logs.

## Session

A session is one full workflow execution.

Examples:

- one checkout attempt
- one background reconciliation job
- one agent task
- one scripted evaluation run

The session is the outer boundary for understanding what happened.

## Segment

A segment is one atomic execution unit inside a session.

Examples:

- an inbound request
- a downstream API call
- a retry loop
- a queue publish
- an LLM tool invocation

Segments matter because they turn a large runtime flow into actionable units. In practice, the segment is usually where you find the first concrete change worth investigating.

## Evaluation

An evaluation is Logito's structured assessment of the observed session and its segments.

It answers questions like:

- Did behavior materially change?
- Which segment changed first?
- Was the change a failure, slowdown, retry spike, or other drift?
- How severe is the difference?

An evaluation is not a generic summary. It is a decision-oriented output backed by runtime evidence.

## Baseline

A baseline is the trusted prior behavior used for comparison.

Without a baseline, you can still inspect a session.

With a baseline, you can answer:

- Is this normal?
- Is this new?
- Is this a regression?

Good baselines make runtime analysis faster because they remove guesswork from the comparison step.

## How They Fit Together

```text
Session
  -> Segment 1
  -> Segment 2
  -> Segment 3
  -> Evaluation against baseline
```

This is the operating model:

- sessions give you the full workflow
- segments give you the actionable units
- evaluations tell you what changed
- baselines tell you whether the change matters

## Practical Reading Order

When using Logito in a real incident or regression:

1. confirm the session
2. scan the segments
3. find the first changed or degraded segment
4. read the evaluation
5. use the baseline comparison to decide whether the behavior is expected or regressing

## Why This Model Matters

Flat logs answer "what was emitted?"

Logito is designed to answer:

- what happened in this workflow?
- where did it change?
- what is the most likely fault boundary?
- what should I inspect next?
