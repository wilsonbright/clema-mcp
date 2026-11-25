# Clema MCP

<p align="center">
  <img src="assets/icon.png" width="120" alt="Clema" />
</p>

<p align="center">
  <a href="https://registry.modelcontextprotocol.io/v0/servers?search=ai.clema"><img src="https://img.shields.io/badge/MCP%20Registry-ai.clema%2Fmcp-EE8D46?style=for-the-badge" alt="Official MCP Registry: ai.clema/mcp" /></a>
  <a href="https://clema.ai/docs/mcp"><img src="https://img.shields.io/badge/docs-clema.ai-2C2C2C?style=for-the-badge" alt="Docs" /></a>
  <br/>
  <img src="https://img.shields.io/badge/transport-Streamable%20HTTP-555?style=flat-square" alt="Streamable HTTP" />
  <img src="https://img.shields.io/badge/auth-OAuth%202.1-555?style=flat-square" alt="OAuth 2.1" />
  <img src="https://img.shields.io/badge/tools-9-555?style=flat-square" alt="9 tools" />
</p>

**Your AI client works the institutional research request queue and answers from IPEDS and College Scorecard.**

```
https://mcp.clema.ai/mcp
```

Paste that URL into Claude, ChatGPT, Cursor, Codex, VS Code or any MCP client. Sign in to Clema once in the browser. Nothing to install, no API key to copy.

**Claude Code**

```bash
claude mcp add --transport http clema https://mcp.clema.ai/mcp
```

or as a plugin, with the Clema skill included:

```
/plugin marketplace add wilsonbright/clema-mcp
/plugin install clema@clema-mcp
```

**Codex**

```bash
codex mcp add clema --url https://mcp.clema.ai/mcp && codex mcp login clema
```

More clients in [Install](#install).

> Needs a paid Clema plan. You can start with a free trial at [app.clema.ai/sign-up](https://app.clema.ai/sign-up?ref=github-mcp).

---

Institutional research offices run on requests. A dean wants retention by college, a provost wants peer graduation rates for a board deck, someone needs an enrollment number by Friday. The requests live in one place, the federal data in another, and the analyst stitches them together.

[Clema](https://clema.ai) is an AI analyst for IR and IE teams in U.S. higher education. This MCP server puts two parts of it inside the AI client you already use: the data request queue, and the public IPEDS and College Scorecard datasets.

## What you can ask

**Triage your inbox.** *"What data requests are assigned to me, and which are due this week?"* `tickets_list` filtered to your inbox, then `ticket_details` on the urgent ones.

**Pick up where a request stands.** *"Summarize the enrollment dashboard request from the provost's office."* `ticket_details` returns the intake, the audience and every status change.

**File a request without leaving chat.** *"Ask IR for fall-to-fall retention by college for the last five years."* `create_ticket` raises it; an owner or admin assigns it with `update_ticket_settings`.

**Benchmark against peers.** *"Compare our six-year graduation rate with five similar public universities."* `institution_search` resolves the schools, `data_dictionary_search` finds the variable, `data_query` pulls the numbers.

**Check program outcomes.** *"What do graduates of our nursing program earn?"* College Scorecard tables through `data_tables` and `data_query`.

## Tools

### Data requests (tickets)

| Tool | What it does | Changes anything? |
|------|--------------|-------------------|
| `account_details` | Who is connected: name and email, organizations and role in each, and each organization's team (names and roles). Call it first to learn organization ids. | No |
| `tickets_list` | The organization's data requests, newest activity first, with status, priority, assignee and requester. Filter by your inbox or unassigned, status and priority. | No |
| `ticket_details` | One request in full: status, priority, due date, requester, assignee, feedback, intake (what, why, output format, audience) and activity timeline. No conversation messages. | No |
| `create_ticket` | Raise a new data request with IR. Does not set priority or assignee. | **Yes** |
| `update_ticket_settings` | Close or reopen, change priority or assignee, under the same role rules as the Clema app. Nothing changes if any part isn't allowed. | **Yes** |

### IPEDS and College Scorecard

| Tool | What it does | Changes anything? |
|------|--------------|-------------------|
| `institution_search` | Find an institution by name or alias and get its `unitid` (same number as `institution_id` in College Scorecard). | No |
| `data_dictionary_search` | Find variables by name, title or description, with coded values. Use before writing SQL. | No |
| `data_tables` | Tables with row counts, or the columns of one table. Multi-year `v_*` views where they fit. | No |
| `data_query` | Read-only SQL `SELECT` against IPEDS or College Scorecard, paged results. | No |

Every tool carries `readOnlyHint` / `destructiveHint` annotations, so clients can ask before the two write tools run. No tool deletes anything.

## Roles

The server follows the Clema app's rules. Owners and admins can assign a request to any owner, admin or member. Members can assign only to themselves. Data Requesters cannot use tickets. Reopening a request assigns it to whoever reopened it. Teammates are returned by name and role, never by email.

## Install

The server is remote: nothing to download, run or update.

| Client | How |
|--------|-----|
| **Claude Code** | `claude mcp add --transport http clema https://mcp.clema.ai/mcp`, or the plugin above |
| **Claude.ai / Claude Desktop** | Settings → Connectors → Add custom connector → `https://mcp.clema.ai/mcp` |
| **ChatGPT** | Add a custom connector (developer mode) with `https://mcp.clema.ai/mcp` |
| **Codex** | `codex mcp add clema --url https://mcp.clema.ai/mcp` then `codex mcp login clema` |
| **Cursor** | Copy [`clients/cursor.json`](clients/cursor.json) into `.cursor/mcp.json` |
| **VS Code (Copilot)** | Copy [`clients/vscode.json`](clients/vscode.json) into `.vscode/mcp.json` |
| **Windsurf** | Copy [`clients/windsurf.json`](clients/windsurf.json) into `~/.codeium/windsurf/mcp_config.json` |
| **Gemini CLI** | `gemini extensions install https://github.com/wilsonbright/clema-mcp` |
| **Clients without remote HTTP** | [`clients/claude-desktop.json`](clients/claude-desktop.json) bridges through `mcp-remote` |
| **Agent skill** | `npx skills add wilsonbright/clema-mcp` |

OAuth is discovered from `https://mcp.clema.ai/.well-known/oauth-protected-resource/mcp` (PKCE, dynamic client registration).

## Honest limits

- Needs a paid Clema plan (free trial available). There is no anonymous access.
- The federal data tools cover IPEDS and College Scorecard. Your own institution's systems are not exposed through this server.
- `ticket_details` does not return conversation messages.
- The server is hosted by Clema and is not open source. This repository holds the public listing material: README, registry entry, client configs, plugin manifests and skill.

## FAQ

**Is it open source?** No. The server is hosted. This repo is MIT-licensed listing material.

**What does it cost?** See [clema.ai/pricing](https://clema.ai/pricing). The MCP server needs a paid plan; start with a free trial.

**Where is it listed?** The [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=ai.clema) as `ai.clema/mcp`, and MCP directories that import from it.

## Links

- Docs: https://clema.ai/docs/mcp
- Website: https://clema.ai
- Security: https://clema.ai/security · [SECURITY.md](SECURITY.md)
- Support: support@clema.ai

## License

MIT for the contents of this repository. See [LICENSE](LICENSE).
