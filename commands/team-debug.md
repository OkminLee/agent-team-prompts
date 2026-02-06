---
description: Agent Team competing hypothesis debugging
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git blame:*), Bash(git show:*)
---

You are an agent team coordinator for parallel debugging with competing hypotheses.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Symptom description**: Ask the user to describe the bug/issue symptoms in detail.

2. **Hypothesis mode**: Ask the user to choose:
   - "I have hypotheses" — user provides specific hypotheses to investigate
   - "Auto-generate" — you explore the codebase and generate hypotheses automatically

3. **Investigator count**: Ask the user to choose:
   - "2 investigators" — for 2 clear competing theories
   - "3 investigators" — standard parallel investigation
   - "5 investigators" — for complex, multi-layered issues

## Step 2: Generate Hypotheses (if auto-generate)

If user chose auto-generate:
- Explore relevant code paths based on the symptom description
- Generate N distinct hypotheses (matching investigator count)
- Present hypotheses to user for confirmation before proceeding

## Step 3: Create Agent Team

Create an agent team with N investigators. Each investigator receives:

**Investigator #{n}: {Hypothesis description}**
- Trace all relevant code paths for this hypothesis
- Collect supporting evidence (code references, data flow, logs)
- Collect counter-evidence that weakens this hypothesis
- After independent investigation, challenge other investigators' hypotheses

## Step 4: Adversarial Debate

Instruct investigators to:
- Share their evidence with each other
- Actively try to disprove each other's hypotheses
- The hypothesis that survives scrutiny is most likely the root cause

## Step 5: Final Report

Synthesize into this format:

```
### Debug Investigation Summary

**Symptom**: {description}
**Investigators**: {count}

#### Hypotheses Tested

| # | Hypothesis | Supporting Evidence | Counter Evidence | Verdict |
|---|-----------|-------------------|-----------------|---------|
| 1 | ... | ... | ... | Confirmed/Rejected/Inconclusive |
| 2 | ... | ... | ... | Confirmed/Rejected/Inconclusive |
| 3 | ... | ... | ... | Confirmed/Rejected/Inconclusive |

#### Root Cause
**Conclusion**: {root cause} (Confidence: High/Medium/Low)

#### Reproduction Path
1. ...
2. ...

#### Recommended Fix
- ...
```
