# Install on macOS

## Install

```bash
brew install progadigital/tap/logito
```

## Verify

```bash
logito --version
```

## First Run

```bash
logito dev start
logito review
```

## Troubleshooting

### `brew` is not installed

Install Homebrew first, then rerun the command above.

### Formula not found

Refresh your brew metadata and try again:

```bash
brew update
brew install progadigital/tap/logito
```

### Old version still appears

Upgrade explicitly:

```bash
brew upgrade progadigital/tap/logito
```

### `logito` is not on PATH after install

Open a new shell session and rerun:

```bash
logito --version
```

## Next Step

Continue to [Getting started](../docs/getting-started.md).
