# Issue Documentation Standards

> Claude Code Memory for .strategic-claude-basic/issues directory
> Issue Documentation

## Document Naming Convention

**CRITICAL**: All issue documents in this directory MUST follow this exact pattern:

```
ISSUE_NNNN_DD-MM-YYYY_day_subject.md
```

Where:

- **NNNN** = Sequential document number (0001, 0002, etc.)
- **DD-MM-YYYY** = Date in day-month-year format
- **day** = Truncated day name (mon, tue, wed, thu, fri, sat, sun)
- **subject** = Brief hyphenated description

## Current Status

✅ All files follow the truncated day name convention.

## Existing Documents

1. **document-name** - **subject**

Next document number: **0001**

## Instructions for Claude Code

When working in this directory:

1. **Always check file names** against the convention before creating new files
2. **Refuse to create** files that don't follow the naming pattern
3. **Suggest correct names** when violations are detected
4. **Maintain sequential numbering** for new documents
5. **Use truncated day names** (mon, tue, wed, thu, fri, sat, sun)
6. **Get current date dynamically**: Run `date '+%d-%m-%Y-%a' | tr '[:upper:]' '[:lower:]'` to get properly formatted date (e.g., "07-09-2025-sat")
7. **Update this file**: After creating new documents, update the "Existing Documents" list and "Next document number" in this CLAUDE.md file

This naming convention ensures:

- Chronological ordering of issue documents showing problem resolution timeline
- Clear identification of document purpose and creation date
- Consistent documentation standards across the project
- Historical context for problem patterns and solutions
