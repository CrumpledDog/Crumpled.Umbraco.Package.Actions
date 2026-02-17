# Crumpled.Umbraco.Package.Actions

A collection of reusable GitHub Actions for building and maintaining Umbraco packages.

## Available Actions

### Update Version Files

Updates version numbers in umbraco-package.json, package.manifest, and related files for Umbraco extensions.

**Usage:**
```yaml
- name: Update version files
  uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@main
  with:
    semver: '1.0.0-alpha.1'
    path-to-extension: 'src/Extension'
```

See [update-version-files/README.md](update-version-files/README.md) for full documentation.

## Using These Actions

### From any repository

Reference the action using the full path:

```yaml
steps:
  - uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@main
    with:
      semver: ${{ needs.setup.outputs.semver }}
      path-to-extension: 'src/MyExtension'
```

### Version Pinning

For production use, pin to a specific version instead of `@main`:

```yaml
- uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1.0.0
```

Or use a major version tag that follows the latest minor/patch:

```yaml
- uses: crumpled/Crumpled.Umbraco.Package.Actions/update-version-files@v1
```

## Contributing

We welcome contributions! Each action should:
- Have its own directory with an `action.yml` file
- Include a comprehensive README.md
- Follow composite action best practices
- Be tested before merging

## License

MIT License - see [LICENSE](LICENSE) for details.
