---
description: Agent Team parallel module refactoring
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git status:*)
---

You are an agent team coordinator for parallel refactoring across modules.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Refactoring goal**: Ask what the refactoring aims to achieve.

2. **Scope**: Ask which modules/directories are involved.

3. **Teammate count**: Ask the user to choose:
   - "2 teammates" — for 2 modules
   - "3 teammates" — for 2 modules + tests
   - "4 teammates" — for 3 modules + tests

   Recommend matching teammate count to module count + 1 (for tests).

## Step 2: Assign File Ownership

CRITICAL: Each teammate MUST own a non-overlapping set of file paths.

Use AskUserQuestion to confirm the path assignments with the user:

| Teammate | Owned Paths | Responsibility |
|----------|-------------|----------------|
| Module A | {path_A}/** | {specific changes} |
| Module B | {path_B}/** | {specific changes} |
| Test | {test_path}/** | Update/create tests |

## Step 3: Create Agent Team

Create an agent team. Each teammate receives:

**{Role}: {Module Name}**
- ONLY edit files under: {assigned path}
- Task: {specific refactoring instructions}
- If you need to change a shared interface (protocol, public API), report to the lead FIRST
- Do NOT edit files outside your assigned path

## Step 4: Cross-Verification

After all teammates complete their work:
- Each teammate reviews the other teammates' changes for compatibility
- Verify no compile errors across module boundaries
- Check that shared interfaces remain consistent

## Step 5: Final Report

```
### Refactoring Summary

**Goal**: {description}
**Teammates**: {count}

#### Changes by Module
| Module | Files Changed | Lines Added | Lines Removed |
|--------|--------------|-------------|---------------|
| ... | ... | ... | ... |

#### Shared Interface Changes
- {list of protocol/API changes, if any}

#### Verification
- [ ] Cross-module compatibility verified
- [ ] No circular dependencies introduced
- [ ] Tests passing

#### Notes
- ...
```
