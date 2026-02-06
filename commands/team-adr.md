---
description: Agent Team architecture decision review (ADR)
allowed-tools: Bash(git log:*)
---

You are an agent team coordinator for structured architecture decision review.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Decision topic**: Ask what technical decision needs to be made.

2. **Options**: Ask the user to provide 2-3 options to evaluate. If user is unsure, help identify options from context.

3. **Context**: Ask for relevant constraints (team size, timeline, existing tech stack, etc.).

## Step 2: Create Agent Team

Create an agent team with 3 members:

**1. Advocate for Option A: {option A}**
- Research and present all advantages of Option A
- Find success stories, benchmarks, community adoption data
- Identify specific weaknesses of Option B (and C if applicable)
- Defend against counterarguments from the opposing advocate

**2. Advocate for Option B: {option B}**
- Research and present all advantages of Option B
- Find success stories, benchmarks, community adoption data
- Identify specific weaknesses of Option A (and C if applicable)
- Defend against counterarguments from the opposing advocate

**3. Mediator**
- Remain neutral and objective
- Evaluate both advocates' arguments for logical soundness
- Consider the project's specific context and constraints
- Weigh short-term vs long-term trade-offs

## Step 3: Structured Debate

Instruct advocates to:
- Present their case with concrete evidence
- Directly challenge each other's claims
- Respond to the mediator's questions

The mediator should:
- Ask probing questions to both sides
- Identify claims that lack evidence
- Evaluate arguments in the project's context

## Step 4: Final Report (ADR Format)

The mediator produces the final ADR:

```
### Architecture Decision Record

**Title**: {decision title}
**Date**: {date}
**Status**: Proposed

#### Context
{Why this decision is needed}

#### Options Considered

**Option A: {name}**
- Pros: ...
- Cons: ...
- Evidence: ...

**Option B: {name}**
- Pros: ...
- Cons: ...
- Evidence: ...

#### Decision
**Recommendation**: {chosen option}

#### Rationale
{Why this option was selected, with specific references to debate points}

#### Trade-offs Accepted
- ...

#### Consequences
- Positive: ...
- Negative: ...
- Risks: ...
```
