# Fluenta MCP server

Remote MCP server that scores business ideas on six live market signals. Run an Idea X-Ray from Claude, Cursor or any MCP client and get a Launch Readiness Score (LRS) across demand, pain, competition, monetisation, timing and distribution, plus search over thousands of already-scored ideas.

- Endpoint: `https://fluenta.space/backend/api/v1/mcp` (Streamable HTTP, JSON-RPC 2.0)
- Docs: https://fluenta.space/docs/api-and-mcp
- OpenAPI: https://fluenta.space/backend/api/v1/ext/openapi.json
- Website: https://fluenta.space

This repository holds the registry manifest (`server.json`) and the integration notes. The server itself is hosted by Fluenta; there is nothing to install or run locally.

## Tools

| Tool | What it does |
|---|---|
| `fluenta_idea_x-ray_sandbox` | Instant preview of an idea's Launch Readiness Score (LRS). No credits, no writes. Use it as the default first look. |
| `fluenta_idea_x-ray` | The full X-Ray. Scores the idea on six live signals. Returns `xray_submitted` while queued and `xray_result` when complete. |
| `fluenta_search_ideas`, `fluenta_get_idea`, `fluenta_compare_ideas` | Search and compare the scored-ideas database. |
| `fluenta_list_collections`, `fluenta_get_pipeline`, `fluenta_add_to_pipeline`, `fluenta_remove_from_pipeline` | Manage your idea pipeline. |
| `fluenta_list_analyses`, `fluenta_list_xray_runs`, `fluenta_download_report` | History and reports. |
| `fluenta_get_account`, `fluenta_get_usage` | Account state and usage. |

## Authentication

Create an API key in your Fluenta account (Settings, API keys). Two scopes:

- `read`: search ideas, read analyses, list collections, read pipeline and usage.
- `read_write`: everything in `read`, plus submit X-Ray analyses and add or remove pipeline bookmarks.

Send the key as `Authorization: Bearer YOUR_FLUENTA_API_KEY`.

## Claude Desktop

Add to `claude_desktop_config.json` and restart Claude:

```json
{
  "mcpServers": {
    "fluenta": {
      "type": "http",
      "url": "https://fluenta.space/backend/api/v1/mcp",
      "headers": { "Authorization": "Bearer YOUR_FLUENTA_API_KEY" }
    }
  }
}
```

Config location: `~/Library/Application Support/Claude/` on macOS, `%APPDATA%\Claude\` on Windows.

## Cursor

Same JSON in `~/.cursor/mcp.json`, or use the one-click install button on the docs page.

## Claude Code

```bash
claude mcp add --transport http fluenta https://fluenta.space/backend/api/v1/mcp --header "Authorization: Bearer YOUR_FLUENTA_API_KEY"
```

## Example prompts

- "Run a sandbox X-Ray on: a subscription service for restaurant menu translation."
- "Search Fluenta for scored ideas about AI bookkeeping and compare the top three."
- "Add the best result to my pipeline."

## Errors and limits

Standard HTTP codes. 401 is a bad or revoked key, 402 is out of credits or key limit exceeded, 403 is the wrong scope for the action, 429 is rate limited (respect `Retry-After`). The response envelope on failure is `{ "success": false, "error": { ... } }`.

## Registry

Published to the official MCP Registry as `io.github.Fluenta-Team/fluenta` from the `server.json` in this repository.

## Licence

MIT for the contents of this repository. Use of the hosted service is governed by the Fluenta Terms of Service at https://fluenta.space/terms-of-service.
