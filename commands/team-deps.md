---
description: Agent Team dependency audit
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(cat Package.swift), Bash(cat Podfile*), Bash(cat package.json), Bash(cat Cartfile*), Bash(cat Gemfile*), Bash(cat requirements*.txt), Bash(cat pyproject.toml), Bash(cat go.mod), Bash(cat Cargo.toml), Bash(cat *.resolved), Bash(cat *.lock)
---

You are an agent team coordinator for parallel dependency audit.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Audit scope**: Ask the user to choose:
   - "Full audit" — all dependencies
   - "Specific dependencies" — user specifies which ones to audit

2. **Priority focus**: Ask the user to choose (multi-select):
   - "Security" — CVEs, known vulnerabilities
   - "License" — license compatibility, commercial use restrictions
   - "Freshness" — outdated packages, available updates, alternatives

## Step 2: Identify Dependencies

- Read the project's dependency manifest (Package.swift, Podfile, package.json, etc.)
- List all direct and transitive dependencies with current versions

## Step 3: Create Agent Team

Create an agent team with 3 auditors:

**1. Security Auditor**
- Search for known vulnerabilities (CVEs) in each dependency
- Check each dependency's maintenance status (last commit, open security issues)
- Identify dependencies that handle sensitive data (auth, crypto, networking)
- Assess supply chain risk (dependency popularity, maintainer count, release frequency)
- Flag dependencies with known security advisories
- Rate each dependency: Safe / Monitor / Action Required / Critical

**2. License Auditor**
- Identify the license of each dependency
- Check license compatibility with the project's license
- Identify restrictive licenses (GPL, AGPL) that may affect distribution
- Check for license conflicts between dependencies
- Verify that license obligations are met (attribution, source disclosure)
- Flag any dependencies with unclear or missing license information

**3. Freshness Auditor**
- Compare current versions against latest available versions
- Identify dependencies that are significantly behind (2+ major versions)
- Check for deprecated dependencies and their recommended replacements
- Evaluate migration effort for major version upgrades
- Identify dependencies with declining community support
- Suggest alternative libraries where a switch would be beneficial

## Step 4: Cross-Verification

Instruct auditors to:
- Security Auditor flags license issues found during security research
- License Auditor flags security concerns noticed during license review
- Freshness Auditor considers security and license changes in newer versions
- All three verify each other's findings for completeness

## Step 5: Final Report

Synthesize into this format:

```
### Dependency Audit Report

**Project**: {name}
**Total Dependencies**: {count direct} direct, {count transitive} transitive
**Audit Date**: {date}

#### Critical Findings (action required)
- {high-priority issues that need immediate attention}

#### Security Assessment

| Dependency | Version | Status | CVEs | Risk |
|-----------|---------|--------|------|------|
| ... | ... | Safe/Monitor/Action/Critical | ... | ... |

#### License Assessment

| Dependency | License | Compatible? | Obligations |
|-----------|---------|-------------|-------------|
| ... | ... | Yes/No/Review | ... |

**License conflicts found**: {count}

#### Freshness Assessment

| Dependency | Current | Latest | Behind | Migration Effort |
|-----------|---------|--------|--------|-----------------|
| ... | ... | ... | {versions} | S/M/L |

**Deprecated dependencies**: {list}

#### Recommended Actions

| Priority | Dependency | Action | Reason |
|----------|-----------|--------|--------|
| P0 | ... | Upgrade/Replace/Remove | ... |
| P1 | ... | Upgrade/Replace/Remove | ... |
| P2 | ... | Upgrade/Replace/Remove | ... |

#### Alternative Recommendations
| Current | Suggested Alternative | Reason |
|---------|----------------------|--------|
| ... | ... | ... |
```
