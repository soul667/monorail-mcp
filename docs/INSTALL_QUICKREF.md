# Monorail Installation Quick Reference

## For Users

### Antigravity
```bash
npm install -g @soul667/monorail-mcp @soul667/monorail-figma-plugin
```
Then add to Antigravity config:
```json
{"mcpServers": {"monorail": {"command": "monorail-mcp"}}}
```
📚 Full guide: [docs/INSTALL_ANTIGRAVITY.md](INSTALL_ANTIGRAVITY.md)

### OpenCode
```bash
npm install -g @soul667/monorail-mcp @soul667/monorail-figma-plugin
```
Then add to OpenCode MCP settings:
```json
{"mcpServers": {"monorail": {"command": "monorail-mcp"}}}
```
📚 Full guide: [docs/INSTALL_OPENCODE.md](INSTALL_OPENCODE.md)

### Claude Desktop / Claude Code
```bash
npm install -g @soul667/monorail-mcp @soul667/monorail-figma-plugin
```
Then add to `claude_desktop_config.json`:
```json
{"mcpServers": {"monorail": {"command": "monorail-mcp"}}}
```

### Figma Plugin (All Clients)
After installing the npm packages, in Figma:
1. **Plugins > Development > Import plugin from manifest...**
2. Find: `$(npm root -g)/@soul667/monorail-figma-plugin/manifest.json`

## For Maintainers

### Publishing New Version
```bash
# Method 1: Create GitHub Release (automated)
# Go to https://github.com/soul667/monorail-mcp/releases
# Click "Draft a new release" → workflows run automatically

# Method 2: Manual workflow trigger
# Go to Actions tab → "Publish Packages" → "Run workflow"

# Method 3: Local publish
npm version patch  # Update version
npm run build && npm publish
cd figma-plugin
npm version patch
npm run build && npm publish
```
📚 Full guide: [docs/PUBLISHING.md](PUBLISHING.md)

## Authentication

If installing from GitHub Packages fails:
```bash
# Configure npm
echo "@soul667:registry=https://npm.pkg.github.com" >> ~/.npmrc

# For private packages, add token:
echo "//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN" >> ~/.npmrc
```

## Project Structure

```
monorail-mcp/
├── dist/                          # MCP server build output
├── figma-plugin/
│   ├── code.js                    # Plugin build output
│   └── manifest.json              # Figma plugin manifest
├── docs/
│   ├── INSTALL_ANTIGRAVITY.md     # Antigravity installation guide
│   ├── INSTALL_OPENCODE.md        # OpenCode installation guide
│   └── PUBLISHING.md              # Publishing guide for maintainers
└── .github/workflows/publish.yml  # Automated publishing workflow
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| 401/404 on npm install | Authenticate with GitHub Packages |
| "No plugin connected" | Check port 9876, restart MCP client |
| Command not found | Use full path to monorail-mcp |
| Plugin not in Figma | Import manifest.json again |

## Links

- Repository: https://github.com/soul667/monorail-mcp
- Issues: https://github.com/soul667/monorail-mcp/issues
- Releases: https://github.com/soul667/monorail-mcp/releases
