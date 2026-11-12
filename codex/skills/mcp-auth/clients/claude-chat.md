# Claude Chat connection

Applies to the Chat and Cowork tabs of the Claude Desktop App, claude.ai, and Claude Code cloud agents (installed through the UI). For Claude Code in the CLI or the desktop Code tab, read `./claude-code.md` instead.

Claude cannot install the plugin on these surfaces. Walk the user through it.

## Install the Datagate plugin

1. Open **Customize** in the left sidebar, then **Plugins**.
2. Under **Personal plugins**, click **+** → **Add marketplace**, and enter `datagate-inc/agent-plugins`.
3. Find **datagate** in the list and click **Install**.
4. Open the plugin's **Connectors** tab and click **Connect** on the Datagate connector. Keep the default settings, sign in with the Datagate portal login, pick the company, and choose read only or read and change.

Plugins need a paid Claude plan. On Team and Enterprise plans an owner may have turned off personal marketplaces; the owner then adds `datagate-inc/agent-plugins` under **Organization settings → Plugins**.

## Confirmation

The Datagate plugin is installed, its skills are available, and the connector shows as connected.
