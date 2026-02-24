# MCP Client Integration (Agent-Agnostic)

AQE exposes capabilities through an MCP server (`aqe-mcp` / `npx agentic-qe mcp`).

This means you can use AQE from any MCP-compatible agent client, including Claude-based, Codex-compatible, and Opencode setups.

## Supported Clients

- [Claude Code](#claude-code)
- [Opencode](#opencode)
- [Other MCP Clients](#other-mcp-clients)

---

## Claude Code

```bash
# aqe init --auto configures .mcp.json automatically
# Claude Code will auto-start the MCP server on connection
```

---

## Opencode

[Opencode](https://opencode.ai) is an open-source AI coding agent that supports any LLM provider. It's a great alternative if you want to use AQE without vendor lock-in.

### Why Opencode?

- **No vendor lock-in**: Use any LLM provider (Claude, GPT, Gemini, local models, etc.)
- **MCP Support**: Full MCP server compatibility
- **Privacy-first**: Code stays local
- **Free models included**: Or connect your own API keys

### Installation

1. **Install Opencode:**

```bash
# Using curl (recommended)
curl -fsSL https://opencode.ai/install | bash

# Or using npm
npm install -g opencode-ai
```

2. **Install AQE:**

```bash
npm install -g agentic-qe
cd your-project
aqe init --auto
```

3. **Configure Opencode MCP:**

Add to your `opencode.json` (global: `~/opencode.json` or project root):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "agentic-qe": {
      "type": "local",
      "command": ["aqe-mcp"],
      "enabled": true,
      "environment": {
        "AQE_PROJECT_ROOT": ".",
        "AQE_LEARNING_ENABLED": "true",
        "AQE_WORKERS_ENABLED": "true"
      }
    }
  }
}
```

4. **Restart Opencode** to load the MCP tools.

### Usage

Use AQE tools in your prompts:

```
use agentic-qe to initialize the fleet
use agentic-qe to generate tests for src/auth.ts
use agentic-qe to assess quality of the codebase
```

Tools are prefixed with `agentic-qe_`:
- `agentic-qe_fleet_init`
- `agentic-qe_test_generate_enhanced`
- `agentic-qe_quality_assess`
- `agentic-qe_memory_store`
- etc.

### LLM Providers

Set API keys as environment variables or in Opencode config:

```json
{
  "mcp": { ... },
  "env": {
    "ANTHROPIC_API_KEY": "{env:ANTHROPIC_API_KEY}",
    "OPENAI_API_KEY": "{env:OPENAI_API_KEY}"
  }
}
```

### Comparison: Claude Code vs Opencode

| Feature | Claude Code | Opencode |
|---------|-------------|----------|
| MCP Support | ✅ | ✅ |
| Custom LLM Providers | Limited | ✅ Any provider |
| Learning Persistence | ✅ | ✅ |
| Self-hosting | ❌ | ✅ |
| Cost | API costs only | API costs only |

---

## Other MCP Clients

## 1) Start AQE MCP server

```bash
# global install path
aqe-mcp

# or ephemeral
npx agentic-qe mcp
```

## 2) Connect from your MCP client

In your client, add an MCP tool endpoint/command that runs one of:
- `aqe-mcp`
- `npx agentic-qe mcp`

## 3) Validate capability discovery

After connection, verify AQE tools/agents are discoverable in your client.

## 4) Suggested first tasks

- Generate tests with `qe-test-architect`
- Run orchestration with `qe-queen-coordinator`
- Analyze flaky tests with `qe-flaky-hunter`

## Notes

- AQE core is provider-neutral at the MCP boundary.
- Client UX / prompt syntax differs by agent client; tool names stay AQE-native.
- Keep MCP server and client versions pinned in CI/devcontainer for reproducibility.
