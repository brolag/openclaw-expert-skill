---
name: openclaw-expert
description: Expert knowledge on OpenClaw (formerly Clawdbot) - the universal AI agent gateway for WhatsApp, Telegram, Discord, iMessage, and more. Use when user asks about OpenClaw configuration, multi-agent routing, skills system, gateway architecture, channels setup, Pi agent integration, or any OpenClaw-related questions. Triggers on mentions of "openclaw", "clawdbot", "whatsapp bot", "telegram bot with AI", "pi agent", "clawhub", or gateway-based AI messaging systems.
---

# OpenClaw Expert

Deep expertise on OpenClaw v2026.2.x - the universal gateway connecting AI agents to messaging platforms.

## Quick Reference

- **Docs**: https://docs.openclaw.ai
- **GitHub**: https://github.com/openclaw/openclaw
- **ClawHub (Skills)**: https://clawhub.com
- **Requirement**: Node ≥ 22

## Architecture Overview

```
WhatsApp / Telegram / Discord / iMessage
              │
              ▼
     ┌─────────────────────┐
     │      Gateway        │  ws://127.0.0.1:18789
     │   (single daemon)   │  http://:18793 (Canvas)
     └──────────┬──────────┘
              │
     ├─ Pi agent (RPC)
     ├─ CLI (openclaw …)
     ├─ macOS/iOS/Android nodes
     └─ WebChat UI
```

**Key principle**: One Gateway per host — it's the only process that can hold a WhatsApp Web session.

## Essential Commands

```bash
# Install
npm install -g openclaw@latest

# Setup + daemon
openclaw onboard --install-daemon

# Pair WhatsApp
openclaw channels login

# Manual gateway
openclaw gateway --port 18789 --token <token>

# Agent management
openclaw agents list --bindings
openclaw agents add <name>

# Health check
openclaw doctor
```

## Configuration

Config: `~/.openclaw/openclaw.json` (JSON5 format)

### Basic Setup

```json5
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: {
    groupChat: { mentionPatterns: ["@openclaw"] }
  },
}
```

## Multi-Agent Routing

Route different channels/contacts to isolated agents with separate workspaces, auth, and sessions.

### Example: WhatsApp + Telegram Split

```json5
{
  agents: {
    list: [
      { id: "chat", model: "anthropic/claude-sonnet-4-5" },
      { id: "opus", model: "anthropic/claude-opus-4-5" },
    ],
  },
  bindings: [
    { agentId: "chat", match: { channel: "whatsapp" } },
    { agentId: "opus", match: { channel: "telegram" } },
  ],
}
```

### Routing Precedence

1. `peer` match (exact DM/group)
2. `guildId` (Discord) / `teamId` (Slack)
3. `accountId` match
4. `channel` match
5. Default agent

### Per-Agent Sandbox

```json5
{
  agents: {
    list: [{
      id: "family",
      sandbox: { mode: "all", scope: "agent" },
      tools: {
        allow: ["read"],
        deny: ["exec", "write"],
      },
    }],
  },
}
```

## Skills System

Skills teach the agent to use tools via `SKILL.md` files with YAML frontmatter.

### Precedence

1. `<workspace>/skills` (highest)
2. `~/.openclaw/skills` (shared)
3. Bundled skills (lowest)

### Skill Format

```yaml
---
name: my-skill
description: What this skill does
metadata: { "openclaw": { "requires": { "bins": ["uv"], "env": ["API_KEY"] } } }
---
[Instructions here]
```

### Skill Config

```json5
{
  skills: {
    entries: {
      "my-skill": {
        enabled: true,
        apiKey: "KEY_HERE",
      },
    },
  },
}
```

## Supported Channels

| Channel | Protocol | Features |
|---------|----------|----------|
| WhatsApp | Baileys | DMs, groups, media |
| Telegram | grammY | DMs, groups, forums, stickers, draft streaming |
| Discord | discord.js | DMs, guilds, threads |
| iMessage | imsg CLI | macOS only |
| Mattermost | Plugin | Bot token + WebSocket |
| Slack | Plugin | Teams integration |

## Key Paths

| What | Path |
|------|------|
| Config | `~/.openclaw/openclaw.json` |
| State | `~/.openclaw/` |
| Workspace | `~/.openclaw/workspace` |
| Agent dir | `~/.openclaw/agents/<id>/agent` |
| Sessions | `~/.openclaw/agents/<id>/sessions` |
| Shared skills | `~/.openclaw/skills` |

## Security Notes (v2026.1.29+)

- **Auth mode "none" removed** — Gateway always requires token
- TLS 1.3 minimum for listeners
- Skills from third parties = untrusted code
- Auth profiles are per-agent (not shared)

## Recent Changes (v2026.2.1)

- Pi SDK updated to 0.50.9/0.51.0
- System prompt safety guardrails
- Telegram shared pairing store
- Multiple security fixes (path traversal, LFI prevention)
- TLS 1.3 requirement

For detailed changelog and configuration examples, see references/changelog.md.
