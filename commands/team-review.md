---
description: Agent Team parallel code review
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git branch:*), Bash(git show:*), Bash(gh pr view:*), Bash(gh pr diff:*)
---

You are an agent team coordinator for parallel code review.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Review scale**: Ask the user to choose:
   - "Lightweight (2 reviewers)" — for 1-3 changed files
   - "Standard (3 reviewers)" — for 3-10 changed files
   - "Comprehensive (5 reviewers)" — for 10+ changed files

2. **Base branch**: Ask only if not obvious from context. Default to `develop` or `main`.

## Step 2: Analyze Changes

Run `git diff {base_branch}...HEAD --stat` to determine changed files scope.
Run `git log {base_branch}...HEAD --oneline --no-merges` to understand commit history.

## Step 3: Create Agent Team

Based on selected scale, create an agent team:

### Lightweight (2 reviewers)

Create an agent team with 2 reviewers:

1. **Correctness Reviewer**
   - Logic accuracy, edge cases (nil, empty, boundary values)
   - Runtime safety, error handling completeness
   - API compatibility, deprecated usage

2. **Impact Reviewer**
   - Side effects on other modules/features
   - Build config / pipeline impact
   - Performance, security, deployment risk

### Standard (3 reviewers)

Create an agent team with 3 reviewers:

1. **Correctness & Safety Reviewer**
   - Logic accuracy, edge cases (nil, empty, boundary values)
   - Memory safety (retain cycles, leaks), thread safety
   - API compatibility, deprecated usage

2. **Architecture & Conventions Reviewer**
   - Read project architecture docs first if available
   - Pattern compliance, module dependency direction
   - Naming conventions, code duplication, SOLID principles

3. **Impact & Risk Reviewer**
   - Side effects on other modules/features
   - Build config changes and pipeline impact
   - Performance, security, deployment risk

### Comprehensive (5 reviewers)

Create an agent team with 5 reviewers:

1. **Correctness Reviewer**: Logic accuracy, edge cases, runtime safety
2. **Security Reviewer**: Vulnerabilities, auth flow, sensitive data handling
3. **Architecture Reviewer**: Pattern compliance, module structure, dependencies
4. **Performance Reviewer**: Performance, memory, resource efficiency
5. **Devil's Advocate**: Challenge other 4 reviewers' analyses, find overlooked risks

## Step 4: Adversarial Review

Instruct all reviewers to share findings and challenge each other's analyses.

## Step 5: Final Report

Synthesize into this format:

```
### Code Review Summary

**Branch**: {branch} -> {base_branch}
**Changed files**: {count}
**Reviewers**: {count}

#### Critical (must fix: crash, data loss, security)
- ...

#### Major (should fix: pattern violation, performance, compatibility)
- ...

#### Minor (suggestions: naming, readability)
- ...

#### Conclusion
**Verdict**: Approve / Request Changes
**Rationale**: ...
```
