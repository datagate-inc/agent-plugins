# Cursor connection

Applies to the Cursor IDE, Cursor web, and the Cursor CLI (`agent`). A plugin installed at user scope in one is available in the others.

## Authentication uses an API key

The Cursor plugin authenticates with a Datagate public API key, not the portal sign-in, so offer the key without waiting for the user to ask.

- The key is created in the Datagate portal under **Settings → API Keys**. A key can be made read only.
- A key reaches only the company it belongs to. To work on another company, the user replaces the key under the plugin's **Configure**.
- The user enters the key in Cursor themselves. Never ask for it in the chat.

## Install the Datagate plugin

Until the plugin is listed in Cursor's marketplace, a team adds this repository as a team marketplace (Teams and Enterprise plans):

1. Open **Dashboard → Plugins**.
2. Under **Team Marketplaces**, click **Add Marketplace**, then **Import from Repo**.
3. Enter `datagate-inc/agent-plugins`.

Then, in Cursor:

1. Open **Customize** in the sidebar and find **Datagate**.
2. Select **Add**, choosing user or project scope.
3. Set the **Datagate API key** when prompted, or later by opening the plugin and selecting **Configure**.

In the Cursor CLI, run `/plugin`, cycle to **Marketplace**, search for "datagate" and install it. The CLI cannot set plugin variables, so set the key in the Cursor app (Datagate plugin → **Configure**).

A user-level `datagate` entry in `~/.cursor/mcp.json` overrides the plugin's headers if both exist. Prefer **Configure**.

## Confirmation

The Datagate plugin is installed, its skills are available, and its MCP server lists tools. The user may need to reload the window or start a new session first.
