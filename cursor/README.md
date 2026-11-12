# Datagate

Cursor plugin that connects agents to [Datagate](https://dgportal.net) through the Datagate [Model Context Protocol](https://modelcontextprotocol.io/) server at `https://mcp.dgportal.net/mcp`.

Find customers, read invoices and billing periods, and manage products, agreements, rate cards and service items without leaving the editor.

## Install

Until the plugin is listed in Cursor's marketplace, add this repository as a team marketplace (Teams and Enterprise plans):

1. Open **Dashboard → Plugins**.
2. Under **Team Marketplaces**, click **Add Marketplace**, then **Import from Repo**.
3. Enter `datagate-inc/agent-plugins`.

Then open **Customize** in Cursor's sidebar, find **Datagate**, select **Add**, and set your **Datagate API key** when prompted.

## MCP

```json
{
  "mcpServers": {
    "datagate": {
      "headers": {
        "Authorization": "Bearer ${DATAGATE_API_KEY}"
      },
      "type": "http",
      "url": "https://mcp.dgportal.net/mcp"
    }
  }
}
```

`${DATAGATE_API_KEY}` is a plugin variable. Cursor substitutes the value you set at install, or later under **Plugins → Configure**. It is not a shell environment variable.

## Authentication

Create a key in the Datagate portal under **Settings → API Keys**. Paste it into the install prompt, or open **Plugins → Configure** on the installed plugin and set **Datagate API key**. Delete and recreate the key in the portal if it is ever exposed.

A key reaches only the company it belongs to. It gets every tool, or only the read tools if you made the key read only. A user-level `datagate` entry in `~/.cursor/mcp.json` overrides the plugin's headers if both exist; prefer **Configure**.

## What agents can do

| Toolset | Covers |
| --- | --- |
| `core` | customers, customer portal users, sites |
| `billing` | invoices, billing periods, agreements, product transactions |
| `catalog` | products and their charges, product templates, kits, rate cards, service items |
| `admin` | invoice delivery methods, account managers, rating assignments |

Updates replace the whole record, and deletes and billing changes need your confirmation; the `working-with-datagate` skill keeps the agent to both.

## Skills

| Skill | What it does |
| --- | --- |
| `working-with-datagate` | What one connection reaches, finding records by name, search filters and paging, full-replace updates, and which calls need confirmation |
| `mcp-auth` | Sets up the API key and diagnoses authentication failures |

## License

MIT
