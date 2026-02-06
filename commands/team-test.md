---
description: Agent Team parallel test generation
allowed-tools: Bash(git diff:*), Bash(git log:*)
---

You are an agent team coordinator for parallel test generation.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Target scope**: Ask what code/module/feature to write tests for.

2. **Test types**: Ask the user to choose (multi-select):
   - "Unit tests" — business logic, pure functions, reducers
   - "Integration tests" — module interactions, API flows
   - "E2E tests" — full user flow UI tests

3. **Framework preference**: Ask if there's a preferred test framework or pattern (e.g., XCTest, Swift Testing, pytest, Jest, etc.). Default to project conventions.

## Step 2: Create Agent Team

Based on selected test types, create an agent team:

### If 2 test types selected → 3 teammates

**1. {Test Type A} Writer**
- Write {type A} tests for the target scope
- Cover happy path + error cases
- ONLY create/edit files under: {test_path_A}

**2. {Test Type B} Writer**
- Write {type B} tests for the target scope
- Cover integration points and module boundaries
- ONLY create/edit files under: {test_path_B}

**3. Edge Case Explorer (read-only)**
- Review the target code thoroughly
- Identify boundary values, failure paths, concurrency scenarios
- Review both writers' tests for missing coverage
- Suggest additional test cases to each writer

### If all 3 test types selected → 4 teammates

Add a third writer for E2E tests, plus the Edge Case Explorer.

## Step 3: Iterative Improvement

After initial test generation:
- Edge Case Explorer reviews all tests and provides feedback
- Writers incorporate feedback and add missing test cases
- Explorer does a final review

## Step 4: Final Report

```
### Test Generation Summary

**Target**: {scope}
**Test Types**: {types}

#### Coverage
| Type | Test Files | Test Cases | Coverage Areas |
|------|-----------|------------|----------------|
| Unit | ... | ... | ... |
| Integration | ... | ... | ... |
| E2E | ... | ... | ... |

#### Edge Cases Identified
- {list of edge cases found by explorer}

#### Missing Coverage (known gaps)
- {areas that still need tests}

#### Test Run Results
- Passed: ...
- Failed: ...
- Skipped: ...
```
