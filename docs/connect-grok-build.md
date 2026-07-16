# Connect Relex in Grok Build / local hosts

## MCP config

```json
{
  "mcpServers": {
    "relex": {
      "type": "http",
      "url": "https://relex.you/api/mcp"
    }
  }
}
```

API-key variant:

```json
{
  "mcpServers": {
    "relex": {
      "type": "http",
      "url": "https://relex.you/api/mcp",
      "headers": {
        "Authorization": "Bearer rlx_..."
      }
    }
  }
}
```

## Skills

Copy or symlink `plugin/skills/` into your Grok skills directory (for example
`~/.grok/skills/relex/`) so Grok auto-loads the PII-safe workflow. Primary skill
is `relex/SKILL.md`.

## First prompt

> Set up my practice workflow with Relex

Complete browser OAuth if prompted, then follow deep links for PII password and
know-how.
