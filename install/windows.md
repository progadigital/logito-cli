# Install on Windows

## Install

```powershell
winget install progadigital.logito
```

## Verify

```powershell
logito --version
```

## First Run

```powershell
logito dev start
logito review
```

## Troubleshooting

### `winget` is not available

Update the Windows App Installer or use a shell session where WinGet is available, then retry the command above.

### Package not found

Refresh sources and try again:

```powershell
winget source update
winget install progadigital.logito
```

### Old version is still installed

Upgrade explicitly:

```powershell
winget upgrade progadigital.logito
```

### `logito` is not recognized after install

Open a new terminal session and rerun:

```powershell
logito --version
```

## Next Step

Continue to [Getting started](../docs/getting-started.md).
