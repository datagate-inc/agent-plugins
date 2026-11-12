---
name: working-with-datagate
description: Shared reference for working in Datagate through its MCP server, covering what one connection reaches, how to find records by name, paging and search filters, full-replace updates, and which calls need the user's confirmation. Use before reading or changing customers, invoices, products, agreements, rate cards or other Datagate data.
---

# Working with Datagate

Datagate is a billing platform for telecom and IT service providers. The Datagate MCP server exposes its public API as tools: customers and sites, invoices and billing periods, agreements, products and product transactions, product templates and kits, rate cards, service items, and a few account settings.

If the Datagate tools are missing from this session, or a call fails authentication, use the `mcp-auth` skill.

## What one connection reaches

- **One company.** A connection is bound to the company (reseller) the user picked when signing in, or the company an API key belongs to. Every call lands there. There is no tool to switch companies; to work on another, the user reconnects and picks it.
- **Distributors stay inside one company too.** A distributor does not get its usual reach across all of its brands over MCP.
- **The user's own portal permissions.** A signed-in user only sees the tools their portal role allows. A tool that is missing from the list is not a bug to work around: tell the user which permission they would need.
- **Read only or read and change.** A user who chose read only at sign-in, or a read-only API key, has no write tools at all. Search tools still work, even though they are POST requests.
- **Rate limits** apply per user and company. Prefer one search with a filter over paging through a whole list.

## Toolsets

| Toolset | Covers |
| --- | --- |
| `core` | customers, customer portal users, sites |
| `billing` | invoices, billing periods, agreements, product transactions |
| `catalog` | products and their charges, product templates, kits, rate cards, service items |
| `admin` | invoice delivery methods, account managers, rating assignments |

## Finding records

Tools take ids, which are GUIDs. Users name things. Turn a name into an id with a search tool before anything else:

- **Customer by name:** `search_customers` with `$like` for a partial name. Don't page through `list_customers` to find one.
- **Product by name:** `search_products`. Product templates: `search_product_templates`. Agreements: `search_agreements`.
- **Invoices:** there is no plain list. `search_invoices` is the entry point for every invoice question. To filter by billing period, get the `BillingPeriodId` from `list_billing_periods` first.
- **Invoice PDF:** `get_invoice_pdf` returns the PDF as an embedded resource. A `409` means the PDF has not been generated yet; say so rather than retrying.

When a search returns several plausible matches, show them and ask the user which one they mean. Never pick one silently before a write.

### Search filters

Search tools take a filter object. Keys must be plain field names, or one of these operators: `$and`, `$or`, `$in`, `$like`, `$between`, `$gt`, `$gte`, `$lt`, `$lte`, `$ne`. Any other key is refused before it reaches Datagate. Each search tool's description lists the field names known to work; an unknown field name fails, sometimes with a bare `500`.

### Paging

`page` starts at 1. `pageSize` defaults to 100 on most lists. Stop paging once you have what the user asked for.

## Changing data

### Updates replace the whole record

Every `update_*` tool is a full replace (HTTP PUT): **fields left out of the body are cleared.** Always:

1. Fetch the current record with the matching `get_*` tool.
2. Change only the fields the user asked about.
3. Send every field back.

Never build an update body from memory or from a list result, which may not carry every field.

### Confirm before destructive or billing-changing calls

Ask the user to confirm, naming the record, before:

- any `delete_*` call. Several cannot be undone, and some cascade: deleting a kit template removes its products, deleting a delivery method clears it from every customer using it, and deleting a service item removes its aliases and group children.
- anything that changes what a customer is billed or how usage is priced: product charges, product transactions, rate cards and their rates and products, and rating assignments.

Invoiced product transactions cannot be deleted. An account manager with active customers cannot be deleted.

## Errors

| Status | Meaning | What to do |
| --- | --- | --- |
| `401` | Not signed in, or the sign-in or API key is no longer valid | Use the `mcp-auth` skill |
| `403` | The user's role or a read-only connection doesn't allow this | Tell the user what is needed; don't retry |
| `404` | The id doesn't exist in this company | Search again; the record may belong to another company |
| `409` | Usually a state conflict, such as an invoice PDF not yet generated | Explain; don't loop |
| `429` | Rate limited | Wait before retrying, and narrow the query |
| `503` | Datagate could not verify the connection right now | Retry shortly; this is not a sign-in problem |

Permission changes made in the portal reach an existing sign-in within about 20 minutes.
