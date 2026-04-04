# Logito CLI

Logito helps developers understand runtime behavior as a sequence of sessions and segments, not as isolated log lines.

It captures what happened, highlights what changed, and points to the most likely next action when behavior drifts from a trusted baseline.

This public repository is the developer entry point for installation, onboarding, examples, and concepts. It does not expose the private runtime analysis engine or internal implementation logic.

## Why Logito

Modern systems do not usually fail because one line in one log file looks strange. They fail because behavior changes across requests, jobs, tool calls, retries, and dependent services.

Logito is built for that problem.

- It organizes runtime activity into sessions so you can inspect a full workflow instead of chasing timestamps by hand.
- It breaks each session into segments so you can see where behavior changed, slowed down, retried, or failed.
- It evaluates the current behavior against a baseline so you can tell the difference between normal variance and a real regression.
- It gives you high-signal findings and suggested next actions instead of dumping raw logs back at you.

## Install

macOS:

```bash
brew install progadigital/tap/logito
```

Windows:

```powershell
winget install progadigital.logito
```

More detail:

- [macOS install](./install/mac.md)
- [Windows install](./install/windows.md)

## 60-Second Quickstart

For live local capture:

```bash
logito dev start
logito review
```

For a reproducible scripted flow:

```bash
logito run flow.yaml
logito compare last
```

What happens:

1. `logito dev start` attaches to the active local workflow and captures runtime behavior as a session.
2. Logito groups the observed execution into segments such as request handling, downstream calls, retries, and completion paths.
3. `logito review` prints the authoritative summary for the latest captured session.
4. For scripted scenarios, `logito run flow.yaml` executes the defined workflow and `logito compare last` evaluates the latest session against the most relevant baseline.

Expected result:

- one session recorded
- ordered segments identified
- at least one evaluation result
- a concise action-oriented summary

## Example Output

```text
$ logito review

Session: checkout-local-2026-04-03T15:18:42Z
Status: analyzed
Baseline: checkout-local-2026-04-02T18:04:11Z

Segments
  1. edge.request        stable      GET /cart -> 200
  2. checkout.submit     changed     POST /checkout -> 502
  3. payments.charge     degraded    retry count 0 -> 3
  4. inventory.reserve   stable      latency within baseline

Evaluation
  Severity: high
  Primary finding:
    payment authorization retries increased and the final checkout response failed

  Evidence:
    - checkout.submit error rate: 0% -> 18%
    - payments.charge p95 latency: 240ms -> 1.9s
    - payments.charge retry pattern: absent -> present in 7 of 9 attempts
    - inventory.reserve remained within baseline, reducing the likely fault domain

Suggested action
  Inspect the payment segment first.
  Compare timeout, retry, and upstream dependency changes since the current baseline.
```

That is the core promise of Logito: a runtime session, broken into meaningful segments, evaluated against known behavior, with a concrete next move.

## Core Concepts

| Concept | Meaning |
| --- | --- |
| Session | A full runtime workflow execution. One user flow, job, script run, or bounded operational event. |
| Segment | An atomic execution unit inside the session. A request, downstream call, tool invocation, retry loop, or other meaningful step. |
| Evaluation | The deterministic assessment of the observed behavior. It scores changes, regressions, anomalies, and stability. |
| Baseline | The trusted prior behavior used for comparison. It answers: "Is this new, expected, or regressing?" |

See also:

- [Getting started](./docs/getting-started.md)
- [Concepts](./docs/concepts.md)

## Use Cases

- Debugging production issues by isolating the session and the segment where behavior diverged.
- Understanding LLM or agent workflows by seeing each runtime step as a segment instead of a flat transcript or log stream.
- Detecting regressions after a deploy, config change, dependency shift, or prompt change.
- Identifying performance drift such as retry growth, queueing, latency expansion, or partial-failure patterns.

## High-Level Architecture

Logito has three public-facing layers:

- CLI: the developer entry point for capture, analysis, and local workflow inspection.
- Runtime capture: the layer that observes workflow activity and organizes it into sessions and segments.
- Analysis engine: the private evaluation layer that compares current behavior to baseline behavior and returns structured findings.

What is public here:

- installation
- onboarding
- examples
- concepts

What is intentionally not public here:

- internal capture heuristics
- private correlation logic
- proprietary analysis implementation

## Repository Guide

```text
public/logito-cli/
  README.md
  docs/
    getting-started.md
    concepts.md
  examples/
    README.md
    basic-multi-request-app.md
    multi-segment-regression.md
  install/
    mac.md
    windows.md
```

## Examples

- [Basic multi-request app](./examples/basic-multi-request-app.md)
- [Multi-segment regression investigation](./examples/multi-segment-regression.md)

## Five-Minute Path

If you want the shortest path to first value:

1. Install the CLI for your platform.
2. Run `logito dev start`.
3. Run `logito review`.
4. Read the session summary and inspect the flagged segment first.

If that sounds like the right workflow, start with [Getting started](./docs/getting-started.md).
