# Relex × Grok (xAI)

Let a lawyer's **Grok** operate inside a **Relex** legal case — **without ever
receiving PII**.

> Relex doesn't replace Grok. It helps you use Grok end-to-end by protecting
> your PII data and know-how, automating customer service, handling payments for
> free, and giving you access to a new market. See
> [`docs/positioning.md`](docs/positioning.md).

Downstream of the shared Relex MCP server. Base package:
[relexyou/relex-mcp](https://github.com/relexyou/relex-mcp). Sibling connectors:
[Claude](https://github.com/relexyou/relex-claude) ·
[GPT](https://github.com/relexyou/relex-gpt) ·
[Gemini](https://github.com/relexyou/relex-gemini).

## How it works

Grok connects to the **remote MCP server** at `https://relex.you/api/mcp` via
xAI **Remote MCP Tools** (Streaming HTTP / SSE). Two tools — `search` and
`execute` — with a fixed ~1k-token footprint. Auth: **OAuth** when the host
drives browser sign-in, or **Authorization bearer** (Relex API key) for API /
headless use.

Party data is sealed client-side; documents are redacted client-side by
default; `execute` refuses plaintext PII and returns deep links.

## Quick start

### xAI API (Responses / native SDK)

```python
from xai_sdk import Client
from xai_sdk.tools import mcp
import os

client = Client(api_key=os.getenv("XAI_API_KEY"))
chat = client.chat.create(
    model="grok-4.5",
    tools=[
        mcp(
            server_url="https://relex.you/api/mcp",
            server_label="relex",
            server_description="Relex legal case management — PII-safe MCP",
            # For headless: pass a Relex API key
            # authorization=os.getenv("RELEX_API_KEY"),
        ),
    ],
)
```

OpenAI-compatible Responses shape:

```json
{
  "type": "mcp",
  "server_url": "https://relex.you/api/mcp",
  "server_label": "relex",
  "server_description": "Relex legal case management — PII-safe MCP"
}
```

With Relex API key (Settings → API Keys):

```json
{
  "type": "mcp",
  "server_url": "https://relex.you/api/mcp",
  "server_label": "relex",
  "authorization": "rlx_..."
}
```

### Grok Build (recommended)

```bash
grok plugin install relexyou/relex-grok#plugin --trust
grok plugin enable relex-legal
# Authenticate MCP (browser OAuth) via /mcps → relex → i, or:
grok mcp add --transport http relex https://relex.you/api/mcp
```

Or MCP-only:

```toml
# ~/.grok/config.toml
[mcp_servers.relex]
url = "https://relex.you/api/mcp"
enabled = true
```

Full guide: [`docs/connect-grok-build.md`](docs/connect-grok-build.md).

## Personal vs Team

| Context | Who configures MCP | Who authenticates to Relex |
|---------|--------------------|----------------------------|
| **Individual** xAI API key / personal Grok | You add the remote MCP tool in your app or Grok Build config | You — OAuth or your Relex API key |
| **Team / org** shared Grok or xAI workspace | Admin publishes the Relex MCP config / secrets policy | Each user should use **their own** Relex OAuth or key; do not share PII passwords |

If your product surfaces **Connectors** with admin install (similar to Claude /
ChatGPT Team): admin installs once; members only click **Connect**.

Full guide: [`docs/install.md`](docs/install.md).

## Layout

```
relex-grok/
├── plugin/
│   ├── .mcp.json
│   ├── plugin.json
│   ├── skills/
│   ├── agents/
│   ├── commands/
│   └── references/
├── docs/
│   ├── install.md
│   ├── connect-xai-api.md
│   ├── connect-grok-build.md
│   └── positioning.md
└── SECURITY.md
```

## Docs on relex.you

- [Grok connector](https://relex.you/docs/connectors/grok)
- [MCP Server](https://relex.you/docs/mcp)
- [For AI Agents](https://relex.you/for-agents)

## License

AGPL-3.0-or-later — see [LICENSE](LICENSE).
