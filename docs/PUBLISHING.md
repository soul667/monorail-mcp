# Publishing Guide

This document explains how to publish new versions of Monorail to GitHub Packages.

## Overview

Monorail consists of two packages:
1. **@soul667/monorail-mcp** - The MCP server
2. **@soul667/monorail-figma-plugin** - The Figma plugin

Both are published to GitHub Packages and can be installed via npm.

## Prerequisites

- Write access to the repository
- GitHub Personal Access Token (PAT) with `write:packages` scope

## Publishing Methods

### Method 1: Automatic Publishing (via GitHub Release)

When you create a new release on GitHub, the workflow automatically publishes both packages:

1. Go to https://github.com/soul667/monorail-mcp/releases
2. Click "Draft a new release"
3. Create a new tag (e.g., `v0.1.1`)
4. Fill in the release title and description
5. Click "Publish release"

The GitHub Actions workflow will automatically:
- Build both packages
- Publish them to GitHub Packages with the version from package.json

### Method 2: Manual Publishing (via GitHub Actions)

You can manually trigger the publish workflow from the Actions tab:

1. Go to https://github.com/soul667/monorail-mcp/actions
2. Select the "Publish Packages" workflow
3. Click "Run workflow"
4. Optionally specify a version number (e.g., `0.1.1`)
5. Click "Run workflow"

The workflow will:
- Build both packages
- Update the version in package.json if you specified one
- Publish to GitHub Packages

### Method 3: Local Publishing (Manual)

For local testing or if automated methods fail:

#### Authenticate with GitHub Packages

```bash
# Add GitHub token to .npmrc
echo "//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN" >> ~/.npmrc
echo "@soul667:registry=https://npm.pkg.github.com" >> ~/.npmrc
```

#### Publish MCP Server

```bash
# Build
npm install
npm run build

# Update version (if needed)
npm version patch  # or minor, major, or specific version like 0.2.0

# Publish
npm publish
```

#### Publish Figma Plugin

```bash
cd figma-plugin

# Build
npm install
npm run build

# Update version (if needed)
npm version patch  # or minor, major, or specific version like 0.2.0

# Publish
npm publish
```

## Version Management

Both packages should typically be kept in sync (same version number).

### Updating Versions

Before publishing, update the version in both `package.json` files:

```bash
# Root package (MCP server)
npm version patch  # 0.1.0 -> 0.1.1
# or
npm version minor  # 0.1.0 -> 0.2.0
# or
npm version major  # 0.1.0 -> 1.0.0

# Figma plugin
cd figma-plugin
npm version patch  # or minor, major
cd ..
```

Or manually edit both `package.json` files to set the desired version.

### Semantic Versioning

Follow semantic versioning (semver):
- **Patch** (0.1.0 -> 0.1.1): Bug fixes, minor changes
- **Minor** (0.1.0 -> 0.2.0): New features, backwards compatible
- **Major** (0.1.0 -> 1.0.0): Breaking changes

## Verifying Publication

After publishing, verify the packages are available:

```bash
# Check MCP server package
npm view @soul667/monorail-mcp

# Check Figma plugin package
npm view @soul667/monorail-figma-plugin
```

## Testing Installation

Test the published packages:

```bash
# In a temporary directory
mkdir test-install
cd test-install

# Configure npm
echo "@soul667:registry=https://npm.pkg.github.com" >> ~/.npmrc

# Install
npm install -g @soul667/monorail-mcp
npm install -g @soul667/monorail-figma-plugin

# Verify commands work
monorail-mcp --help
```

## Troubleshooting

### Authentication Errors

If you get 401 or 403 errors:
1. Verify your GitHub token has `write:packages` scope
2. Check token is correctly added to `.npmrc`
3. Try `npm login --registry=https://npm.pkg.github.com`

### Build Failures

If the workflow fails:
1. Check the GitHub Actions logs
2. Verify TypeScript compilation works locally: `npm run build`
3. Ensure all dependencies are installed

### Version Conflicts

If npm complains about existing versions:
1. Check published versions: `npm view @soul667/monorail-mcp versions`
2. Choose a new, unique version number
3. Update `package.json` manually before publishing

## Rollback

If a version has issues:

1. Publish a new patch version with the fix
2. Update documentation to recommend the new version
3. Consider deprecating the problematic version:
   ```bash
   npm deprecate @soul667/monorail-mcp@0.1.1 "Please upgrade to 0.1.2"
   ```

## Best Practices

1. **Test before publishing**: Always test builds and functionality locally first
2. **Update changelog**: Keep CHANGELOG.md or release notes up to date
3. **Version sync**: Keep both packages at the same version when possible
4. **Git tags**: Create git tags for releases: `git tag v0.1.1 && git push --tags`
5. **Announce updates**: Update README.md or notify users of new versions

## CI/CD Workflow

The automated workflow (`.github/workflows/publish.yml`) includes:
- ✅ Node.js setup (v18)
- ✅ Dependency installation
- ✅ TypeScript compilation
- ✅ Package publishing to GitHub Packages
- ✅ Separate jobs for MCP server and Figma plugin

The workflow is triggered by:
- Creating a new GitHub release
- Manual workflow dispatch (with optional version parameter)
