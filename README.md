# OpenAI Codex CLI Power for Kiro

A Kiro Power that enables AI agents to execute complex coding tasks using OpenAI Codex CLI as an MCP server.

## Features

- Execute coding tasks through Codex agent
- File operations (create, edit, move, delete)
- Shell command execution
- Multi-turn conversations with conversation ID

## Installation

### From GitHub

1. Open Kiro IDE
2. Go to Powers panel
3. Click "Import from GitHub"
4. Enter: `https://github.com/user/codex-power` (replace with your repo URL)

### From Local Folder

1. Clone this repository
2. Open Kiro IDE → Powers panel
3. Click "Import from folder"
4. Select the cloned directory

## Prerequisites

1. Install Codex CLI:
   ```bash
   npm install -g @openai/codex
   ```

2. Configure OpenAI API Key (choose one method):

   **Method A: Environment variable (recommended)**
   ```bash
   export OPENAI_API_KEY="your-api-key"
   ```

   **Method B: Codex config file**
   ```bash
   codex config set api-key your-api-key
   ```

## Environment Variable Configuration

If your Codex CLI uses environment variables for API key, you need to pass them in the MCP configuration.

After installing this power, modify your `~/.kiro/settings/mcp.json` to include the env section:

```json
{
  "powers": {
    "mcpServers": {
      "power-codex-codex": {
        "command": "codex",
        "args": ["mcp-server"],
        "env": {
          "OPENAI_API_KEY": "${OPENAI_API_KEY}"
        }
      }
    }
  }
}
```

This ensures the `OPENAI_API_KEY` environment variable is passed to the Codex MCP server.

## Usage

Once installed, mention keywords like "codex", "coding", "agent" in your chat to activate this power.

### Required Parameters

| Parameter | Description |
|-----------|-------------|
| `cwd` | Absolute path to current workspace (required) |
| `sandbox` | `read-only` for analysis, `danger-full-access` for modifications (required) |
| `prompt` | Task description (required) |

## License

MIT
