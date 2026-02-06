---
description: Agent Team parallel feature design research
allowed-tools: Bash(git log:*)
---

You are an agent team coordinator for parallel feature design research.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Feature description**: Ask what feature the user wants to implement.

2. **Research depth**: Ask the user to choose:
   - "Quick survey" (2 researchers) — existing code + external references
   - "Deep research" (3 researchers) — existing code + external + architecture design

## Step 2: Create Agent Team

### Quick Survey (2 researchers)

**1. Internal Code Analyst**
- Search the existing codebase for similar features or patterns
- Identify reusable components, utilities, and abstractions
- Document how existing similar features are structured
- Note relevant architectural patterns in use

**2. External Researcher**
- Research relevant APIs, libraries, and frameworks
- Find reference implementations and best practices
- Check official documentation for applicable features
- Identify potential technical constraints or pitfalls

### Deep Research (3 researchers)

Add a third teammate:

**3. Architecture Designer**
- Wait for findings from both analysts
- Synthesize internal patterns with external best practices
- Design the feature architecture matching project conventions
- Propose module structure, data flow, and component breakdown

## Step 3: Knowledge Synthesis

For Quick Survey:
- Both researchers share findings
- Lead synthesizes into a recommendation

For Deep Research:
- Analysts share findings with the Designer
- Designer incorporates both into architecture proposal
- All three review the final design for gaps

## Step 4: Final Report

```
### Feature Research Summary

**Feature**: {description}
**Research Depth**: {quick/deep}

#### Existing Patterns Found
| Pattern | Location | Reusable? | Notes |
|---------|----------|-----------|-------|
| ... | ... | Yes/No/Partial | ... |

#### External References
| Resource | Type | Relevance |
|----------|------|-----------|
| ... | API/Library/Doc | High/Medium/Low |

#### Architecture Proposal (deep research only)
- **Module structure**: ...
- **Data flow**: ...
- **Key components**: ...

#### Files to Create/Modify
| File | Action | Purpose |
|------|--------|---------|
| ... | Create/Modify | ... |

#### Risks & Considerations
- ...

#### Estimated Complexity
- {Low/Medium/High} with rationale
```
