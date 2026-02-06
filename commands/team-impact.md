---
description: Agent Team change impact analysis
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git blame:*)
---

You are an agent team coordinator for parallel change impact analysis.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Change target**: Ask what file, module, or API is being changed.

2. **Change description**: Ask how it will change (rename, signature change, removal, behavior change, etc.).

## Step 2: Create Agent Team

Create an agent team with 3 analysts:

**1. Direct Dependency Analyst**
- Find all locations that directly call/import/inherit the change target
- List every file:line that will break at compile time
- Map the full call graph from the change target outward

**2. Indirect Impact Analyst**
- Trace cascading effects through modules, config files, build pipelines
- Check runtime behavior changes (dependency injection, reflection, config values)
- Identify protocol conformances, generic constraints, or type aliases affected
- Check CI/CD, build scripts, and deployment configs

**3. Test Impact Analyst**
- List existing tests that will break or need updates
- Identify test scenarios that are currently missing for the change
- Check test fixtures, mocks, and stubs that reference the change target

## Step 3: Cross-Verification

Instruct analysts to:
- Share findings with each other
- Check if any impacted paths were missed by other analysts
- Verify each other's severity assessments

## Step 4: Final Report

Synthesize into this format:

```
### Impact Analysis Summary

**Change Target**: {target}
**Change Type**: {description}

#### Breaking Changes (compile-time)
| File | Line | Description | Severity |
|------|------|-------------|----------|
| ... | ... | ... | High/Medium/Low |

#### Runtime Impact
| Area | Description | Severity |
|------|-------------|----------|
| ... | ... | High/Medium/Low |

#### Test Impact
- Tests that will break: {count}
- Tests that need updates: {count}
- Missing test coverage: {list}

#### Impact Scope
- **High impact**: {list of heavily affected modules}
- **Medium impact**: {list of moderately affected modules}
- **Low impact**: {list of minimally affected modules}

#### Recommended Migration Order
1. ...
2. ...
3. ...
```
