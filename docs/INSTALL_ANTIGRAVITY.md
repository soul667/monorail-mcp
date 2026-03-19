# Installing Monorail for Antigravity

Antigravity is an MCP-compatible client. This guide shows how to install and configure Monorail.

## Quick Install

### 1. Install from GitHub Packages

First, configure npm to authenticate with GitHub Packages:

```bash
echo "@soul667:registry=https://npm.pkg.github.com" >> ~/.npmrc
```

Then install the package globally:

```bash
npm install -g @soul667/monorail-mcp
```

### 2. Configure Antigravity

Add Monorail to your Antigravity MCP configuration file (typically `~/.config/antigravity/mcp.json` or similar):

```json
{
  "mcpServers": {
    "monorail": {
      "command": "monorail-mcp"
    }
  }
}
```

### 3. Install the Figma Plugin

#### Option A: From GitHub Packages (if you have Node.js)

```bash
npm install -g @soul667/monorail-figma-plugin
```

Then in Figma:
1. Go to **Plugins > Development > Import plugin from manifest...**
2. Navigate to: `$(npm root -g)/@soul667/monorail-figma-plugin/manifest.json`

#### Option B: Manual Installation

Download the latest plugin release:

```bash
# Create a directory for the plugin
mkdir -p ~/.monorail/figma-plugin
cd ~/.monorail/figma-plugin

# Download and extract the latest release
# Replace VERSION with the latest version number
curl -L https://github.com/soul667/monorail-mcp/releases/download/v0.1.0/figma-plugin.zip -o plugin.zip
unzip plugin.zip
rm plugin.zip
```

Then in Figma:
1. Go to **Plugins > Development > Import plugin from manifest...**
2. Select `~/.monorail/figma-plugin/manifest.json`

### 4. Verify Installation

1. Restart Antigravity
2. Open a Figma Slides document
3. Run the plugin: **Plugins > Development > Monorail**
4. Look for the green "Connected" indicator in the plugin
5. In Antigravity, run: `monorail_status`

You should see a success message confirming the plugin is connected.

## Alternative: Development Installation

If you want to contribute or modify Monorail:

```bash
# Clone the repository
git clone https://github.com/soul667/monorail-mcp.git
cd monorail-mcp

# Install and build
npm install && npm run build
cd figma-plugin
npm install && npm run build
cd ..
```

Configure Antigravity to use the local installation:

```json
{
  "mcpServers": {
    "monorail": {
      "command": "node",
      "args": ["/full/path/to/monorail-mcp/dist/src/index.js"]
    }
  }
}
```

## Troubleshooting

### Package Authentication Error

If you get a 401 or 404 error when installing from GitHub Packages, you need to authenticate:

1. Create a GitHub Personal Access Token (PAT) with `read:packages` scope at: https://github.com/settings/tokens
2. Run: `npm login --registry=https://npm.pkg.github.com`
3. Use your GitHub username and the PAT as the password

Or add to `~/.npmrc`:
```
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
@soul667:registry=https://npm.pkg.github.com
```

### "No plugin connected" Error

1. Make sure Antigravity is running
2. Check that the plugin is running in Figma (green "Connected" indicator)
3. Verify port 9876 is not in use: `lsof -i :9876`
4. Restart Antigravity and re-run the Figma plugin

### Plugin Not Showing in Figma

1. Ensure you selected the correct manifest.json file
2. Try **Plugins > Development > Reload current plugin**
3. Check Figma's developer console (Plugins > Development > Open Console) for errors

## Updates

To update to the latest version:

```bash
npm update -g @soul667/monorail-mcp
npm update -g @soul667/monorail-figma-plugin
```

Then restart Antigravity and reload the Figma plugin.

## Uninstall

```bash
npm uninstall -g @soul667/monorail-mcp
npm uninstall -g @soul667/monorail-figma-plugin
```

Remove the Monorail configuration from your Antigravity MCP config file.
