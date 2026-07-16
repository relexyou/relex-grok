# Security

Relex lets you use Grok on legal matters **without exposing client PII**.

## Authentication

- MCP: `https://relex.you/api/mcp`
- OAuth 2.1 + PKCE when the host supports browser auth
- Relex API key as `authorization` / bearer for xAI remote MCP and CI
- xAI API keys are separate from Relex credentials

## Client PII never reaches the model

- Parties sealed client-side under PII password
- Documents redacted client-side by default
- MCP `execute` blocks plaintext party/document endpoints

## Reporting

**security@relex.you**
