# Monorail Figma Plugin

This is the Figma plugin component of Monorail MCP. It communicates with the Monorail MCP server over WebSocket to enable real-time design collaboration.

## Installation

This plugin is typically installed as part of the Monorail MCP setup. See the main repository for full installation instructions: https://github.com/soul667/monorail-mcp

### Quick Install

If you've installed via npm:

```bash
npm install -g @soul667/monorail-figma-plugin
```

Then in Figma:
1. Go to **Plugins > Development > Import plugin from manifest...**
2. Navigate to `$(npm root -g)/@soul667/monorail-figma-plugin/manifest.json`

### Manual Installation

1. Download this package
2. In Figma, go to **Plugins > Development > Import plugin from manifest...**
3. Select the `manifest.json` file from this directory

## Usage

1. Open a Figma Slides document
2. Run **Plugins > Development > Monorail**
3. The plugin will connect to the MCP server automatically
4. Look for the green "Connected" indicator

## Requirements

- Figma desktop app or browser version
- Monorail MCP server running (installed separately)
- The MCP server must be running on port 9876 (default)

## Files

- `code.js` - Plugin logic (bundled)
- `manifest.json` - Figma plugin manifest
- `ui.html` - Plugin UI with WebSocket client

## Documentation

For full documentation, see: https://github.com/soul667/monorail-mcp

## License

MIT
