# ChatGPT & Codex connection

Applies to ChatGPT web and desktop, Codex in the ChatGPT desktop app, and the Codex CLI. The Codex CLI and the ChatGPT desktop app share `~/.codex/config.toml`, so configuring one configures both.

## Running the `codex` commands

Offer to run these for the user; they only need to approve the change to `~/.codex/config.toml`. They can also run them in their own terminal.

The ChatGPT desktop app bundles its own `codex` binary. If `codex` is not on the PATH, or `codex plugin` is not recognised (an outdated Codex CLI), use:

```sh
/Applications/ChatGPT.app/Contents/Resources/codex-cli/bin/codex
```

## Install the Datagate plugin

ChatGPT web cannot install plugins from a marketplace yet.

For the Codex CLI and the ChatGPT desktop app:

```sh
codex plugin marketplace add https://github.com/datagate-inc/agent-plugins.git
codex plugin add datagate@datagate
```

If the `codex` commands cannot be run, add this to `~/.codex/config.toml` instead:

```toml
[marketplaces.datagate]
source_type = "git"
source = "https://github.com/datagate-inc/agent-plugins.git"

[plugins."datagate@datagate"]
enabled = true
```

## Sign in

Codex asks the user to sign in when the Datagate server first connects. If it doesn't, run `codex mcp login datagate`. The user signs in with their portal login, picks the company, and chooses read only or read and change.

## Using an API key instead

Codex removes `Authorization` headers from plugin MCP configs, so a key needs the user's own server entry. Have the user run this in their terminal with the key in an environment variable:

```sh
codex mcp add datagate --url https://mcp.dgportal.net/mcp --bearer-token-env-var DATAGATE_API_KEY
```

Using the name `datagate` replaces the plugin's entry, so the skills keep working.

## Confirmation

The Datagate plugin, its skills, and the Datagate tools are visible. The user may have to restart the app or start a new Codex session first.
