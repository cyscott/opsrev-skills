---
name: notion-workspace-opsrev
description: "Use when the user asks to find, read, summarize, analyze, create, or update Notion pages, databases, blocks, and comments through the connected OpsRev Notion integration."
---

# Notion Workspace OpsRev

## Goal
Use connected Notion data safely and quickly for discovery, retrieval, summaries, structured analysis, and content updates.

## Tooling

### Read tools
- `notion_search` / `notion-search` — search pages and databases
- `notion_list_databases` / `notion-query-data-sources` — list databases
- `notion_query_database` / `notion-query-database-view` — query a database with filters/sorts
- `notion_get_page` / `notion-fetch` — get page metadata/properties
- `notion_get_page_content` — get block children for a page or block
- `notion_get_comments` / `notion-get-comments` — list comments for a page or block

### Write tools
- `notion_create_page` / `notion-create-pages` / `create-a-page` — create a page
- `notion_update_page` / `notion-update-page` / `update-a-page` — update page properties/metadata
- `notion_append_block` / `notion-append-a-block` / `append-a-block` — append blocks
- `notion_update_block` / `notion-update-a-block` / `update-a-block` — update a block
- `notion_delete_block` / `notion-delete-a-block` / `delete-a-block` — delete a block
- `notion_create_database` / `notion-create-database` / `create-a-data-source` — create a database
- `notion_create_comment` / `notion-create-comment` / `create-a-comment` — create a comment

Use canonical names (`notion_*`) by default. Aliases are for compatibility.

## Workflow

### Reading & Analysis
1. Confirm intent in one sentence (search, summarize, extract, compare, or report).
2. Discover targets with `notion_search` or `notion_list_databases`.
3. Pull details with `notion_get_page`, `notion_get_page_content`, or `notion_query_database`.
4. Return the answer first, then include source object IDs/titles used.
5. If results are empty or permission-limited, say that explicitly and suggest reconnecting Notion or sharing the page/database with the integration.

### Creating Pages
1. Ask for parent target (page or database), title, and desired content.
2. Discover IDs first (use search/list/query tools).
3. Call `notion_create_page` with:
   - `parent` object (`{ "page_id": "..." }` or `{ "database_id": "..." }`)
   - `properties` for title/database fields
   - optional `children` block array
4. Return the created page URL/ID and what was set.

### Updating Pages
1. Resolve the page ID.
2. Call `notion_update_page` with only the fields to change.
3. Confirm exactly what changed.

### Working with Blocks
1. Use `notion_get_page_content` to inspect existing block structure first.
2. For new content, call `notion_append_block`.
3. For edits, call `notion_update_block`.
4. For removals, call `notion_delete_block`.
5. Return affected block IDs.

### Databases and Comments
1. Create databases with `notion_create_database` using explicit `parent`, `title`, and `properties`.
2. Add comments with `notion_create_comment` using `discussionId` or `pageId`/`blockId`.
3. Confirm returned IDs/URLs.

## Response Rules
- Do not invent Notion content.
- Keep results concise and decision-oriented.
- Prefer structured output for ops use cases (table-like bullets with owner, status, due date, blockers).
- When confidence is low, ask one targeted follow-up question.
- After any write, summarize exactly what was created/changed and include returned IDs/URLs.
