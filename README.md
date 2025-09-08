# strategic-claude-basic

A structured template system for Claude AI development workflows with specialized commands, agents, and documentation conventions.

## Key Commands

### Research & Analysis
- `/research <topic>` - Comprehensive research with parallel sub-agents
- `/codebase_analyzer <component>` - Analyze code implementations 
- `/codebase_locator <feature>` - Find files and components
- `/codebase_pattern_finder <pattern>` - Find similar implementations

### Planning & Implementation
- `/plan <research_file>` - Create implementation plans
- `/plan_phase <plan_file>` - Execute plan phases
- `/read_and_execute_plan <plan_file>` - Execute plans with validation

### Documentation & Tracking
- `/summary [plan_reference]` - Problem-focused work summaries
- `/validate_summary <summary_file>` - Validate and archive summaries
- `/create_issue <subject_or_file>` - Create issue documentation

### Product Management
- `/create_product_roadmap <research_or_plan>` - Generate roadmaps
- `/archive_docs_update_roadmap` - Archive docs and update roadmap

## Structure

```
.strategic-claude-basic/
├── core/                # Commands and agent definitions
├── templates/           # Document templates
├── research/            # Research documentation
├── plan/                # Implementation plans
├── summary/             # Work summaries
├── issues/              # Issue tracking
└── product/             # Product documentation
```

## Document Naming

**CRITICAL**: Follow strict patterns:
- Research: `RESEARCH_NNNN_DD-MM-YYYY_day_subject.md`
- Plans: `PLAN_NNNN_DD-MM-YYYY_day_subject.md`
- Summaries: `SUMMARY_NNNN_DD-MM-YYYY_day_subject.md`

Get current date: `date '+%d-%m-%Y-%a' | tr '[:upper:]' '[:lower:]'`

## Usage

1. Start with `/research` for exploration
2. Use `/plan` to create implementation strategies
3. Use `/summary` to document problems and progress
4. Always check directory CLAUDE.md files for specific conventions

This framework emphasizes structured documentation over traditional development workflows.