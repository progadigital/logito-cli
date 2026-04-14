# Install on macOS

## Install

```bash
brew tap progadigital/tap
brew install logito
```

## Verify

```bash
logito --version
```

## First Run

```bash
logito dev start
# make a change, hit an endpoint, or run a test
logito review
```

## Troubleshooting

### `brew` is not installed

Install Homebrew first, then rerun the command above.

### Formula not found

Refresh your brew metadata and try again:

```bash
brew update
brew tap progadigital/tap
brew install logito
```

### Old version still appears

Upgrade explicitly:

```bash
brew upgrade logito
```

### `logito` is not on PATH after install

Open a new shell session and rerun:

```bash
logito --version
```

## Next Step

Continue to [Getting started](../docs/getting-started.md).
