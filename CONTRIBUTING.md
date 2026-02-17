# Contributing to Crumpled.Umbraco.Package.Actions

Thank you for considering contributing to this project! This document provides guidelines for contributing.

## Code of Conduct

Be respectful and inclusive. We welcome contributions from everyone.

## How to Contribute

### Reporting Issues

- Check if the issue already exists
- Provide clear steps to reproduce
- Include example workflows if applicable
- Specify which action is affected

### Suggesting Features

- Open an issue describing the feature
- Explain the use case and benefits
- Consider if it's generally applicable to Umbraco package development

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Make your changes
4. Test your action locally
5. Update documentation (README, CHANGELOG)
6. Commit with clear messages
7. Push and create a pull request

## Action Structure

Each action should follow this structure:

```
action-name/
├── action.yml       # Action definition
├── README.md        # Comprehensive documentation
└── tests/           # Optional test workflows
```

### Action Guidelines

1. **Composite Actions**: Use composite actions (shell scripts) when possible
2. **Inputs**: Clearly describe all inputs with examples
3. **Outputs**: Document all outputs
4. **Error Handling**: Use `set -e` and handle errors gracefully
5. **Documentation**: Include usage examples
6. **Testing**: Manually test before submitting

### Documentation Requirements

Each action must include:

- Clear description of what it does
- Input/output table
- At least one usage example
- Requirements section
- Example workflow when applicable

### Commit Messages

- Use clear, descriptive commit messages
- Start with a verb (Add, Fix, Update, etc.)
- Reference issues when applicable

Example:
```
Add support for v15 package format

- Updated update-version-files to support new structure
- Added backwards compatibility
- Fixes #123
```

## Testing Your Changes

Before submitting a PR:

1. Test the action in a real workflow
2. Verify all documented examples work
3. Check edge cases (missing files, invalid versions, etc.)
4. Update tests if applicable

### Local Testing

You can test composite actions locally by running the shell script directly:

```bash
# For update-version-files
bash update-version-files/action.yml semver path-to-extension
```

Or test in a workflow by referencing your fork:

```yaml
uses: your-username/Crumpled.Umbraco.Package.Actions/update-version-files@your-branch
```

## Versioning

This project uses semantic versioning:

- **Major**: Breaking changes
- **Minor**: New features (backwards compatible)
- **Patch**: Bug fixes

When your PR is merged, maintainers will:
1. Update CHANGELOG.md
2. Create a new version tag
3. Update major version tag (e.g., `v1` points to latest `v1.x.x`)

## Questions?

Open an issue with your question, and we'll be happy to help!

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
