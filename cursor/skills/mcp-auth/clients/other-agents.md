# Other agents connection (no plugin)

Applies to any agent without a Datagate plugin, such as OpenCode, Gemini CLI, GitHub Copilot CLI, VS Code and Windsurf. It also works for Claude Code, Codex and Cursor if the user does not want a plugin.

This setup uses two separate CLIs, both of which need Node.js:

- [`npx skills`](https://github.com/vercel-labs/skills) installs the Datagate skills.
- [`npx add-mcp`](https://github.com/neondatabase/add-mcp) registers the Datagate MCP server.

Both detect installed agents and ask which ones to target; `-a <agent>` picks one directly, and `-g` installs for the user rather than the project.

## Install the Datagate skills

```sh
npx skills add datagate-inc/agent-plugins -g
```

Run `npx skills update` later to pull new versions.

## Add the Datagate MCP server

```sh
npx add-mcp "https://mcp.dgportal.net/mcp" --name datagate -g
```

Agents that support MCP sign-in (OAuth) prompt the user to sign in with their Datagate portal login when the server connects.

### Agents without sign-in support

The user adds an `Authorization: Bearer <api-key>` header to the `datagate` entry in the agent's own MCP settings, with a key from the portal's **Settings → API Keys**. The user enters the key there themselves so it stays out of the chat and shell history. Prefer user scope (`-g`) for a key, so the config file holding it is not in a project that could be committed.

## Confirmation

The Datagate skills are readable and the Datagate MCP server lists tools. The user may need to restart their agent first.
