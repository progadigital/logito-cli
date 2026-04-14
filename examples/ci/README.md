# Logito CI Integration

Gate your pull requests with runtime intelligence. Logito compares behavioral drift between your baseline and the current run, and fails the build when drift exceeds your configured threshold.

## Quick Start

1. Copy `github-actions.yml` to `.github/workflows/logito-ci.yml` in your repository
2. Set `LOGITO_API_KEY` as a repository secret
3. Customize the "Start app" and "Run tests" steps for your project
4. (Optional) Copy `logito-config.json` to `.logito/config.json` in your repo to customize thresholds

## Commands

| Command | Purpose |
|---------|---------|
| `logito ci check` | Evaluate the latest run — returns PASS, WARNING, or FAIL |
| `logito ci baseline` | Show the current comparison reference |
| `logito ci summary --run <id>` | Print the compact review block for a specific run |

## Thresholds

Configure in `.logito/config.json`:

```json
{
  "ci": {
    "drift_threshold_warn": 30,
    "drift_threshold_fail": 60
  }
}
```

- **PASS**: drift score below warn threshold
- **WARNING**: drift score between warn and fail thresholds (build succeeds)
- **FAIL**: drift score above fail threshold (build fails with exit code 1)

## Other CI Systems

The `logito ci check` command works in any CI system — it uses standard exit codes:
- Exit 0 = PASS or WARNING
- Exit 1 = FAIL

For non-GitHub CI systems, just install the CLI and run `logito ci check` after your tests.
