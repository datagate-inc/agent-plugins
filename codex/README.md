# Datagate plugin for ChatGPT and Codex

Connects ChatGPT and Codex to the Datagate MCP server (`https://mcp.dgportal.net/mcp`), so the model can find customers, read invoices, and manage products, agreements and rating in Datagate.

## Install

The repository root is a Codex plugin marketplace. Add it, then install the plugin:

```
codex plugin marketplace add datagate-inc/agent-plugins
codex plugin add datagate@datagate
```

For a local checkout, pass the checkout path to `codex plugin marketplace add` instead. Start a new Codex session after installing so the skills and MCP server load.

## Authentication

Codex asks you to sign in with your Datagate portal login when the server first connects (or run `codex mcp login datagate`). Pick the company and choose read only or read and change.

To use a public API key instead, register the server yourself with the key read from an environment variable. Codex removes `Authorization` headers from plugin MCP configs, so this can't live in the plugin:

```
export DATAGATE_API_KEY=…
codex mcp add datagate --url https://mcp.dgportal.net/mcp --bearer-token-env-var DATAGATE_API_KEY
```

Your `datagate` entry replaces the plugin's. The skills keep working because the server name is unchanged. Create the key in the Datagate portal under **Settings → API Keys**.

## Skills

| Skill | What it does |
| --- | --- |
| `working-with-datagate` | What one connection reaches, finding records by name, search filters and paging, full-replace updates, and which calls need confirmation |
| `mcp-auth` | Installs the plugin and MCP server, and signs in when a call fails authentication |
