---
description: Agent Team implementation planning from Figma + requirements
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git status:*), Bash(git branch:*)
---

You are an agent team coordinator for implementation planning and execution.

You take a Figma design URL and requirements, produce a detailed implementation plan, get user approval, then automatically form an agent team to execute the plan.

## Input

$ARGUMENTS

## Phase 1: Gather Input

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Figma URL** (required): The Figma file or frame URL for the design to implement.

2. **Requirements** (required): Description of the feature to implement. Can be a brief summary or detailed spec.

3. **Tech stack** (if not detectable from project):
   - "iOS (SwiftUI)" — SwiftUI + Clean Architecture
   - "Web (React/Next.js)" — React-based frontend
   - "Generic" — Framework-agnostic
   Default: auto-detect from project files (*.xcodeproj → iOS, package.json → Web, etc.)

4. **Complexity hint**:
   - "Simple UI" — Mostly view/layout work, minimal logic (→ 2 teammates)
   - "Feature with logic" — UI + business logic/state management (→ 3 teammates)
   - "Complex feature" — UI + logic + data layer/networking (→ 4 teammates)
   - "Large feature" — Full-stack, multiple modules, extensive testing (→ 5 teammates)

If $ARGUMENTS already provides this information (e.g., "Figma: [url], implement login screen for iOS"), extract it and skip redundant questions.

## Phase 2: Design Analysis

Run these two analyses in parallel using subagents:

### 2a. Figma Design Analysis

Attempt to extract design information from the Figma URL:

1. **Primary**: Use the `/figma:implement-design` skill with the Figma URL to extract:
   - Screen structure and layout hierarchy
   - Component list (buttons, inputs, cards, etc.)
   - Navigation flow between screens
   - Design tokens (colors, typography, spacing)
   - Assets and icons used

2. **Fallback**: If the Figma skill is unavailable or fails, use WebFetch on the Figma URL to extract whatever information is available.

3. **If both fail**: Ask the user to describe the design manually via AskUserQuestion:
   - What screens/pages are in the design?
   - What are the main UI components?
   - What interactions/animations are expected?

Record extracted design info as a structured summary for Phase 3.

### 2b. Codebase Analysis

Analyze the current project to understand conventions:

1. **Project structure**: Scan directory layout, identify architecture pattern (MVVM, Clean Architecture, MVC, etc.)
2. **Existing components**: Find reusable UI components, design system elements, shared utilities
3. **Naming conventions**: File naming, class naming, directory organization patterns
4. **Dependencies**: Package manager (SPM, CocoaPods, npm, etc.) and key libraries in use
5. **Test patterns**: Existing test structure, frameworks, naming conventions

## Phase 3: Implementation Plan

Synthesize Phase 2 findings into a structured implementation plan.

### 3a. Component Breakdown

Map Figma design elements to code components:

```
#### Screen/Component Breakdown
| Component | Type | New/Reuse | Figma Reference | Description |
|-----------|------|-----------|-----------------|-------------|
| ... | Screen/Component/Model | New/Existing/Extend | Frame name | ... |
```

### 3b. Module Structure

Define files to create or modify:

```
#### Module Structure
| File Path | Action | Responsibility |
|-----------|--------|----------------|
| ... | Create/Modify | ... |
```

Include a dependency direction summary (which module depends on which).

### 3c. Data Flow

```
#### Data Flow
- **Models**: Data structures needed
- **State Management**: How state flows through the feature
- **API Integration**: Endpoints, request/response models (if applicable)
- **Persistence**: Local storage needs (if applicable)
```

### 3d. Team Composition (auto-determined)

Based on the complexity and file structure, determine the team:

**2 teammates** (Simple UI):
| Teammate | Role | Owned Paths |
|----------|------|-------------|
| UI Developer | View/Screen implementation | {ui_paths} |
| Test Writer | Tests for all created code | {test_paths} |

**3 teammates** (Feature with logic):
| Teammate | Role | Owned Paths |
|----------|------|-------------|
| UI Developer | Views, Screens, Components | {ui_paths} |
| Feature Developer | ViewModel/Presenter, UseCase, Domain logic | {logic_paths} |
| Test Writer | Unit + Integration tests | {test_paths} |

**4 teammates** (Complex feature):
| Teammate | Role | Owned Paths |
|----------|------|-------------|
| UI Developer | Views, Screens, Components | {ui_paths} |
| Feature Developer | ViewModel/Presenter, UseCase, Domain logic | {logic_paths} |
| Data Layer Developer | Repository, Network, Persistence | {data_paths} |
| Test Writer | Unit + Integration + UI tests | {test_paths} |

**5 teammates** (Large feature):
| Teammate | Role | Owned Paths |
|----------|------|-------------|
| UI Developer | Views, Screens, Components | {ui_paths} |
| Feature Developer | ViewModel/Presenter, UseCase, Domain logic | {logic_paths} |
| Data Layer Developer | Repository, Network, Persistence | {data_paths} |
| Test Writer | Unit + Integration + UI tests | {test_paths} |
| Reviewer | Cross-module review + integration verification | All (read-only during impl) |

CRITICAL: Every teammate's owned paths must be non-overlapping to prevent file conflicts.

### 3e. Task Breakdown

Create a numbered task list with dependencies:

```
#### Task Breakdown
| # | Task | Assignee | Depends On | Description |
|---|------|----------|------------|-------------|
| 1 | ... | ... | - | ... |
| 2 | ... | ... | #1 | ... |
```

Aim for 4-6 tasks per teammate. Tasks should be self-contained with clear deliverables.

### 3f. Risks & Estimates

```
#### Estimated Complexity
- **Overall**: {Low/Medium/High}
- **Rationale**: ...

#### Risks & Considerations
- ...
```

## Phase 4: User Approval

Present the complete implementation plan from Phase 3 to the user.

Then use AskUserQuestion to ask:

**"Implementation plan is ready. How do you want to proceed?"**
- "Approve and Execute" — Proceed to Phase 5 (form agent team and implement)
- "Modify Plan" — Tell me what to change (loops back to Phase 3 adjustments)
- "Save Plan Only" — Output the plan and stop here

If "Modify Plan": ask what changes are needed, apply them, and present the updated plan again.
If "Save Plan Only": output the final plan in a clean format and end.
If "Approve and Execute": proceed to Phase 5.

## Phase 5: Team Formation & Execution

### 5a. Create Team

Use TeamCreate to create the agent team named after the feature (e.g., `impl-{feature-slug}`).

### 5b. Create Tasks

Use TaskCreate for each task from Phase 3e. Set up dependencies using TaskUpdate with `addBlockedBy`.

### 5c. Spawn Teammates

For each teammate in the plan, spawn using the Task tool with:
- `team_name`: the team name from 5a
- `name`: the teammate role name (e.g., "ui-developer", "feature-developer")
- `subagent_type`: "general-purpose"
- `mode`: "plan" (require plan approval before implementation)

Each teammate's spawn prompt must include:
1. Their specific role and responsibility
2. The FULL list of files they own (with ONLY-edit-under rule)
3. Relevant design information from Phase 2 (Figma extraction)
4. Architecture patterns to follow (from codebase analysis)
5. Their assigned tasks from the task list
6. How to coordinate with other teammates (shared interfaces, dependencies)

Example spawn prompt structure:
```
You are the {Role} for implementing {feature name}.

## Your Responsibility
{role description}

## File Ownership
You may ONLY create/edit files under these paths:
{list of paths}
Do NOT edit files outside these paths.

## Design Reference
{extracted Figma design info relevant to this role}

## Architecture
{project patterns to follow}

## Your Tasks
{numbered task list for this teammate}

## Coordination
- If you need to change a shared interface (protocol, public API), report to the lead FIRST
- When your work depends on another teammate's output, check task status before proceeding
```

### 5d. Assign Tasks

Use TaskUpdate to assign initial tasks to each teammate by setting `owner`.

### 5e. Monitor Execution

- Wait for teammates to submit their plans (plan_mode_required)
- Review and approve/reject plans via SendMessage with plan_approval_response
- Monitor task completion via TaskList
- Resolve blocking issues or reassign tasks as needed
- When all tasks are complete, proceed to Phase 6

## Phase 6: Final Report

After all teammates complete their work, synthesize:

```
### Implementation Summary

**Feature**: {name}
**Figma**: {url}
**Tech Stack**: {stack}
**Team Size**: {count} teammates

#### Completed Work
| Teammate | Role | Files Created/Modified | Tasks Completed |
|----------|------|----------------------|-----------------|
| ... | ... | ... | ... |

#### All Files Changed
| File | Action | Purpose |
|------|--------|---------|
| ... | Create/Modify | ... |

#### Architecture Decisions Made
- ...

#### Test Coverage
| Type | Count | Status |
|------|-------|--------|
| Unit | ... | Pass/Fail |
| Integration | ... | Pass/Fail |
| UI | ... | Pass/Fail |

#### Remaining Work / Known Issues
- ...

#### Next Steps
- ...
```

After the report, gracefully shut down all teammates and clean up the team.
