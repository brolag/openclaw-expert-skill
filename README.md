# OpenClaw Expert Skill 🦞

A Claude/Pi skill that provides deep expertise on [OpenClaw](https://docs.openclaw.ai) - the universal AI agent gateway for WhatsApp, Telegram, Discord, iMessage, and more.

## What's Included

- **SKILL.md** - Core skill with architecture, commands, configuration examples, multi-agent routing, and security notes
- **references/changelog.md** - Detailed changelog for v2026.2.x with configuration examples

## Installation

### For OpenClaw/Pi Agent

Copy to your workspace skills folder:

```bash
cp -r openclaw-expert <your-workspace>/skills/
```

Or add to `extraDirs` in `~/.openclaw/openclaw.json`:

```json5
{
  skills: {
    load: {
      extraDirs: ["/path/to/openclaw-expert"],
    },
  },
}
```

### For Claude Code

Copy to your Claude skills directory:

```bash
cp -r openclaw-expert ~/.claude/skills/
```

## What This Skill Covers

- **Architecture** - Gateway, nodes, WebSocket protocol, Canvas
- **Configuration** - JSON5 config, channels, agents, bindings
- **Multi-Agent Routing** - Multiple isolated agents, routing rules, sandbox per agent
- **Skills System** - Creating, configuring, and managing OpenClaw skills
- **Channels** - WhatsApp, Telegram, Discord, iMessage, Slack, Mattermost
- **Security** - Auth requirements, TLS, sandboxing, best practices
- **Recent Changes** - Changelog for v2026.2.1, v2026.1.30, v2026.1.29

## Triggers

The skill activates when you mention:
- "openclaw", "clawdbot"
- "whatsapp bot", "telegram bot with AI"
- "pi agent", "clawhub"
- Gateway-based AI messaging systems

## Resources

- [OpenClaw Docs](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [ClawHub (Skills Registry)](https://clawhub.com)

## License

MIT
