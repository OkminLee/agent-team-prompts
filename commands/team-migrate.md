---
description: Agent Team migration planning
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git show:*)
---

You are an agent team coordinator for parallel migration planning.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Migration type**: Ask the user to choose:
   - "Language migration" — e.g., Objective-C to Swift, JavaScript to TypeScript
   - "Framework migration" — e.g., UIKit to SwiftUI, REST to GraphQL
   - "Architecture migration" — e.g., MVC to MVVM, Monolith to Modules
   - "Version migration" — e.g., iOS 16 to iOS 17 APIs, Swift 5 to Swift 6

2. **Scope**: Ask which modules/directories are in scope for migration.

3. **Constraints**: Ask for timeline, backward compatibility requirements, or phased rollout needs.

## Step 2: Analyze Current State

- Explore the codebase to understand the scope of migration
- Identify files, patterns, and dependencies related to the migration target
- Count the volume of code to be migrated

## Step 3: Create Agent Team

Create an agent team with 3 analysts:

**1. Compatibility Analyst**
- Classify every relevant file/component into:
  - Ready to migrate (no blockers)
  - Needs preparation (minor changes required first)
  - Cannot migrate yet (hard dependencies on old system)
- Identify APIs, protocols, and patterns that have direct equivalents in the target
- Identify APIs, protocols, and patterns with NO equivalent (need custom solutions)
- Map inter-module dependencies that affect migration order

**2. Risk Analyst**
- Identify features that may break during migration
- Trace critical user flows through code that will change
- Assess regression risk per module (High/Medium/Low)
- Identify areas with insufficient test coverage for safe migration
- Flag third-party dependencies that may conflict with the target

**3. Migration Planner**
- Wait for findings from both analysts
- Design a phased migration order that minimizes risk
- Define coexistence strategy (bridging layers, adapters, feature flags)
- Propose milestone checkpoints with verification criteria
- Estimate effort per phase (Small/Medium/Large)

## Step 4: Cross-Verification

Instruct all analysts to:
- Review each other's classifications and risk assessments
- Challenge migration order assumptions
- Verify that the coexistence strategy handles edge cases

## Step 5: Final Report

Synthesize into this format:

```
### Migration Plan

**Type**: {migration type}
**From**: {source} → **To**: {target}
**Scope**: {modules/directories}

#### Compatibility Assessment

| Component | Status | Blockers | Effort |
|-----------|--------|----------|--------|
| ... | Ready/Needs Prep/Blocked | ... | S/M/L |

#### Risk Assessment

| Area | Risk Level | Reason | Mitigation |
|------|-----------|--------|------------|
| ... | High/Medium/Low | ... | ... |

#### Migration Phases

**Phase 1: {name}** (Effort: {S/M/L})
- Scope: ...
- Coexistence strategy: ...
- Verification: ...

**Phase 2: {name}** (Effort: {S/M/L})
- Scope: ...
- Dependencies: Phase 1 complete
- Verification: ...

**Phase 3: {name}** (Effort: {S/M/L})
- Scope: ...
- Dependencies: Phase 2 complete
- Verification: ...

#### Coexistence Strategy
- Bridging approach: ...
- Adapter patterns needed: ...
- Feature flags: ...

#### Prerequisites
- [ ] {required before starting}
- [ ] ...

#### Known Gaps
- {things that cannot be migrated with current approach}
```
