# Getting Started

This guide is designed to get a developer from install to first useful runtime summary in under five minutes.

## 1. Install the CLI

macOS:

```bash
brew install progadigital/tap/logito
```

Windows:

```powershell
winget install progadigital.logito
```

## 2. Start local capture

For exploratory local debugging:

```bash
logito dev start
```

This starts capture against the active local workflow and records a bounded runtime session.

What Logito is looking for:

- workflow start and finish
- meaningful execution boundaries
- retries, latency shifts, failures, and downstream calls
- enough evidence to compare current behavior against baseline behavior

## 3. Read the latest session

For the latest captured local session:

```bash
logito review
```

For a scripted scenario you want to rerun and compare deterministically:

```bash
logito run flow.yaml
logito compare last
```

You should expect:

- a named session
- a segment list
- an evaluation result
- a suggested next action

## 4. Read the output in this order

1. Session
   Confirm you are looking at the right execution.
2. Segments
   Find the step marked as changed, degraded, or failed.
3. Evaluation
   Check severity, primary finding, and supporting evidence.
4. Suggested action
   Start investigation from the segment Logito names first.

## 5. What success looks like

The first successful run should answer questions like:

- Which segment changed?
- Was the change a failure, slowdown, retry spike, or logic drift?
- What baseline was used?
- Where should I look next?

## First Practical Workflow

For a local service:

```bash
logito dev start
logito review
```

For repeated scripted debugging:

```bash
logito run flow.yaml
logito compare last
# make one fix
logito run flow.yaml
logito compare last
```

That second pass is where baselines start paying off. Instead of rereading raw output, you can ask whether the changed segment is actually back within expected behavior.

## What Logito Is Not Doing

This public repo does not expose internal engine logic.

You should think of the public interface like this:

- capture a runtime workflow
- break it into meaningful segments
- evaluate it against baseline behavior
- return actionable findings

## Next Reading

- [Concepts](./concepts.md)
- [Basic multi-request app example](../examples/basic-multi-request-app.md)
- [Multi-segment regression example](../examples/multi-segment-regression.md)
