# Example Workflows

This directory contains example workflows demonstrating how to use the Crumpled.Umbraco.Package.Actions in your project.

## Examples

### [release-workflow.yml](release-workflow.yml)
Complete release workflow that:
- Detects version from git tag
- Updates all package files
- Builds the package
- Creates a GitHub release
- Publishes to NuGet

### [pr-validation.yml](pr-validation.yml)
Pull request validation that:
- Validates version format
- Checks that version files are in sync
- Runs tests

## Using These Examples

1. Copy the relevant workflow file to your repository's `.github/workflows/` directory
2. Adjust the paths to match your project structure
3. Update the `path-to-extension` input to point to your extension directory
4. Commit and push

## Customizing

These examples are starting points. Common customizations:

- **Different triggers**: Change `on:` section
- **Multiple extensions**: Add matrix strategy
- **Additional steps**: Insert deployment, notification, etc.
- **Different runners**: Change `runs-on:`

## Need Help?

- Check the [main README](../README.md)
- Review the [action documentation](../update-version-files/README.md)
- Open an issue if you're stuck
