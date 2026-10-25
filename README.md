# NPI Who MCP server

Look up US health care providers from AI assistants: by NPI, by name, by phone number, by what is near them, and by facility type and state. The server is hosted at **https://npiwho.com/mcp** (Model Context Protocol, Streamable HTTP). It is free and needs no key.

Every result that names a provider or facility starts with the public page's URL on [npiwho.com](https://npiwho.com/), so an assistant can cite the page it answered from and send readers there for the detail.

## Connect

Nothing to install from this repository. Each client keeps its own list of MCP servers: add the endpoint once in the client you use, then start a **new** conversation (it does not appear in one that is already open). No account, nothing to sign in to.

**Claude Code** (terminal), once, in any directory:

```
claude mcp add --transport http -s user npiwho https://npiwho.com/mcp
```

`-s user` makes it available in every project; leave it out for the current project only. `claude mcp list` should show `npiwho` as connected. Remove with `claude mcp remove -s user npiwho`.

**Claude.ai** (web and desktop): Settings → Connectors → Add custom connector. Name: NPI Who. URL: `https://npiwho.com/mcp`. Authentication: none. Save, then turn it on in a chat's tools menu.

**ChatGPT**: Settings → Connectors → Create (developer mode must be on). URL as above, No authentication, save, then enable it for a chat.

**Codex** (terminal):

```
codex mcp add npiwho --url https://npiwho.com/mcp
```

**Cursor, Windsurf, VS Code** and other clients with a JSON config (`mcp.json` in this repository):

```json
{
  "mcpServers": {
    "npiwho": { "type": "http", "url": "https://npiwho.com/mcp" }
  }
}
```

Then ask in plain language: "What do the public records say about NPI 1366487498?", "Who is at (210) 489-7220?", "Which hospices in Austin, Texas are non-profit?" The assistant picks the tool.

## Tools

| Tool | Returns |
|---|---|
| `lookup_provider` | One NPI: the registry record, and what the public files have on the provider (Open Payments total, Medicare billing, OIG exclusion, Medicare clinician record, facility enrollments, claimed-page details), each pointing to the page section with the detail. |
| `search_providers` | Individuals or organizations by name, optionally within a state. Up to 20. |
| `lookup_phone` | Who lists a given phone number, in any US format. |
| `nearby_providers` | The nearest providers of the same specialty, from an NPI or from a ZIP code and taxonomy code. |
| `list_facilities` | Medicare-certified home health agencies, hospices, hospitals, nursing homes, or HRSA health centers in a state or city. |
| `data_sources` | Which files the answers come from and their snapshot dates. |

Try: "What do the public records say about NPI 1366487498?" or "Which hospices in Austin, Texas are non-profit?"

## Data

Public federal records joined on the NPI: the NPPES registry (CMS), CMS Open Payments (program years 2019–2025), Medicare Part B and Part D (2024), the HHS-OIG exclusion list, CMS Doctors and Clinicians, Medicare facility enrollments, and HRSA's health center list. File dates are on the [methodology page](https://npiwho.com/methodology) and in the `data_sources` tool.

None of it is a licence, credential or quality check. Providers can ask for their contact details to be withheld, and those requests apply to the tools exactly as to the pages.

## Allowance

About 1,000 tool calls a day per client address, resetting at 00:00 UTC. Calls made through Claude.ai or ChatGPT come from the platform's addresses and share a larger pool. For more, or for bulk work, write to data@npiwho.com. The per-provider payment totals are also an open dataset: [npiwho/us-provider-payments](https://github.com/npiwho/us-provider-payments).

## Who runs it

[Derek Hong](https://npiwho.com/about), Singapore. The server is part of the site; this repository holds its description for the MCP registry and the connection instructions. Issues and questions here or at data@npiwho.com.
