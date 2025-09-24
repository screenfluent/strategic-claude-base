Create an automated fetch and diff log for local documentation.

Goal: Fetch latest doc from URL, save with date, compare to previous version, generate changelog using diff-analyzer, save to /research/.

Usage: /fetch-docs [URL] [name] [--path="docs/official"] (e.g., /fetch-docs "https://svelte.dev/llms-full.txt" "svelte-llms")

Process Steps:
Step 1: Parse inputs - URL, name (e.g., "svelte-llms"), path (default docs/official).
Step 2: Use Bash to fetch: curl -s [URL] > temp_new.txt. Check if success (HTTP 200).
Step 3: Find previous version: Use Glob on [path]/[name]-*.txt, sort by date in filename (newest before today), Read as old.txt. If none, note "First fetch".
Step 4: Save new: Generate date (YYYY-MM-DD), mv temp_new.txt [path]/[name]-[date].txt. If old exists, mv [path]/[name]-[old_date].txt to [path]/archive/[name]-[old_date].txt (immediate archive to keep official/ clean).
Step 5: If old exists, use diff-analyzer on old.txt and new.txt to generate changelog.
Step 6: Format output using research.template.md: Title "FETCH_DOCS [date] [name]", sections: New File Path, Diff Summary (from analyzer), Changelog.
Step 7: Save to /research/FETCH_DOCS_[date]_[name].md.

Hooks: Run post-fetch hook if exists (e.g., notify).

If error (e.g., curl fail), output "Fetch failed: [reason]".