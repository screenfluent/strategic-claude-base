---
description: "Read and execute a plan document from .strategic-claude-basic/plan/"
argument-hint: <plan_file> [--with-codex]
model: claude-sonnet-4-0
---

# Implement Plan

You are tasked with implementing an approved technical plan from `.strategic-claude-basic/plan/`. These plans contain phases with specific changes and success criteria.

**Plan file provided:** $1

## Optional Pair Programming Navigator (`--with-codex`)

- Check the command arguments for `--with-codex`. When present, enable Codex pair-programming mode.
- First, confirm the `mcp__codex__codex` tool is available. If not, inform the user:

  ```
  ❌ Codex MCP Navigator unavailable — continuing in solo driver mode. Please configure the Codex MCP server and rerun with `--with-codex` when ready.
  ```

- When the tool is available, treat Codex as a Navigator that maintains the global picture:
  - Brief Codex on the plan goals, constraints, and current context.
  - Ask Codex to outline the execution roadmap and explicitly provide the next action when requested.
  - Before each significant action, consult Codex for guidance; after completing it, summarize progress back to Codex and request the next direction.
- Operate as the Driver with constant self-reflection:
  - After receiving Codex guidance, restate the instruction in your own words, evaluate risks, and confirm the plan still aligns with success criteria.
  - Proactively note uncertainties or validation needs before acting; adjust with Codex if gaps appear.
  - Periodically sanity-check that Codex's roadmap still matches plan phases and checkbox items.
- Finish by confirming with Codex that all Navigator tasks are satisfied and no remaining checkpoints exist before marking plan items complete.

## CRITICAL: Checkbox-Only Updates

**NEVER modify the plan text**. The ONLY changes allowed are:
- Changing `- [ ]` to `- [x]` when a task is complete
- NO summaries, NO additional details, NO implementation notes
- The plan document is a CONTRACT - preserve it exactly as written

**Example of correct checkbox update:**
```
OLD: "- [ ] Task description"
NEW: "- [x] Task description"
```

**DO NOT add comments, notes, or ANY other text to the plan file.**

## Getting Started

**If no plan path provided:**
```
I'll help you implement a plan from .strategic-claude-basic/plan/

Please provide the plan filename you'd like me to execute.
Example: `/read_and_execute_plan PLAN_0001_07-09-2025_sat_user-authentication.md`
```

**When given a plan path:**

1. Determine the actual plan file argument (ignore the optional `--with-codex` flag when present), then read the plan completely and check for any existing summary for the plan in `.strategic-claude-basic/summary/`
2. Read all files mentioned in the plan
3. If a corresponding summary exists read that summary and note the progress that may have been completed already
4. **Read files fully** - never use limit/offset parameters, you need complete context
5. Think deeply about how the pieces fit together
6. Create a todo list to track your progress
7. Start implementing if you understand what needs to be done

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:

- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update ONLY checkboxes in the plan as you complete sections (change `- [ ]` to `- [x]`)

When things don't match the plan exactly, think about why and communicate clearly. The plan is your guide, but your judgment matters too.

If you encounter a mismatch:

- STOP and think deeply about why the plan can't be followed
- Present the issue clearly:

  ```
  Issue in Phase [N]:
  Expected: [what the plan says]
  Found: [actual situation]
  Why this matters: [explanation]

  How should I proceed?
  ```

## Verification Approach

After implementing a phase:

### Automated Verification:
- Run all commands listed under "Automated Verification" in the success criteria
- Ensure all tests pass and builds succeed
- Fix any issues before proceeding

### Manual Verification:
- Test features listed under "Manual Verification" 
- Confirm functionality works as expected
- Test edge cases mentioned in the plan

### Progress Updates:
- Update your TodoWrite progress tracker
- Check off completed items in the plan file using Edit tool
- **REMINDER**: Only change `- [ ]` to `- [x]` in the plan - NO other modifications

Don't let verification interrupt your flow - batch it at natural stopping points.

## If You Get Stuck

When something isn't working as expected:

- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:

- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

## Plan Integrity

The plan document is a historical contract that must remain unchanged except for checkbox updates:

- **Preserve all text exactly as written**
- **Only update checkbox states**: `- [ ]` → `- [x]`
- **No summaries or notes** in the plan document
- **No implementation details** added to the plan
- **No modifications to success criteria** or task descriptions

Use your TodoWrite tracker for implementation notes and progress details.

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum while preserving the plan's integrity.
