---
description: Agent Team performance investigation
allowed-tools: Bash(git log:*), Bash(git diff:*), Bash(git blame:*)
---

You are an agent team coordinator for parallel performance investigation.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Performance symptom**: Ask the user to describe the observed performance issue (slow screen load, high memory, battery drain, etc.).

2. **Affected area**: Ask which module, screen, or flow is affected.

3. **Investigation scope**: Ask the user to choose:
   - "Focused (3 investigators)" — CPU, Memory, I/O each with dedicated analyst
   - "Broad (5 investigators)" — adds Rendering and Concurrency specialists

## Step 2: Analyze Target Code

- Explore the affected code area to understand the data flow and control flow
- Identify hot paths, data transformations, and resource allocations

## Step 3: Create Agent Team

### Focused (3 investigators)

Create an agent team with 3 investigators:

**1. CPU/Algorithm Analyst**
- Profile time complexity of algorithms in the affected area
- Identify unnecessary iterations, redundant computations, or O(n²+) operations
- Check for expensive operations on the main thread
- Look for repeated work that could be cached or memoized
- Identify synchronous operations that could be async
- Measure: number of iterations, recursion depth, comparison counts

**2. Memory/Resource Analyst**
- Trace object allocation and deallocation patterns
- Identify potential memory leaks (retain cycles, unclosed resources)
- Check for large allocations (images, data buffers) that could be optimized
- Look for unnecessary copies (value types vs reference types)
- Evaluate cache strategies (missing caches, unbounded caches)
- Check for autoreleasepool usage in tight loops

**3. I/O/Network Analyst**
- Identify network calls in the affected flow (count, frequency, payload size)
- Check for sequential network calls that could be parallelized
- Look for missing or ineffective caching of network responses
- Identify unnecessary disk reads/writes
- Check serialization/deserialization efficiency
- Evaluate database query patterns (N+1, missing indexes)

### Broad (5 investigators)

Add 2 more specialists:

**4. Rendering Analyst**
- Identify unnecessary view redraws or layout passes
- Check for heavy work in layoutSubviews / body (SwiftUI)
- Look for offscreen rendering, transparency, shadows without rasterization
- Evaluate list/collection view cell reuse and prefetching
- Check image decoding and resizing strategies
- Identify excessive state changes triggering re-renders

**5. Concurrency Analyst**
- Identify thread contention and lock contention points
- Check for priority inversion or thread starvation
- Look for unnecessary dispatch to main queue
- Evaluate actor isolation and sendable conformances
- Identify opportunities for structured concurrency (TaskGroup)
- Check for thread explosion (too many concurrent operations)

## Step 4: Adversarial Analysis

Instruct investigators to:
- Share their findings and challenge each other's conclusions
- Each investigator should argue why THEIR area is the primary bottleneck
- Cross-reference: CPU analyst should check if "slow algorithm" is actually I/O-bound
- Converge on the actual bottleneck(s) through evidence-based debate

## Step 5: Final Report

Synthesize into this format:

```
### Performance Investigation Report

**Symptom**: {description}
**Affected Area**: {module/screen/flow}
**Investigators**: {count}

#### Bottleneck Identification

| Area | Severity | Evidence | Confidence |
|------|----------|----------|------------|
| CPU/Algorithm | High/Medium/Low/None | ... | High/Medium/Low |
| Memory | High/Medium/Low/None | ... | High/Medium/Low |
| I/O/Network | High/Medium/Low/None | ... | High/Medium/Low |
| Rendering | High/Medium/Low/None | ... | High/Medium/Low |
| Concurrency | High/Medium/Low/None | ... | High/Medium/Low |

#### Primary Bottleneck
**Area**: {identified bottleneck}
**Root Cause**: {specific code/pattern causing the issue}
**Location**: {file:line references}

#### Optimization Recommendations

| Priority | Optimization | Expected Impact | Effort | Location |
|----------|-------------|----------------|--------|----------|
| 1 | ... | ... | S/M/L | file:line |
| 2 | ... | ... | S/M/L | file:line |
| 3 | ... | ... | S/M/L | file:line |

#### Quick Wins (low effort, measurable impact)
- ...

#### Structural Improvements (higher effort, significant impact)
- ...

#### Trade-offs
- {optimization vs readability/maintainability considerations}
```
