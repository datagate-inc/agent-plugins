# Claude Code connection

Applies to the Claude Code CLI and the Code tab in the Claude Desktop App. For Claude Code cloud agents, the Chat interface in the Claude Desktop App, or claude.ai, read `./claude-chat.md` instead.

These commands can be run by Claude Code, or the user can run them in their terminal.

## Install the Datagate plugin

Add the Datagate marketplace, then install the plugin from it.

### User scope (recommended)

Available to the user in every project.

```sh
claude plugin marketplace add datagate-inc/agent-plugins
claude plugin install datagate@datagate --scope user
```

### Project scope

The plugin is enabled in the project's `.claude/settings.json`, which is committed so that other contributors are prompted to install it. Nothing secret is committed: each contributor signs in with their own portal login.

```sh
claude plugin marketplace add datagate-inc/agent-plugins --scope project
claude plugin install datagate@datagate --scope project
```

## Sign in

Run `/mcp`, select **datagate**, and choose **Authenticate**. A browser opens on the Datagate portal sign-in. The user signs in, picks the company, and chooses read only or read and change.

## Using an API key instead

The user adds their own server entry with the key, then disables the plugin's `datagate` server under `/mcp` so the two don't duplicate. Have the user run this in their own terminal, with the key held in an environment variable so it stays out of the chat and shell history:

```sh
claude mcp add --transport http --scope user datagate-key https://mcp.dgportal.net/mcp --header "Authorization: Bearer $DATAGATE_API_KEY"
```

## Restarting

The user may have to restart Claude Code for the MCP server to load. Recommend they quit and resume this same session (`claude --continue`) rather than start a new conversation.

## Confirmation

The Datagate skills are readable and the `datagate` MCP server lists its tools.
