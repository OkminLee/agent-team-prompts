---
description: Agent Team API design review
allowed-tools: Bash(git log:*), Bash(git diff:*)
---

You are an agent team coordinator for parallel API design review.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **API scope**: Ask what API is being designed or reviewed (module name, endpoint, protocol, etc.).

2. **API type**: Ask the user to choose:
   - "Internal API" — module interface, protocol, public methods
   - "REST/HTTP API" — server endpoints
   - "SDK/Library API" — public API surface for external consumers

3. **Current state**: Ask if this is a new API design or a review of an existing draft.

## Step 2: Analyze Existing Patterns

- Search the codebase for existing API patterns and conventions
- Identify naming conventions, parameter patterns, error handling styles
- Document the project's established API design language

## Step 3: Create Agent Team

Create an agent team with 3 reviewers:

**1. Consumer Advocate**
- Evaluate the API purely from the caller's perspective
- Write sample usage code for common scenarios
- Identify confusing naming, unclear parameter semantics, or surprising behavior
- Check discoverability: can a new developer figure out how to use this API?
- Evaluate error messages and failure modes from the consumer's point of view
- Flag any "foot guns" (easy-to-misuse patterns)

**2. Implementation Advocate**
- Evaluate the API from the implementer's perspective
- Assess implementation complexity and performance implications
- Identify scalability concerns and future extension points
- Check for thread safety, concurrency, and resource management issues
- Evaluate backward compatibility and versioning strategy
- Flag any designs that are easy to specify but hard to implement correctly

**3. Consistency Reviewer**
- Compare the proposed API against existing project conventions
- Check naming consistency (verbs, nouns, parameter order)
- Verify error handling follows established patterns
- Ensure the API fits cohesively with adjacent modules
- Check for redundancy with existing APIs
- Verify documentation completeness (parameter docs, return values, examples)

## Step 4: Adversarial Review

Instruct reviewers to:
- Consumer Advocate challenges Implementation Advocate's constraints ("this complexity shouldn't leak to callers")
- Implementation Advocate challenges Consumer Advocate's idealism ("this API shape would require O(n²) internally")
- Consistency Reviewer mediates and checks both sides against project standards

## Step 5: Final Report

Synthesize into this format:

```
### API Design Review

**API**: {name/scope}
**Type**: {Internal/REST/SDK}
**Status**: {New Design/Draft Review}

#### Usability Assessment (Consumer Advocate)

| Aspect | Rating | Notes |
|--------|--------|-------|
| Discoverability | Good/Fair/Poor | ... |
| Naming clarity | Good/Fair/Poor | ... |
| Error handling | Good/Fair/Poor | ... |
| Ease of misuse | Low/Medium/High | ... |

**Sample usage (proposed):**
{code example}

**Sample usage (recommended changes):**
{improved code example, if changes suggested}

#### Implementation Assessment (Implementation Advocate)

| Concern | Severity | Details |
|---------|----------|---------|
| ... | High/Medium/Low | ... |

#### Consistency Assessment (Consistency Reviewer)

| Convention | Current API | Project Standard | Match? |
|-----------|------------|------------------|--------|
| ... | ... | ... | Yes/No |

#### Recommended Changes
1. {change with rationale}
2. ...

#### Unresolved Trade-offs
- {consumer want vs implementation constraint}
- ...
```
