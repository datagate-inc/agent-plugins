# Datagate agent plugins

Plugins that connect AI agents to the [Datagate](https://dgportal.net) MCP server at `https://mcp.dgportal.net/mcp`, so an agent can find customers, read invoices and billing periods, and manage products, agreements, rate cards and service items, within the permissions of the user or API key behind the connection.

There is one plugin per client, because each client has its own manifest format and its own way of handling credentials.

| Client | Plugin | Marketplace manifest | Install | Signs in with |
| --- | --- | --- | --- | --- |
| Claude | [`claude/`](claude/) | `.claude-plugin/marketplace.json` | `/plugin marketplace add datagate-inc/agent-plugins` | Portal login |
| ChatGPT and Codex | [`codex/`](codex/) | `.agents/plugins/marketplace.json` | `codex plugin marketplace add datagate-inc/agent-plugins` | Portal login |
| Cursor | [`cursor/`](cursor/) | `.cursor-plugin/marketplace.json` | **Dashboard → Plugins → Team Marketplaces → Import from Repo** | API key |

Each plugin directory has its own README with install steps and authentication.

## Authentication

Every client uses the same URL, `https://mcp.dgportal.net/mcp`, in every region.

- **Portal login** (Claude, ChatGPT, Codex). The user signs in through Passport, picks the company, and chooses read only or read and change. The connection gets the user's own portal permissions. The brand must have **Allow MCP connections** turned on, and SSO-only brands must use an API key.
- **Public API key** (Cursor, or any client on request). Created in the portal under **Settings → API Keys** and sent as `Authorization: Bearer <key>`. A key reaches only its own company, and gets every tool, or only the read tools if the key is read only.

The plugins never read credentials from your machine, and no key is stored in this repository.

## Repository structure

The repository root is a marketplace for all three clients; it is not itself a plugin. Each client reads only its own marketplace manifest and ignores the others.

```
agent-plugins/
├── .claude-plugin/marketplace.json   # Claude marketplace  → ./claude
├── .agents/plugins/marketplace.json  # Codex marketplace   → ./codex
├── .cursor-plugin/marketplace.json   # Cursor marketplace  → cursor
├── skills/                           # Canonical skills — edit these
├── claude/
│   ├── .claude-plugin/plugin.json
│   ├── .mcp.json
│   ├── skills/                       # Generated from ../skills
│   └── README.md
├── codex/
│   ├── plugin.json                   # Agent Plugins 1.0.0 manifest
│   ├── mcp.json
│   ├── skills/                       # Generated from ../skills
│   └── README.md
├── cursor/
│   ├── .cursor-plugin/plugin.json
│   ├── mcp.json
│   ├── skills/                       # Generated from ../skills
│   ├── README.md
│   ├── CHANGELOG.md
│   └── LICENSE
├── scripts/
├── README.md
└── LICENSE
```

## Skills

| Skill | What it does |
| --- | --- |
| `working-with-datagate` | Shared reference: what one connection reaches, finding records by name, search filters and paging, full-replace updates, and which calls need confirmation |
| `mcp-auth` | Installs the plugin and MCP server, and signs in or sets up an API key when a call fails authentication |

Edit skills only under `skills/`, then copy them into every plugin:

```sh
node scripts/sync-skills.mjs
```

CI runs `node scripts/sync-skills.mjs --check` and `node scripts/validate-skills.mjs`, and validates the Claude manifests with `claude plugin validate --strict`.

## License

MIT
