# OpenClaw Changelog Reference

Detailed changelog for recent OpenClaw releases.

## v2026.2.1 (Feb 2, 2026) — Latest

### Changes
- Docs: onboarding/install/i18n/exec-approvals/Control UI/exe.dev/cacheRetention updates
- Telegram: use shared pairing store
- Agents: add OpenRouter app attribution headers
- Agents: add system prompt safety guardrails
- Agents: update pi-ai to 0.50.9, rename cacheControlTtl → cacheRetention
- Agents: extend CreateAgentSessionOptions with systemPrompt/skills/contextFiles
- Agents: add tool policy conformance snapshot
- Auth: update MiniMax OAuth hint + portal auth note copy
- Discord: inherit thread parent bindings for routing
- Gateway: inject timestamps into agent and chat.send messages
- Gateway: require TLS 1.3 minimum for TLS listeners
- Web UI: refine chat layout + extend session active duration
- CI: add formal conformance + alias consistency checks

### Fixes
- Plugins: validate plugin/hook install paths and reject traversal-like names
- Telegram: add download timeouts for file fetches
- Telegram: enforce thread specs for DM vs forum sends
- Streaming: flush block streaming on paragraph boundaries
- Streaming: stabilize partial streaming filters
- Auto-reply: avoid referencing workspace files in /new greeting prompt
- Tools: align tool execute adapters/signatures
- Tools: treat "*" tool allowlist entries as valid
- Skills: update session-logs paths from .clawdbot to .openclaw
- Slack: harden media fetch limits and Slack file URL validation
- Memory search: L2-normalize local embedding vectors to fix semantic search
- Agents: align embedded runner + typings with pi-coding-agent API updates
- Agents: cap context window resolution for compaction safeguard
- System prompt: resolve overrides using session_status for current date/time
- Subagents: fix announce failover race
- Telegram: restore draft streaming partials
- Onboarding: friendlier Windows onboarding message
- Browser: secure Chrome extension relay CDP sessions
- Docker: use container port for gateway command instead of host port

### Security Fixes
- fix(lobster): block arbitrary exec via lobsterPath/cwd injection (GHSA-4mhr-g7xj-cg8j)
- Security: sanitize WhatsApp accountId to prevent path traversal
- Security: restrict MEDIA path extraction to prevent LFI
- Security: validate message-tool filePath/path against sandbox root
- Security: block LD*/DYLD* env overrides for host exec
- Security: harden web tool content wrapping + file parsing safeguards
- Security: enforce Twitch allowFrom allowlist gating

---

## v2026.1.30 (Jan 31, 2026)

### Changes
- CLI: add `completion` command (Zsh/Bash/PowerShell/Fish) + auto-setup during postinstall/onboarding
- CLI: add `models status` per-agent filter (--agent)
- Agents: add Kimi K2.5 to synthetic model catalog
- Auth: switch Kimi Coding to built-in provider; normalize OAuth profile email
- Auth: add MiniMax OAuth plugin + onboarding option
- Agents: update pi SDK/API usage and dependencies
- Web UI: refresh sessions after chat commands + improve session display names
- Build: move TypeScript builds to tsdown + tsgo (faster builds)
- Docs: add pi/pi-dev docs and update OpenClaw branding + install links

### Fixes
- Security: restrict local path extraction in media parser to prevent LFI
- Gateway: prevent token defaults from becoming literal "undefined"
- Control UI: fix assets resolution for npm global installs
- macOS: avoid stderr pipe backpressure in gateway discovery
- Telegram: normalize account token lookup for non-normalized IDs
- Telegram: preserve delivery thread fallback and fix threadId handling
- Telegram: fix HTML nesting for overlapping styles/links
- Telegram: accept numeric messageId/chatId in react actions
- Telegram: honor per-account proxy dispatcher via undici fetch
- Telegram: scope skill commands to bound agent per bot
- BlueBubbles: debounce by messageId to preserve attachments
- Routing: prefer requesterOrigin over stale session entries
- OAuth: skip expired-token warnings when refresh tokens are still valid

---

## v2026.1.29 (Jan 29, 2026)

### ⚠️ BREAKING CHANGE
- **Gateway auth mode "none" is removed** — Gateway now requires token/password (Tailscale Serve identity still works as an alternative)

### Changes
- **Rebrand**: Rename npm package/CLI from `clawdbot` to `openclaw`, extensions to `@openclaw/*` scope
- Onboarding: strengthen security warning copy for beta + access control expectations
- Config: auto-migrate legacy state/config paths
- Gateway: warn on hook tokens via query params; document header auth preference
- Gateway: add dangerous Control UI device auth bypass flag + audit warnings
- Doctor: warn on gateway exposure without auth
- Web UI: keep sub-agent announce replies visible in WebChat
- Browser: route browser control via gateway/node
- Browser: route via node proxies when available; honor proxy timeouts
- Telegram: allow caption param for media sends
- Telegram: support plugin sendPayload channelData (media/buttons)
- Telegram: avoid block replies when streaming is disabled
- Telegram: add optional silent send flag (disable notifications)
- Telegram: support editing sent messages via message(action="edit")
- Telegram: support quote replies for message tool and inbound context
- Telegram: add sticker receive/send with vision caching
- Telegram: send sticker pixels to vision models
- Discord: add configurable privileged gateway intents for presences/members
- Slack: clear ack reaction after streamed replies
- Matrix: switch plugin SDK to @vector-im/matrix-bot-sdk
- Tools: add per-sender group tool policies and fix precedence
- Agents: summarize dropped messages during compaction safeguard pruning
- Agents: expand cron tool description with full schema docs
- Memory Search: allow extra paths for memory indexing
- Skills: add multi-image input support to Nano Banana Pro skill
- Commands: group /help and /commands output with Telegram paging
- Routing: add per-account DM session scope
- CLI: use Node's module compile cache for faster startup
- macOS: finish OpenClaw app rename for bundle identifiers
- Branding: update launchd labels, mobile bundle IDs to bot.molt

### Docs Added
- Migration guide for moving to a new machine
- Northflank one-click deployment guide
- Vercel AI Gateway provider guide
- Render deployment guide
- Claude Max API Proxy guide
- DigitalOcean deployment guide
- Oracle Cloud (OCI) platform guide
- Raspberry Pi install guide
- GCP Compute Engine deployment guide
- LINE channel guide

### Major Fixes
- Telegram: avoid silent empty replies by tracking normalization skips
- Mentions: honor mentionPatterns even when explicit mentions are present
- Discord: restore username directory lookup in target resolution
- Agents: prevent retries on oversized image errors
- Gateway: prevent crashes on transient network errors
- Security: pin npm overrides to keep tar@7.5.4
- Security: harden Tailscale Serve auth by validating identity via local tailscaled
- Media: fix text attachment MIME misclassification with CSV/TSV inference

---

## Configuration Examples

### Multi-Account WhatsApp

```json5
{
  channels: {
    whatsapp: {
      accounts: {
        personal: {},
        business: {},
      },
    },
  },
  bindings: [
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "work", match: { channel: "whatsapp", accountId: "business" } },
  ],
}
```

### Family Agent with Restricted Tools

```json5
{
  agents: {
    list: [{
      id: "family",
      name: "Family",
      workspace: "~/.openclaw/workspace-family",
      groupChat: {
        mentionPatterns: ["@family", "@familybot"],
      },
      sandbox: { mode: "all", scope: "agent" },
      tools: {
        allow: ["exec", "read", "sessions_list", "sessions_history"],
        deny: ["write", "edit", "apply_patch", "browser", "canvas", "nodes", "cron"],
      },
    }],
  },
  bindings: [{
    agentId: "family",
    match: {
      channel: "whatsapp",
      peer: { kind: "group", id: "120363999999999999@g.us" },
    },
  }],
}
```

### Peer-Specific Routing

```json5
{
  bindings: [
    // Specific DM goes to opus agent
    { agentId: "opus", match: { channel: "whatsapp", peer: { kind: "dm", id: "+15551234567" } } },
    // All other WhatsApp goes to chat agent
    { agentId: "chat", match: { channel: "whatsapp" } },
  ],
}
```
