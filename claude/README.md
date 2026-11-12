# Datagate plugin for Claude

Bundles:

- **Datagate skills** that teach Claude how to work in Datagate: finding records by name, safe updates, and which calls need confirmation.
- **The Datagate MCP server** (`https://mcp.dgportal.net/mcp`), through which Claude reads and changes Datagate data with your portal permissions.

## Installation (Claude Code)

The **Code** tab of the Claude Desktop app is Claude Code, so these steps apply there too.

1. Add the marketplace: `/plugin marketplace add datagate-inc/agent-plugins`
2. Install the plugin: `/plugin install datagate@datagate`
3. Restart Claude Code so the MCP server starts.
4. Run `/mcp`, select **datagate** and choose **Authenticate**.

## Installation (Claude Desktop Chat, Cowork, claude.ai)

Plugins need a paid plan (Pro, Max, Team or Enterprise).

1. Click **Customize** in the left sidebar, then **Plugins**. In Cowork, open the **Cowork** tab first.
2. Under **Personal plugins**, click **+** → **Add marketplace** and enter `datagate-inc/agent-plugins`.
3. Find **datagate** and click **Install**.
4. Open the plugin's **Connectors** tab, click **Connect** on the Datagate connector, and sign in.

On Team and Enterprise plans an owner may have turned off personal marketplaces. Ask them to add this marketplace under **Organization settings → Plugins**.

## Authentication

You sign in with your Datagate portal login, pick the company to connect, and choose **read only** or **read and change**. Claude then has exactly your portal permissions in that company. To connect another company, sign in again.

Your company's administrator must turn on **Allow MCP connections** for the brand first. SSO-only brands can't sign in this way; use a public API key instead, which the `mcp-auth` skill walks you through adding in Claude's own MCP settings.

## Skills

| Skill | What it does |
| --- | --- |
| `working-with-datagate` | What one connection reaches, finding records by name, search filters and paging, full-replace updates, and which calls need confirmation |
| `mcp-auth` | Installs the plugin and MCP server, and signs in when a call fails authentication |
