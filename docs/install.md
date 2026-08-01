# Install Relex for Grok

**MCP URL:**

```
https://relex.legal/api/mcp
```

## Paths

| Surface | Doc |
|---------|-----|
| xAI API (Responses / native SDK / Voice Agent) | [connect-xai-api.md](connect-xai-api.md) |
| Grok Build / local MCP hosts | [connect-grok-build.md](connect-grok-build.md) |

## Auth options

1. **OAuth (preferred for interactive hosts)** — first tool call opens browser
   sign-in to Relex (Google/Apple). No key paste.
2. **Relex API key** — Settings → API Keys → Create key. Pass as
   `authorization` on the remote MCP tool or `Authorization: Bearer rlx_...`
   header. Best for CI and pure API use.

You still need an **xAI API key** for the Grok model itself; that is separate
from the Relex credential.

## Personal vs Team

### Individual developer / solo lawyer

1. Add remote MCP tool with `server_url=https://relex.legal/api/mcp`,
   `server_label=relex`.
2. Complete OAuth or attach your Relex API key.
3. Prompt: *“Set up my practice workflow with Relex.”*

### Team / organisation

| Role | Responsibility |
|------|----------------|
| **Admin** | Approve use of remote MCP; distribute the Relex server URL and (if required) a pattern for per-user Relex keys or OAuth. Never put a shared PII password in env. |
| **Member** | Configure their own client with the Relex MCP URL and **their** Relex OAuth/key. |

When a Grok product offers workspace **Connectors** with admin-only install
(like ChatGPT Team / Claude Team):

1. Admin adds the custom connector once.
2. Members open Settings → Connectors, find Relex, click **Connect**.

## After connect

Same practice workflow as other connectors:

1. PII password (browser)
2. Know-how upload
3. Encrypted parties (counts only to Grok)
4. First case

## Security

See [SECURITY.md](../SECURITY.md). Grok only receives de-identified labels and
structure — never names, national IDs, or document plaintext.
