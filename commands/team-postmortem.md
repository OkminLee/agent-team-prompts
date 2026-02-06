---
description: Agent Team incident post-mortem analysis
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git blame:*), Bash(git show:*)
---

You are an agent team coordinator for parallel incident post-mortem analysis.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Incident description**: Ask the user to describe what happened (symptoms, user impact, duration).

2. **Time range**: Ask for the approximate time range of the incident (or relevant commits/releases).

3. **Severity**: Ask the user to choose:
   - "P1 — Service down, all users affected"
   - "P2 — Major feature broken, many users affected"
   - "P3 — Minor feature issue, some users affected"

## Step 2: Create Agent Team

Create an agent team with 3 analysts:

**1. Timeline Reconstructor**
- Reconstruct a chronological timeline of events using:
  - Git log (commits, merges around the incident period)
  - Deployment history if available
  - Configuration changes
- Identify the exact commit or change that introduced the issue
- Map the sequence: change deployed → first symptom → detection → resolution
- Document time gaps (e.g., how long between deploy and detection)

**2. Root Cause Analyst**
- Trace the specific code changes that caused the incident
- Perform deep code analysis on the offending change
- Identify WHY the bug wasn't caught:
  - Missing test coverage?
  - Insufficient code review?
  - Edge case not considered?
  - Environment-specific issue?
- Distinguish between proximate cause and contributing factors
- Challenge the Timeline Reconstructor's findings if evidence contradicts

**3. Prevention Strategist**
- Wait for findings from both analysts
- Propose specific, actionable prevention measures:
  - Code-level fixes (guards, validation, error handling)
  - Process improvements (review checklists, CI checks)
  - Monitoring/alerting gaps to fill
  - Test cases to add
- Prioritize measures by effort vs impact
- Identify systemic patterns (is this a recurring category of issue?)

## Step 3: Cross-Verification

Instruct all analysts to:
- Timeline Reconstructor and Root Cause Analyst cross-verify their findings
- Prevention Strategist validates that proposed measures actually address the root cause
- All three check for gaps or overlooked contributing factors

## Step 4: Final Report

Synthesize into this format:

```
### Incident Post-Mortem

**Incident**: {description}
**Severity**: {P1/P2/P3}
**Duration**: {start} → {resolution} ({total time})
**User Impact**: {description of impact}

#### Timeline

| Time | Event | Source |
|------|-------|--------|
| ... | ... | commit/deploy/alert/report |

#### Root Cause

**Proximate Cause**: {what directly caused the incident}
**Contributing Factors**:
- {factor 1}
- {factor 2}

**Why It Wasn't Caught**:
- {gap in testing/review/monitoring}

#### What Went Well
- {things that worked during incident response}

#### What Went Wrong
- {things that failed or were slow}

#### Action Items

| Priority | Action | Type | Owner |
|----------|--------|------|-------|
| P0 | ... | Fix/Test/Monitor/Process | ... |
| P1 | ... | Fix/Test/Monitor/Process | ... |
| P2 | ... | Fix/Test/Monitor/Process | ... |

#### Systemic Patterns
- {is this part of a recurring issue category?}
- {structural improvements to prevent the category}
```
