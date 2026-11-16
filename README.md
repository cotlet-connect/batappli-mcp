# Batappli MCP server

Connect Batappli to ChatGPT, Claude and any AI agent through Cotlet.dev. Work with clients, draft quotes, invoices and unpaid balances in your own signed-in Batappli en ligne account. Sharing quotes, recording deposits and payments, converting quotes to invoices and sending reminders ask for approval by default.

## What you can ask

> List unpaid invoices and the days late for each client.

> Draft a quote for a water heater installation and show me the total before sending.

> Read revenue and gross margin for last week.

## Connect in 2 minutes

Request access at https://cotlet.dev/signup?app=batappli. ChatGPT and Claude use the connector sign-in flow. For editors and scripts, make an API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Codex

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${env:COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Batappli tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 batappli_agent.py serve
```

Or with Docker:

```bash
docker build -t batappli-mcp .
docker run -i --rm -e COTLET_API_KEY batappli-mcp
```

Tools: `batappli_guide`, `batappli_status`, `batappli_open`, `batappli_read`, `batappli_click`, `batappli_type`, `batappli_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 batappli_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 batappli_agent.py status    # is Batappli ready in your Cotlet.dev account
python3 batappli_agent.py guide     # Batappli screens and sign-in link
python3 batappli_agent.py read      # open Batappli, read the page, close the browser
python3 batappli_agent.py tools     # list Cotlet.dev tools
python3 batappli_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Batappli users

**Which Batappli version can I connect?**

Use Batappli en ligne at batappli.app. Installed desktop versions are outside this browser integration.

**Which actions ask for approval?**

Sharing quotes, recording deposits and payments, converting quotes to invoices and sending payment reminders ask first by default. Reads, new clients and draft quotes run without that approval.

**Is Cotlet affiliated with Batappli?**

No. Cotlet.dev is independent and not affiliated with Batappli.

## Learn more

- [Batappli integration and hosted tools](https://cotlet.dev/integrations/batappli-api-mcp-server)

---

More: https://cotlet.dev/integrations/batappli-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=batappli

Request access: https://cotlet.dev/signup?app=batappli&utm_source=github&utm_medium=readme&utm_campaign=batappli

## Workflows on the hosted integration

The live integration page documents these 14 operations:

- `list_clients`
- `get_client`
- `create_client`
- `list_quotes`
- `get_quote`
- `create_quote`
- `share_quote`
- `list_invoices`
- `record_deposit`
- `convert_quote_to_invoice`
- `record_payment`
- `list_unpaid_invoices`
- `send_payment_reminder`
- `get_dashboard`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
