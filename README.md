# Exa MCP Server Deployment

This repository contains the configuration to deploy the [Exa MCP Server](https://github.com/exa-labs/exa-mcp-server) to Render or any other cloud provider.

## Local Usage

You can run the server locally in stdio mode (for Claude Desktop) via `npx`:

```bash
export EXA_API_KEY=your_key
npx exa-mcp-server
```

Or run the HTTP server locally:

```bash
export EXA_API_KEY=your_key
npm start
```

## Deployment to Render

You have two options for deployment: **Node (recommended)** or **Docker**.

### Option 1: Node (Simpler)
1.  Create a new **Web Service** on Render.
2.  Connect this repository.
3.  Select **Node** as the Runtime.
4.  Render will automatically try to run `npm start`, which is already configured.
5.  Add the Environment Variable:
    *   `EXA_API_KEY`: Your Exa API key.
6.  Deploy.

### Option 2: Docker
1.  Create a new **Web Service** on Render.
2.  Connect this repository.
3.  Select **Docker** as the Runtime.
4.  Add the Environment Variable:
    *   `EXA_API_KEY`: Your Exa API key.
5.  Deploy.

## Client Configuration

Once deployed, configure your MCP client (Claude Desktop, Cursor, etc.) to point to your new server URL.

**Claude Desktop Configuration (`claude_desktop_config.json`):**

Since Claude Desktop primarily supports stdio, use `mcp-remote` to bridge to your remote server:

```json
{
  "mcpServers": {
    "exa-custom": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://your-render-app-name.onrender.com/mcp"
      ]
    }
  }
}
```

**Cursor & Claude Code Configuration:**

Cursor and Claude Code support HTTP directly:

```json
{
  "mcpServers": {
    "exa-custom": {
      "type": "http",
      "url": "https://your-render-app-name.onrender.com/mcp",
      "headers": {}
    }
  }
}
```

**Note:** The `/mcp` path suffix is common for MCP servers over SSE. If the root URL doesn't work, try adding `/sse` or just the root depending on the server implementation. The standard Exa server typically uses `/mcp`.

## Environment Variables

*   `EXA_API_KEY`: Required. Your Exa API key.
*   `PORT`: Optional. The port to listen on (default: 8080).
