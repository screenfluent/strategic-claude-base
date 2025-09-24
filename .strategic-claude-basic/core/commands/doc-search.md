Perform targeted search in local documentation with quotes and context.

Goal: Search docs for query, return structured quotes from newest files, save to /research/.

Usage: /doc-search [query] [--path="docs/official" --update] (e.g., /doc-search "vite config")

Process Steps:
Step 1: Parse query and path (default docs/official/*.txt). If --update, run /fetch-docs first if files >7 days old (check mtime via Bash).
Step 2: Use doc-searcher agent with query and path.
Step 3: Collect quotes from agent output.
Step 4: Format using research.template.md: Title "DOC_SEARCH [date] [query]", section "Relevant Quotes" with file:line + context.
Step 5: Save to /research/DOC_SEARCH_[date]_[query].md.
Step 6: (Optional) If MCP configured, use codex-researcher agent for independent analysis: Re-examine topic from alternative angle, identify missed connections, suggest related patterns from external docs, find conceptual gaps in search results.
Step 7: If no results, suggest: "Try broader query or fetch updates with /fetch-docs".

Integrate: Can chain to /plan by providing output file as input.