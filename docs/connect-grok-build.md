# Connect Relex in Grok Build

Grok Build supports plugins (skills + MCP) and direct MCP server config.

## Option A — Install the plugin (recommended)

Installs skills, slash commands, and the remote MCP connector together.

```bash
# From GitHub (plugin lives under plugin/)
grok plugin install relexyou/relex-grok#plugin --trust

# Or from a local clone
grok plugin install /path/to/relex-grok/plugin --trust

# Enable if not already active
grok plugin enable relex-legal
```

Confirm:

```bash
grok plugin details relex-legal
grok mcp list
grok mcp doctor relex
```

If the MCP server needs auth, open `/mcps` in the TUI and press **`i`** on
**relex** to run the browser OAuth flow (or use an API key — see below).

## Option B — MCP only (no plugin skills)

```bash
grok mcp add --transport http relex https://relex.you/api/mcp
```

Equivalent `~/.grok/config.toml`:

```toml
[mcp_servers.relex]
url = "https://relex.you/api/mcp"
enabled = true
```

API-key variant (Relex → **Settings → API Keys**):

```bash
grok mcp add --transport http relex https://relex.you/api/mcp \
  --header "Authorization: Bearer rlx_..."
```

```toml
[mcp_servers.relex]
url = "https://relex.you/api/mcp"
enabled = true
headers = { Authorization = "Bearer ${RELEX_API_KEY}" }
```

## Skills only (without plugin install)

Symlink or copy `plugin/skills/*` into `~/.grok/skills/` (or project
`.grok/skills/`). Primary skill: `relex/SKILL.md`.

## Project-scoped config

Commit a team-shared MCP entry:

```toml
# .grok/config.toml
[mcp_servers.relex]
url = "https://relex.you/api/mcp"
enabled = true
```

## First prompt

> Set up my practice workflow with Relex

Complete browser OAuth if prompted, then follow deep links for PII password and
know-how. Tools appear as `relex__search` and `relex__execute` (namespaced).
