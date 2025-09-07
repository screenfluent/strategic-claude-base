---
description: "Create implementation plans for specific roadmap phases or milestones"
argument-hint: <phase_or_milestone_number> [optional_focus_area]
allowed-tools: Read(./**), Write(.strategic-claude-basic/plan/**), Task, Bash(mkdir:*, date:*, git:*), Glob
model: claude-opus-4-1
---

You are tasked with creating focused implementation plans for specific phases or milestones from the product roadmap. You should validate product documentation exists, extract phase/milestone-specific requirements, and create detailed implementation plans aligned with the phase objectives and deliverables.

**Phase/Milestone provided:** $1

## Initial Response

When this command is invoked:

1. **Check if phase/milestone number was provided**:

   - If a phase/milestone number was provided as parameter (e.g., "2.3", "1", "Phase 2.1", "3"), proceed with validation
   - If no phase/milestone number provided, respond with:

```
I'll help you create implementation plans for specific phases or milestones from your product roadmap.

Please provide:
1. The phase or milestone number you want to plan (e.g., "1", "1.2", "2.3", "3")
2. Optional focus area or specific aspect to emphasize

Examples:
- `/plan_phase 2.3` - Creates plan for milestone 2.3 only
- `/plan_phase 3` - Creates plans for all milestones in phase 3 (3.1, 3.2, 3.3)
- `/plan_phase 1.1 infrastructure` - Creates plan for milestone 1.1 with infrastructure focus

I'll validate your product documentation, extract phase/milestone details, and create targeted implementation plans.
```

Then wait for the user's input.

## Process Steps

### Step 1: Product Documentation Validation

1. **Check for required product documentation files**:

   - Look for existence of these files in `.strategic-claude-basic/product/`:
     - `PRD.md` (Product Requirements Document) - **REQUIRED**
     - `ARCHITECTURE.md` (System Architecture) - **REQUIRED**
     - `ROADMAP.md` (Product Roadmap) - **REQUIRED**
     - `REFERENCES.md` (Local Repository References) - **OPTIONAL**

2. **If any required files are missing**:

   ```
   Product documentation validation failed. Missing required files:
   - [ ] PRD.md - Product Requirements Document
   - [ ] ARCHITECTURE.md - System Architecture
   - [ ] ROADMAP.md - Product Roadmap

   To use phase-based planning, you need complete product documentation.

   Please first create your product documentation using:
   `/create_product_roadmap [research_file1].md [research_file2].md`

   This will generate all required product documentation files based on your research.
   ```

   Stop here and wait for user to create missing documentation.

3. **If all required files exist**, proceed to read them:

   - **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - Read PRD.md, ARCHITECTURE.md, and ROADMAP.md completely
   - Read REFERENCES.md if it exists
   - This provides full product context before phase planning

### Step 2: Phase/Milestone Extraction and Validation

1. **Parse the provided phase/milestone number**:

   - Support multiple formats:
     - Simple numbers: "1", "2", "3" (full phase)
     - Decimal notation: "1.1", "2.3", "1.2" (specific milestone)
     - With Phase prefix: "Phase 1", "Phase 2.1"
     - With Milestone prefix: "Milestone 1.2"
   - Extract major phase number (e.g., "2" from "2.3")
   - Extract milestone number if provided (e.g., "3" from "2.3")

2. **Search for phase/milestone in ROADMAP.md**:

   - Look for phase headers matching patterns:
     - `## Phase [X]:` for major phases
     - `### Milestone [X.Y]:` or `### [Milestone X.Y]:` for specific milestones
     - Handle variations in formatting and naming

3. **If phase/milestone not found**:

   - Extract available phases and milestones from ROADMAP.md
   - Present clear options to user:

   ```
   Phase/Milestone [provided_number] not found in ROADMAP.md.

   Available phases and milestones in your roadmap:
   - Phase 1: [Phase Name] (Milestones: 1.1, 1.2, 1.3)
   - Phase 2: [Phase Name] (Milestones: 2.1, 2.2, 2.3)
   - Phase 3: [Phase Name] (Milestones: 3.1, 3.2)

   Please provide a valid phase or milestone number from the list above.
   ```

   Wait for user correction.

4. **Determine planning scope**:

   **For specific milestone (e.g., "2.3")**:

   - Extract single milestone content
   - Plan for that milestone only

   **For full phase (e.g., "2")**:

   - Extract all milestones in that phase
   - Plan to create separate plan files for each milestone

5. **Present planning scope**:

   **For single milestone**:

   ```
   Found Milestone [X.Y]: [Milestone Name]

   **Objectives**: [Milestone objectives]
   **Key Deliverables**:
   - [Deliverable 1]
   - [Deliverable 2]

   **Timeline**: [Target dates if specified]

   I'll create a focused implementation plan for this milestone.
   ```

   **For full phase**:

   ```
   Found Phase [X]: [Phase Name]

   **Milestones to plan**:
   - Milestone [X.1]: [Name] - [Brief description]
   - Milestone [X.2]: [Name] - [Brief description]
   - Milestone [X.3]: [Name] - [Brief description]

   I'll create separate implementation plans for each milestone in this phase.
   ```

### Step 3: Research Discovery and Integration

1. **Search for phase/milestone-related research documents**:

   - Look in `.strategic-claude-basic/research/` for files related to the phase
   - Search patterns:
     - Exact match: "phase[X]", "phase-[X]", "milestone-[X]-[Y]"
     - Partial match: "[X].[Y]" in filename
     - Content search: phase/milestone numbers in document content

2. **Read all related research documents**:

   - **IMPORTANT**: Read completely, not partially
   - Extract research relevant to the specific phase/milestone
   - Note technical findings and implementation recommendations

3. **Create planning research todo list** using TodoWrite to track planning tasks

4. **Spawn parallel sub-tasks for comprehensive planning research**:

   **For each milestone being planned:**

   - Use the **codebase-locator** agent to find files related to milestone deliverables
   - Use the **codebase-analyzer** agent to understand current implementation state
   - Use the **codebase-pattern-finder** agent to find similar implementation patterns

   Each research task should:

   - Focus on specific milestone deliverables and success criteria
   - Find relevant code examples and implementation patterns
   - Identify technical challenges and solutions
   - Return specific file:line references
   - Understand integration points and dependencies

5. **Wait for ALL sub-tasks to complete** before proceeding with planning

### Step 4: Implementation Plan Generation

1. **For each milestone to be planned**:

   - Gather metadata for plan document:

     ```bash
     # Get current date/time with timezone
     date --iso-8601=seconds

     # Get current git commit hash
     git rev-parse HEAD

     # Get current branch name
     git branch --show-current

     # Get next document number and create filename
     date '+%d-%m-%Y-%a' | tr '[:upper:]' '[:lower:]'
     ```

   - Generate subject name from milestone:
     - Milestone 1.1 → "phase1-1-project-infrastructure"
     - Milestone 2.3 → "phase2-3-motion-blur-integration"
     - Full Phase 3 → "phase3-1-interactive-controls", "phase3-2-visual-themes", "phase3-3-mobile-optimization"

2. **Create implementation plan document**:

   - Use template: `@.strategic-claude-basic/templates/commands/plan.template.md`
   - Replace ALL bracketed placeholders with actual milestone details
   - Follow naming convention: `PLAN_NNNN_DD-MM-YYYY_day_phase[X]-[Y]-[subject].md`

3. **Structure plan around milestone deliverables**:

   For each major deliverable:

   - Current state analysis (what exists today)
   - Implementation approach (how to build it)
   - Technical challenges (what obstacles exist)
   - Success validation (automated and manual criteria)
   - Integration points (how it connects to other components)

4. **Write plan document to**: `.strategic-claude-basic/plan/[filename]` using the naming convention

5. **Update the planning registry**: Add entry to `.strategic-claude-basic/plan/CLAUDE.md`

### Step 5: Plan Presentation and Completion

1. **For single milestone planning**:

   ```
   I've created a focused implementation plan for Milestone [X.Y]: [Milestone Name]

   **Plan Document**: `.strategic-claude-basic/plan/[filename].md`

   **Key Implementation Areas**:
   - [Technical area 1 with specific approach]
   - [Technical area 2 with integration points]
   - [Technical area 3 with success criteria]

   **Success Criteria**: [X] automated verification steps, [Y] manual verification steps

   The plan is ready for implementation and includes specific file paths, code examples, and verification procedures.
   ```

2. **For full phase planning**:

   ```
   I've created implementation plans for all milestones in Phase [X]: [Phase Name]

   **Plan Documents Created**:
   - `.strategic-claude-basic/plan/PLAN_NNNN_..._phase[X]-1-[name].md` - [Milestone X.1 Name]
   - `.strategic-claude-basic/plan/PLAN_NNNN+1_..._phase[X]-2-[name].md` - [Milestone X.2 Name]
   - `.strategic-claude-basic/plan/PLAN_NNNN+2_..._phase[X]-3-[name].md` - [Milestone X.3 Name]

   **Phase Overview**:
   - Total Deliverables: [Number across all milestones]
   - Implementation Timeline: [Phase duration]
   - Success Criteria: [Total automated and manual verification steps]

   Each plan is milestone-focused with specific implementation steps, code examples, and verification procedures.
   ```

## Important Guidelines

1. **Be Milestone-Focused**:

- All planning should directly support the specified milestone objectives
- Prioritize deliverables that impact milestone success criteria
- Connect technical solutions to business outcomes defined in the milestone

2. **Be Implementation-Ready**:

- Provide actionable implementation steps for milestone deliverables
- Include specific code examples and patterns from existing codebase
- Identify reusable components and utilities
- Define clear success criteria (automated and manual)

3. **Be Research-Informed**:

- Read ALL related research documents before planning
- Include specific file paths and line numbers for implementation references
- Cross-reference technical solutions with research findings
- Address technical challenges identified in research

4. **Be Scope-Appropriate**:

- Single milestone plans focus exclusively on that milestone
- Full phase plans create separate, focused plans per milestone
- Include clear "what we're NOT doing" sections to prevent scope creep
- Balance comprehensiveness with milestone-specific relevance

## Plan Quality Standards

### For Single Milestone Plans:

- [ ] Milestone objectives clearly addressed
- [ ] All deliverables have implementation approaches
- [ ] Success criteria include automated and manual verification
- [ ] Dependencies and integration points identified
- [ ] Technical challenges from research addressed
- [ ] Specific file:line references provided for implementation

### For Full Phase Plans:

- [ ] Separate plan created for each milestone in the phase
- [ ] Each plan maintains milestone-specific focus
- [ ] Cross-milestone dependencies identified
- [ ] Phase-level coordination considerations included
- [ ] Sequential numbering follows CLAUDE.md conventions
- [ ] All milestone plans meet single milestone standards

## Milestone Planning Focus Areas

### For Infrastructure Milestones (typically X.1):

- Development environment and tooling setup
- Build system configuration and optimization
- Dependency management and integration
- Basic project structure and organization

### For Core Feature Milestones (typically X.2):

- Primary feature implementation and logic
- Data models and state management
- API design and integration points
- Core user experience components

### For Integration/Polish Milestones (typically X.3):

- System integration and testing
- Performance optimization and tuning
- User interface polish and enhancement
- Documentation and deployment preparation

### For Advanced Feature Milestones (typically X.4+):

- Advanced functionality and edge cases
- Scalability and performance enhancements
- Complex integrations and external services
- Specialized tooling and utilities

## Example Phase/Milestone Patterns

### Foundation Phase (Phase 1):

- 1.1: Project Infrastructure Setup
- 1.2: Basic System Implementation
- 1.3: Core Algorithm/Logic Implementation

### Development Phase (Phase 2):

- 2.1: Advanced System Implementation
- 2.2: Feature Enhancement and Optimization
- 2.3: Integration and Effects Implementation

### Enhancement Phase (Phase 3):

- 3.1: Interactive Features and Controls
- 3.2: Visual Enhancements and Themes
- 3.3: Platform Optimization (Mobile/Desktop)

### Production Phase (Phase 4):

- 4.1: Testing and Quality Assurance
- 4.2: Performance Optimization and Tuning
- 4.3: Documentation and Deployment

---

**Planning Focus**: Milestone-specific implementation plans with actionable steps
**Output**: Focused implementation plans aligned with milestone objectives and deliverables
**Success Measure**: Plans directly enable milestone implementation and success criteria validation
