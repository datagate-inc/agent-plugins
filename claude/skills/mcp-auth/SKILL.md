---
name: mcp-auth
description: Install and authenticate Datagate's plugin and MCP server. Use during first-time setup, when the Datagate skills or MCP tools are missing, or when a Datagate call fails authentication.
---

# Datagate MCP authentication

## Assume the user is authenticated

Treat the user as authenticated. Call Datagate tools directly, and enter the [workflow](#workflow) only when the Datagate skills or MCP server are missing from this session, or a call actually fails authentication. When the user says they have signed in, take that as done and retry.

## Installed check

The Datagate plugin brings both the Datagate skills and the Datagate MCP server (tools such as `search_customers` and `search_invoices`). Before any install step, check what this session already has:

- **Skills and MCP server both available:** install nothing. Installing again creates duplicate skills and duplicate MCP servers.
- **Only one available:** install only the missing piece, following the client reference.

## Authentication

There is one MCP server for every region: `https://mcp.dgportal.net/mcp`. It accepts two kinds of credential.

### Portal sign-in (recommended)

The user signs in with their Datagate portal login through Passport (`login.dgportal.net`), picks the company to connect, and chooses **read only** or **read and change**. The connection then has exactly the user's own portal permissions in that company.

- The client asks the user to sign in when the server first connects. Ask the user to complete it in their client, then retry the original request.
- The company's administrator has to turn on **Allow MCP connections** for the brand first. If sign-in says MCP is not allowed, the user needs their administrator to turn it on; there is nothing to fix on the user's machine.
- Brands that sign in only through SSO can't connect this way. Use an API key instead.
- To work on another company, the user disconnects and signs in again, picking that company.
- Don't add `?toolsets=` to the URL for a sign-in connection. Sign-in only accepts the bare URL.

### Public API keys

Use an API key in Cursor (where it is the standard setup), when the user asks for one, or when their client cannot complete the sign-in.

- The key is created in the Datagate portal under **Settings → API Keys**, by a user with permission to manage API access. A key can be made read only.
- A key reaches only the company it belongs to, and is not limited by any one user's portal role: it gets every tool, or only the read tools if the key is read only.
- The key is sent as an `Authorization: Bearer <key>` header to `https://mcp.dgportal.net/mcp`, set in the client's own MCP settings. **Have the user enter the key there themselves, so it never appears in the chat.** Never ask the user to paste a key into the conversation, and never write one into a file that could be committed.
- An API key added by hand is a separate MCP server entry from the plugin's. Have the user disable the plugin's Datagate server so the two don't duplicate.

## Workflow

1. **Classify.** Capture the failing tool call and the non-secret error. Confirm it is an authentication failure (`401`, a missing or expired sign-in, an unknown key) or a missing install, not a permission refusal (`403`), a missing tool because of the user's role or a read-only connection, a validation error, or an outage (`503`). Hand anything else back to the `working-with-datagate` skill.
   **Done:** There is an authentication failure or missing install to repair, or the error is handed back.
2. **Identify the client.** Establish the client and surface you are running in (for example Claude Code versus Claude Desktop Chat). You usually know this already; ask if unclear. Read only the matching [client connection reference](CLIENTS.md).
   **Done:** You know the client and surface, and have read its reference.
3. **Connect.** Run the [installed check](#installed-check) and install only what is missing. Then have the user sign in, or enter their API key, as the reference describes.
   **Done:** Nothing is missing and the user confirms sign-in or the key is set.
4. **Verify and resume.** Call `list_customers` with `pageSize` 1 and confirm it answers. Ask the user to confirm it is the company they meant if that is in doubt. Resume the original request.
   **Done:** Datagate answers and the original request resumes, or the unresolved error is reported without repeated guesses.

## Cloud agents

A cloud or remote sandbox (Claude Code on the web, Codex cloud, Cursor background agents) is temporary and is not the user's app config, so leave its plugins and MCP config as they are. Direct the user to install the plugin and sign in through the app's own settings, following the UI steps in the matching client reference.
