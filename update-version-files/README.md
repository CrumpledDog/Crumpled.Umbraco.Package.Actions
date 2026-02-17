# Update Version Files Action

A composite GitHub Action that updates version numbers in umbraco-package.json, package.manifest, and related files for Umbraco extensions.

## Features

- Updates `Client/public/umbraco-package.json` (if exists)
- Updates `Client/package.json` (if exists)
- Updates `wwwroot/umbraco-package.json` (if exists and Client version doesn't exist)
- Updates `wwwroot/package.manifest` (if exists, Umbraco v13)
- Converts semantic version to numeric file version
- Outputs the file version for use in subsequent steps

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `semver` | Semantic version to set (e.g., `1.0.0-alpha.1`) | Yes |
| `path-to-extension` | Path to the extension directory (e.g., `src/Extension`) | Yes |

## Outputs

| Output | Description |
|--------|-------------|
| `file-version` | The numeric file version (e.g., `1.0.0.1`) |

## Usage

### Basic Usage

```yaml
- name: Update version files
  uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1
  with:
    semver: '1.0.0-alpha.1'
    path-to-extension: 'src/Extension'
```

### With Dynamic Version

```yaml
- name: Update version files
  uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1
  with:
    semver: ${{ needs.setup.outputs.semver }}
    path-to-extension: 'src/Extension'
```

### Using the Output

```yaml
- name: Update version files
  id: version
  uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1
  with:
    semver: ${{ needs.setup.outputs.semver }}
    path-to-extension: 'src/Extension'

- name: Pack NuGet package
  working-directory: src/Extension
  run: dotnet pack --configuration Release /p:PackageVersion=${{ needs.setup.outputs.semver }} /p:FileVersion=${{ steps.version.outputs.file-version }}
```

## Requirements

- `jq` must be available in the runner (pre-installed on GitHub-hosted runners)
- Git repository with tags (for stable release version calculation)

## Version Conversion Logic

### Pre-release versions
Extracts the numeric suffix from the prerelease identifier:
- Input: `1.0.0-alpha.1` → Output: `1.0.0.1`
- Input: `2.3.4-beta.5` → Output: `2.3.4.5`
- Input: `1.0.0-rc.10` → Output: `1.0.0.10`

### Stable versions
Finds the highest pre-release number for the version and increments it:
- If `v1.0.0-alpha.3` is the highest tag, `1.0.0` → `1.0.0.4`
- If no pre-release tags exist, `1.0.0` → `1.0.0.1`

This ensures that stable releases have a higher file version than any pre-release.

## File Priority

The action handles different Umbraco package structures:

1. **Client-based packages** (Umbraco v14+): Updates `Client/public/umbraco-package.json` and `Client/package.json`
2. **wwwroot-based packages**: Updates `wwwroot/umbraco-package.json` and `wwwroot/package.manifest`

If a `Client/public/umbraco-package.json` exists, the `wwwroot/umbraco-package.json` will NOT be updated (Client version takes precedence).

## Example Workflow

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      semver: ${{ steps.version.outputs.semver }}
    steps:
      - name: Get version from tag
        id: version
        run: echo "semver=${GITHUB_REF#refs/tags/v}" >> $GITHUB_OUTPUT

  build:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Update version files
        id: update-version
        uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1
        with:
          semver: ${{ needs.setup.outputs.semver }}
          path-to-extension: 'src/MyPackage'
      
      - name: Pack NuGet package
        working-directory: src/MyPackage
        run: dotnet pack --configuration Release /p:PackageVersion=${{ needs.setup.outputs.semver }} /p:FileVersion=${{ steps.update-version.outputs.file-version }}
```

## License

MIT License
