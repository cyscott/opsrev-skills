---
name: notion-workspace-opsrev
description: "Use when the user asks to find, read, summarize, analyze, or create/update Notion pages, databases, blocks, and comments through the connected OpsRev Notion integration."
---

# Notion Workspace OpsRev

## Goal
Use connected Notion data safely and quickly for search, retrieval, summaries, structured analysis, and content creation.

## Tooling

### Read Tools (use first for discovery)
- `notion_search` / `notion-search` — search across the workspace
- `notion_list_databases` / `notion-query-data-sources` — discover databases and data sources
- `notion_query_database` / `notion-query-database-view` — query a database with filters/sorts
- `notion_get_page` / `notion-fetch` — retrieve page metadata and properties
- `notion_get_page_content` — retrieve full page block content
- `notion-get-comments` — list comments on a page

### Write Tools
- `notion-create-pages` / `create-a-page` — create one or more new pages with properties and content
- `notion-update-page` / `update-a-page` — update page properties or content
- `notion-append-a-block` / `append-a-block` — add content blocks (paragraphs, headings, lists, to-dos, code, etc.) to a page
- `notion-update-a-block` / `update-a-block` — modify an existing block's content
- `notion-delete-a-block` / `delete-a-block` — remove a block from a page
- `notion-create-database` / `create-a-data-source` — create a new database with schema
- `notion-move-pages` / `move-page` — move pages or databases to a new parent
- `notion-duplicate-page` — duplicate a page within the workspace
- `notion-create-comment` / `create-a-comment` — add a comment to a page or block

> Tool names vary between Notion's hosted MCP server and the open-source self-hosted server. Try the first variant; fall back to the second if unavailable.

## Workflow

### Reading & Analysis
1. Confirm intent in one sentence (search, summarize, extract, compare, or report).
2. Discover targets with `notion_search` or `notion_list_databases`.
3. Pull details with `notion_get_page`, `notion_get_page_content`, or `notion_query_database`.
4. Return the answer first, then include source object IDs/titles used.
5. If results are empty or permission-limited, say that explicitly and suggest reconnecting Notion or sharing the page/database with the integration.

### Creating Pages
1. Ask the user for: **parent** (page or database), **title**, and **content outline**.
2. If the parent is a database, discover its schema first with `notion_query_database` or `notion-query-data-sources` so you can set the correct properties.
3. Build the page payload:
   - Set `parent` to the database ID or page ID.
   - Set `properties` (title and any database-specific fields like Status, Tags, Assignee, Due Date).
   - Set `children` with an array of block objects for the page body.
4. Call `notion-create-pages` / `create-a-page`.
5. Return the new page URL and ID.

### Adding Content Blocks
1. Identify the target page ID (search if needed).
2. Build block objects for the content. Supported block types:
   - `paragraph`, `heading_1`, `heading_2`, `heading_3`
   - `bulleted_list_item`, `numbered_list_item`, `to_do`
   - `code`, `quote`, `callout`, `divider`, `table`, `toggle`
   - `image`, `embed`, `bookmark`, `file`
3. Call `notion-append-a-block` / `append-a-block` with the page ID and children array.
4. Return confirmation with block IDs.

### Updating Content
1. Retrieve the current page or block content first.
2. Call the appropriate update tool.
3. Return confirmation.

## Response Rules
- Do not invent Notion content.
- Keep results concise and decision-oriented.
- Prefer structured output for ops use cases (table-like bullets with owner, status, due date, blockers).
- When confidence is low, ask one targeted follow-up question.
- After any write operation, summarize what was created/changed and include the page or block URL/ID.

## Write Safety
- If write tools are unavailable, state that clearly and provide the exact write plan that would be executed once enabled.
