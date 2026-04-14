# Getting Started

This guide gets you from install to a useful answer fast.

## 1. Install

macOS and Linux:

```bash
brew tap progadigital/tap
brew install logito
```

Windows:

```powershell
winget install --id ProgaDigital.Logito
```

## 2. Start capture

```bash
logito dev start
```

Logito waits for real activity in your system and captures the run.

## 3. Trigger activity

Make a change, hit an endpoint, run a test, or reproduce the issue you care about.

## 4. Review what changed

```bash
logito review
```

If you want a live summary during capture:

```bash
logito ask "what changed?"
```

## 5. Read the answer

Logito is optimized to return:

- `WHAT CHANGED`
- `WHAT TO DO NEXT`

That means you should be able to answer:

- what changed in the run
- whether behavior drifted from baseline
- what to inspect or fix first

## First Practical Workflow

```bash
logito dev start
# reproduce the issue
logito review
```

Optional:

```bash
logito ask "what changed?"
logito review --json
```

## Next Reading

- [Concepts](./concepts.md)
- [Basic multi-request app example](../examples/basic-multi-request-app.md)
- [Multi-segment regression example](../examples/multi-segment-regression.md)
