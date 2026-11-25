---
name: clema
description: Use the Clema MCP server for institutional research work in U.S. higher education. Use when the user asks about their IR data request queue (tickets), wants to file a data request, or asks a question that needs IPEDS or College Scorecard data (enrollment, graduation and retention rates, costs, aid, earnings, peer comparisons).
---

# Clema: data requests and federal higher-ed data

Clema is an AI analyst for institutional research (IR) and institutional effectiveness (IE) teams. Its MCP server at `https://mcp.clema.ai/mcp` (Streamable HTTP, OAuth sign-in in the browser) gives you two sets of tools.

If the tools are missing, connect the server first. Claude Code: `claude mcp add --transport http clema https://mcp.clema.ai/mcp`. A paid Clema plan is required; new users can start a free trial at https://app.clema.ai/sign-up.

## Data requests (tickets)

1. Call `account_details` first. It returns the user's organizations (with ids), their role in each, and the team.
2. `tickets_list` for the queue (filter by the user's inbox or unassigned, status, priority). `ticket_details` for one request: intake, due date, assignee, activity timeline. It does not include conversation messages.
3. `create_ticket` raises a new request. `update_ticket_settings` closes, reopens, reprioritizes or reassigns.

Rules the server enforces, so explain them rather than retrying:

- Data Requesters cannot use tickets.
- Members can assign a ticket only to themselves; owners and admins can assign to any owner, admin or member.
- Reopening assigns the ticket to whoever reopened it.
- People come back by name and role, never by email. Don't ask for or guess email addresses.

Confirm with the user before `create_ticket` or `update_ticket_settings`; both change data in Clema.

## IPEDS and College Scorecard

Always in this order:

1. `institution_search` to resolve each school to its `unitid` (the same number is `institution_id` in College Scorecard). If several match, ask which one.
2. `data_dictionary_search` to find the real variable names and their coded values.
3. `data_tables` to see which table or multi-year `v_*` view holds them, and its columns.
4. `data_query` with a read-only `SELECT`. Results are paged; aggregate or filter rather than pulling many pages.

In the answer, cite the tables and variables used and the data year.
