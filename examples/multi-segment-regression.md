# Example: Multi-Segment Regression Investigation

This example shows a more realistic failure case with multiple segments, one likely fault boundary, and an actionable next step.

## Scenario

A local payment workflow used to complete successfully. After a recent dependency or config change, checkout now fails intermittently.

The workflow includes:

- request acceptance
- fraud screening
- payment authorization
- inventory reservation
- order persistence

## Command

```bash
logito run flow.yaml
logito compare last
```

## Example Output

```text
$ logito compare last

Session: payments-local-2026-04-03T15:31:07Z
Status: analyzed
Baseline: payments-local-2026-04-02T19:12:54Z

Segments
  1. edge.request         stable      POST /checkout -> accepted
  2. fraud.screen         stable      score returned within baseline
  3. payment.authorize    degraded    retries increased
  4. inventory.reserve    stable      no abnormal drift
  5. order.persist        impacted    skipped after payment failure

Evaluation
  Severity: high
  Primary finding:
    payment authorization introduced retry-driven latency and caused downstream order completion failure

  Evidence:
    - payment.authorize retry count: 0 -> 4
    - payment.authorize p95 latency: 280ms -> 2.3s
    - final checkout response: 200 -> 502
    - inventory.reserve remained stable, reducing the likely fault boundary to payment authorization or its upstream dependency

Suggested action
  Inspect payment authorization timeout, retry, and dependency behavior first.
  The strongest fault boundary is the payment.authorize segment.
```

## What Makes This Useful

Logito is not only saying "checkout failed."

It is narrowing the problem:

- the session shows the full workflow
- the segment list shows where behavior first degraded
- the evaluation distinguishes the probable source segment from downstream impact
- the baseline comparison tells you this was not normal variance

## Practical Next Step

If you were debugging this for real, you would start with:

- timeout configuration
- retry policy changes
- upstream dependency behavior for payment authorization

That is a far better starting point than scanning the entire workflow from the top.
