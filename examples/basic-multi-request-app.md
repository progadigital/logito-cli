# Example: Basic Multi-Request App

This example shows a normal local workflow with multiple requests inside one session.

## Scenario

You are running a local checkout service and want to understand one full workflow instead of reading separate request logs.

The workflow includes:

- loading the cart
- validating inventory
- submitting checkout
- confirming the order

## Command

```bash
logito run ./checkout-service
logito analyze
```

## What Logito Sees

One session with several segments:

```text
Session: checkout-local-2026-04-03T14:02:18Z

Segments
  1. edge.request        GET /cart
  2. inventory.check     POST /inventory/check
  3. checkout.submit     POST /checkout
  4. order.confirm       POST /orders/confirm
```

## Example Output

```text
$ logito analyze

Session: checkout-local-2026-04-03T14:02:18Z
Status: analyzed
Baseline: checkout-local-2026-04-02T17:44:02Z

Segments
  1. edge.request        stable
  2. inventory.check     stable
  3. checkout.submit     stable
  4. order.confirm       stable

Evaluation
  Severity: low
  Primary finding:
    no material runtime drift detected

  Evidence:
    - all segments completed within expected latency bounds
    - no new retries observed
    - no error-path segment introduced

Suggested action
  None required. This session matched the current baseline.
```

## Why This Matters

Even when nothing is wrong, Logito still adds value:

- it shows the full workflow as one session
- it keeps the segment boundaries explicit
- it confirms that current behavior still matches baseline behavior

That makes "nothing changed" a useful result instead of an assumption.
