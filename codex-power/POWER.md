---
name: "codex"
displayName: "OpenAI Codex CLI"
description: "Use OpenAI Codex CLI as an MCP server, enabling AI agents to execute complex coding tasks, file operations, and shell commands"
keywords: ["codex", "openai", "cli", "coding", "agent", "code generation", "automation"]
author: "OpenAI"
---

# OpenAI Codex CLI Power

## Overview

OpenAI Codex CLI is a powerful command-line tool that can run as an MCP server, enabling other AI agents to execute complex coding tasks. With this Power, you can:

- **Execute coding tasks**: Let Codex agent help you write, modify, and debug code
- **File operations**: Create, edit, move, and delete files and directories
- **Shell commands**: Execute various shell commands for automation tasks
- **Multi-turn conversations**: Continue previous conversations using conversation ID

**Key capabilities:**

- Support for multiple OpenAI models (o3, o4-mini, etc.)
- Flexible sandbox modes to control file access permissions
- Configurable command approval policies
- Support for custom instructions and configurations

## Available Steering Files

- **usage-rules** - Codex usage rules and constraints to ensure correct use of cwd parameter

## Available MCP Servers

### codex

**Connection:** STDIO (local command launch)
**Command:** `codex mcp-server`

## Available Tools

### 1. codex

Start a new Codex session to execute tasks.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `prompt` | string | ✅ | Initial user prompt describing the task to execute |
| `cwd` | string | ✅ | Working directory, **must use the absolute path of current workspace** |
| `sandbox` | string | ✅ | Sandbox mode: `read-only` for advice/analysis, `danger-full-access` for modifications |
| `approval-policy` | string | ❌ | Command approval policy: `untrusted`, `on-failure`, `on-request`, `never` |
| `model` | string | ❌ | Model name (e.g., `o3`, `o4-mini`) |
| `base-instructions` | string | ❌ | Custom instructions to replace default ones |
| `profile` | string | ❌ | Configuration profile from config.toml |
| `config` | object | ❌ | Config items to override config.toml |

**Example:**
```json
{
  "prompt": "Create a simple React component that displays a user list",
  "sandbox": "workspace-write",
  "approval-policy": "never",
  "cwd": "/path/to/project"
}
```

### 2. codex-reply

Continue an existing Codex session.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `conversationId` | string | ✅ | The session ID to continue |
| `prompt` | string | ✅ | User prompt to continue the conversation |

**Example:**
```json
{
  "conversationId": "conv_abc123",
  "prompt": "Add pagination to this component"
}
```

## Best Practices

### Sandbox Mode Selection

Choose the appropriate sandbox mode based on task requirements:

| Mode | Description | Use Case |
|------|-------------|----------|
| `read-only` | Read-only mode, cannot modify files | Code review, analysis tasks |
| `workspace-write` | Can write to workspace files | Most development tasks (recommended) |
| `danger-full-access` | Full access permissions | System-level operations (use with caution) |

### Approval Policy Selection

| Policy | Description | Use Case |
|--------|-------------|----------|
| `untrusted` | All commands require approval | High security requirements |
| `on-failure` | Approval required on failure | Balance security and efficiency |
| `on-request` | Approval on request | Flexible control |
| `never` | Auto-execute all commands | Automated workflows (requires trusted environment) |

### Working Directory Setup

- **Always set `cwd`**: Ensure Codex works in the correct project directory
- **Use absolute paths**: Avoid path resolution issues
- **Verify directory exists**: Ensure directory exists before execution

### Prompt Writing

1. **Be specific**: Clearly describe the task to complete
2. **Break into steps**: Split complex tasks into multiple steps
3. **Provide context**: Explain project background and tech stack
4. **Specify output**: Clearly state expected output format and location

## Common Workflows

### Workflow 1: Create New Project

```json
{
  "prompt": "Create a TypeScript + Express REST API project with user CRUD endpoints",
  "sandbox": "workspace-write",
  "approval-policy": "never",
  "cwd": "/Users/me/projects/new-api"
}
```

### Workflow 2: Code Refactoring

```json
{
  "prompt": "Convert all functions in src/utils.js to TypeScript with type definitions",
  "sandbox": "workspace-write",
  "approval-policy": "on-failure",
  "cwd": "/Users/me/projects/my-app"
}
```

### Workflow 3: Bug Fixing

```json
{
  "prompt": "Analyze and fix performance issues in src/components/UserList.tsx",
  "sandbox": "workspace-write",
  "approval-policy": "on-failure",
  "cwd": "/Users/me/projects/my-app"
}
```

### Workflow 4: Multi-turn Conversation

```json
// First turn: Create basic structure
{
  "prompt": "Create a React component framework",
  "sandbox": "workspace-write"
}

// Second turn: Add features (using returned conversationId)
{
  "conversationId": "conv_abc123",
  "prompt": "Add state management to this component"
}
```

## Configuration

### Prerequisites

1. **Install Codex CLI**:
   ```bash
   npm install -g @openai/codex
   ```

2. **Configure OpenAI API Key**:
   ```bash
   export OPENAI_API_KEY="your-api-key"
   ```

3. **Verify installation**:
   ```bash
   codex --version
   ```

### MCP Configuration

```json
{
  "mcpServers": {
    "codex": {
      "command": "codex",
      "args": ["mcp-server"]
    }
  }
}
```

### Advanced Configuration

To customize Codex configuration, edit `~/.codex/config.toml`:

```toml
# Default model
model = "o4-mini"

# Default sandbox mode
sandbox = "workspace-write"

# Default approval policy
approval-policy = "on-failure"
```

## Troubleshooting

### Error: codex command not found

**Cause:** Codex CLI not installed or not in PATH
**Solution:**
1. Verify installation: `npm list -g @openai/codex`
2. Reinstall: `npm install -g @openai/codex`
3. Check PATH: `which codex`

### Error: Invalid API Key

**Cause:** OpenAI API Key not set or invalid
**Solution:**
1. Check environment variable: `echo $OPENAI_API_KEY`
2. Reset: `export OPENAI_API_KEY="sk-..."`
3. Verify key validity

### Error: Permission denied

**Cause:** Sandbox mode restricts file access
**Solution:**
1. Check `sandbox` parameter setting
2. Use `workspace-write` or `danger-full-access`
3. Ensure `cwd` points to correct working directory

### Error: Session timeout

**Cause:** Codex task execution took too long
**Solution:**
1. Split complex tasks into smaller tasks
2. Increase timeout settings
3. Use `codex-reply` to continue unfinished sessions

### Error: Invalid conversationId

**Cause:** Session expired or incorrect ID
**Solution:**
1. Confirm conversationId comes from previous codex call
2. Session may have timed out, need to restart
3. Check ID format is correct

## Tips

1. **Set reasonable timeout**: Codex tasks may take several minutes, recommend 10-minute timeout
2. **Use workspace-write**: Sufficient for most development tasks
3. **Save conversationId**: Convenient for continuing conversations later
4. **Specify working directory**: Always set `cwd` to avoid file location confusion
5. **Execute in steps**: Splitting complex tasks into smaller tasks is more reliable
6. **Check output**: Verify generated code and files after execution

## Resources

- [Codex CLI GitHub](https://github.com/openai/codex)
- [Codex MCP Documentation](https://developers.openai.com/codex/mcp/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [OpenAI API Documentation](https://platform.openai.com/docs)

---

**License:** MIT
