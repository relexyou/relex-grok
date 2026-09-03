# Grok Build test notes (2026-07-16)

Verified on a developer machine with Grok Build CLI.

## Steps run

```bash
# 1. Validate package
grok plugin validate ./plugin
# → Plugin manifest is valid. name: relex-legal v0.1.1

# 2. Install + enable
grok plugin install ./plugin --trust
# or: grok plugin install relexlegal/relex-grok#plugin --trust
grok plugin enable relex-legal

# 3. Register MCP (if not already from plugin)
grok mcp add --transport http relex https://relex.legal/api/mcp

# 4. Inspect
grok plugin details relex-legal
grok inspect
# → relex-legal: 10 skills, 1 agent, MCP
# → skills: relex, relex-steering, relex-counsel, relex-ontology, …

# 5. Doctor
grok mcp doctor relex
# → server started (HTTP OK)
# → handshake fails until OAuth completes (expected)
```

## Endpoint checks

| Check | Result |
|-------|--------|
| `POST https://relex.legal/api/mcp` unauthenticated | `401` + `WWW-Authenticate: Bearer resource_metadata=…` |
| `GET /.well-known/oauth-protected-resource` | OK — resource `https://relex.legal/api/mcp` |
| `GET /.well-known/oauth-authorization-server` | OK — authorize + token + register |
| Dynamic client registration | OK — issued `client_id` |

## What you must do once in the TUI

OAuth is interactive. In Grok Build:

1. Run `/mcps`
2. Select **relex**
3. Press **`i`** to authenticate
4. Complete Google/Apple sign-in in the browser

After that, `grok mcp doctor relex` should report healthy tools `search` and
`execute` (namespaced as `relex__search` / `relex__execute`).

API-key alternative (no browser):

```bash
grok mcp add --transport http relex https://relex.legal/api/mcp \
  --header "Authorization: Bearer rlx_..."
```

## First real prompt after auth

> Set up my practice workflow with Relex

Expected: onboarding status via `execute`, deep links for PII password / knowledge.
