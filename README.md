# Agent Team Prompts

Claude Code [Agent Teams](https://code.claude.com/docs/en/agent-teams) plugin with 13 slash commands for parallel workflows.

## Prerequisites

Enable Agent Teams in `~/.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  },
  "teammateMode": "in-process"
}
```

## Installation

### From GitHub (recommended)

```shell
# 1. Add marketplace
/plugin marketplace add OkminLee/agent-team-prompts

# 2. Install plugin
/plugin install agent-team-prompts@agent-team-prompts
```

Or via CLI:

```bash
claude plugin marketplace add OkminLee/agent-team-prompts
claude plugin install agent-team-prompts@agent-team-prompts
```

### Local (development / testing)

```bash
claude --plugin-dir /path/to/agent-team-prompts
```

> **Note:** `--plugin-dir` loads the plugin for the current session only. For permanent installation, use the marketplace method above.

## Commands

Plugin commands are namespaced: `/agent-team-prompts:team-review`. With `--plugin-dir`, the same namespace applies.

| Command | Description | Teammates | Use When |
|---------|-------------|-----------|----------|
| `team-plan` | Implementation planning from Figma + requirements | 2-5 | New feature implementation with Figma designs |
| `team-review` | Parallel code review | 2 / 3 / 5 | PR review, branch review |
| `team-debug` | Competing hypothesis debugging | 2 / 3 / 5 | Bugs with unclear root cause |
| `team-impact` | Change impact analysis | 3 | Before modifying shared code |
| `team-adr` | Architecture decision review | 3 | Choosing between tech options |
| `team-refactor` | Parallel module refactoring | 2-4 | Cross-module refactoring |
| `team-test` | Parallel test generation | 3-4 | Expanding test coverage |
| `team-research` | Feature design research | 2-3 | Pre-implementation research |
| `team-migrate` | Migration planning | 3 | Large-scale codebase migration |
| `team-postmortem` | Incident post-mortem | 3 | After production incidents |
| `team-api-design` | API design review | 3 | Designing or reviewing APIs |
| `team-deps` | Dependency audit | 3 | Security, license, freshness check |
| `team-perf` | Performance investigation | 3 / 5 | Diagnosing performance issues |
| `team-compliance` | Compliance & guidelines check | 3-5 | HIG, accessibility, App Store, privacy |

## Usage

### With arguments

```
/agent-team-prompts:team-plan https://www.figma.com/file/abc123 Implement user profile screen for iOS
/agent-team-prompts:team-debug App crashes after background resume
/agent-team-prompts:team-impact Renaming UserService.login() to UserService.authenticate()
/agent-team-prompts:team-adr State management: Redux vs Zustand vs Jotai
/agent-team-prompts:team-refactor Migrate auth module to Clean Architecture
/agent-team-prompts:team-test Payment module test coverage
/agent-team-prompts:team-research Implement push notification deep linking
/agent-team-prompts:team-migrate UIKit to SwiftUI migration for Settings module
/agent-team-prompts:team-postmortem Login failure incident on 2024-01-15 release
/agent-team-prompts:team-api-design Review PaymentService public API surface
/agent-team-prompts:team-deps Full dependency security and license audit
/agent-team-prompts:team-perf Profile screen takes 3+ seconds to load
/agent-team-prompts:team-compliance Check onboarding flow for App Store submission
/agent-team-prompts:team-plan https://www.figma.com/file/abc123 Implement user profile screen for iOS
```

### Without arguments

All commands support interactive mode. Run the command without arguments and it will ask clarifying questions via `AskUserQuestion`.

```
/agent-team-prompts:team-plan
→ Asks: Figma URL, requirements, tech stack, complexity

/agent-team-prompts:team-review
→ Asks: review scale (2/3/5 reviewers), base branch

/agent-team-prompts:team-debug
→ Asks: symptom description, hypothesis mode (manual/auto), investigator count

/agent-team-prompts:team-impact
→ Asks: change target, change description

/agent-team-prompts:team-adr
→ Asks: decision topic, options, constraints

/agent-team-prompts:team-refactor
→ Asks: goal, scope, teammate count, path assignments

/agent-team-prompts:team-test
→ Asks: target scope, test types, framework preference

/agent-team-prompts:team-research
→ Asks: feature description, research depth (quick/deep)

/agent-team-prompts:team-migrate
→ Asks: migration type, scope, constraints

/agent-team-prompts:team-postmortem
→ Asks: incident description, time range, severity

/agent-team-prompts:team-api-design
→ Asks: API scope, API type, current state

/agent-team-prompts:team-deps
→ Asks: audit scope, priority focus (security/license/freshness)

/agent-team-prompts:team-perf
→ Asks: symptom, affected area, investigation scope (3/5 investigators)

/agent-team-prompts:team-compliance
→ Asks: check scope, compliance domains (HIG/accessibility/App Store/privacy)
```

## Command Details

### `/team-plan` — Implementation Planning from Figma

Takes a Figma design URL and requirements, analyzes the design and codebase, produces a detailed implementation plan, then forms an agent team to execute it.

**Workflow (3-phase pipeline):**

| Phase | Description |
|-------|-------------|
| Plan | Analyze Figma design + codebase → generate implementation plan with team composition |
| Approve | User reviews the plan → approve, modify, or stop |
| Execute | Auto-create agent team → teammates implement with plan approval required |

**Figma analysis:** Uses the `/figma:implement-design` skill when available, falls back to WebFetch, then manual description.

**Team auto-composition:**

| Complexity | Teammates | Roles |
|------------|-----------|-------|
| Simple UI | 2 | UI Developer, Test Writer |
| Feature with logic | 3 | UI Developer, Feature Developer, Test Writer |
| Complex feature | 4 | UI + Feature + Data Layer + Test |
| Large feature | 5 | UI + Feature + Data + Test + Reviewer |

**Key feature:** Each teammate must submit their implementation plan to the lead for approval before writing code (`plan_mode_required`). File ownership is strictly non-overlapping to prevent conflicts.

**Output:** Implementation plan (component breakdown, module structure, task list), then implementation summary with all files changed.

### `/team-review` — Parallel Code Review

Spawns independent reviewers with distinct focus areas. Each reviewer analyzes the diff, then they challenge each other's findings.

**Scale options:**

| Scale | Reviewers | Best For |
|-------|-----------|----------|
| Lightweight | 2 (Correctness, Impact) | 1-3 changed files |
| Standard | 3 (Safety, Architecture, Risk) | 3-10 changed files |
| Comprehensive | 5 (Correctness, Security, Architecture, Performance, Devil's Advocate) | 10+ changed files |

**Output:** Severity-rated findings (Critical/Major/Minor) with Approve/Request Changes verdict.

### `/team-debug` — Competing Hypothesis Debugging

Assigns each investigator a different hypothesis to pursue independently. After investigation, they attempt to disprove each other's theories.

**Key feature:** Auto-generate mode explores the codebase and proposes hypotheses when you don't have specific theories yet.

**Output:** Hypothesis comparison table, root cause with confidence level, reproduction path, fix recommendation.

### `/team-impact` — Change Impact Analysis

Three analysts trace different layers of impact: direct dependencies (compile-time), indirect effects (runtime), and test coverage.

**Output:** Breaking changes list with file:line references, severity-rated impact table, migration order.

### `/team-adr` — Architecture Decision Review

Structured debate format: two advocates argue for competing options while a neutral mediator evaluates arguments and produces an ADR.

**Output:** Architecture Decision Record with options, trade-offs, and rationale.

### `/team-refactor` — Parallel Module Refactoring

Each teammate owns a non-overlapping set of file paths to prevent conflicts. Shared interface changes require lead approval.

**Important:** Always confirm path assignments before execution.

**Output:** Per-module change summary, shared interface changes, compatibility verification.

### `/team-test` — Parallel Test Generation

Writers create different test types in parallel while an Edge Case Explorer reviews for missing coverage and suggests additional scenarios.

**Output:** Coverage table by test type, edge cases identified, known gaps.

### `/team-research` — Feature Design Research

Internal code analyst and external researcher work in parallel. In deep mode, a designer synthesizes both into an architecture proposal.

**Output:** Reusable patterns found, external references, architecture proposal, file list, complexity estimate.

### `/team-migrate` — Migration Planning

Three analysts work in parallel: Compatibility Analyst classifies migratability, Risk Analyst assesses regression danger, and Migration Planner designs a phased rollout based on both analyses.

**Supports:** Language, framework, architecture, and version migrations.

**Output:** Compatibility assessment, risk matrix, phased migration plan with coexistence strategy.

### `/team-postmortem` — Incident Post-Mortem

Timeline Reconstructor traces events chronologically while Root Cause Analyst digs into the code changes. Prevention Strategist synthesizes both into actionable prevention measures.

**Key feature:** Cross-verification between timeline and root cause to ensure the real cause is identified, not just the proximate one.

**Output:** Event timeline, root cause with contributing factors, prioritized action items.

### `/team-api-design` — API Design Review

Consumer Advocate evaluates usability, Implementation Advocate assesses feasibility, and Consistency Reviewer checks against project conventions. Advocates debate trade-offs adversarially.

**Supports:** Internal APIs (protocols/modules), REST/HTTP APIs, and SDK/Library APIs.

**Output:** Usability/implementation/consistency assessments, recommended changes, unresolved trade-offs.

### `/team-deps` — Dependency Audit

Three auditors run in parallel: Security (CVEs, supply chain risk), License (compatibility, obligations), and Freshness (outdated packages, alternatives).

**Output:** Per-dependency security/license/freshness ratings, prioritized action items, alternative recommendations.

### `/team-perf` — Performance Investigation

Investigators analyze different performance dimensions (CPU, Memory, I/O) and debate which area is the actual bottleneck. Broad mode adds Rendering and Concurrency specialists.

**Key feature:** Adversarial debate forces investigators to prove their area is the real bottleneck with evidence.

**Output:** Bottleneck identification with confidence levels, prioritized optimization recommendations.

### `/team-compliance` — Compliance & Guidelines Check

Domain-specific reviewers check against Apple HIG, Accessibility, App Store Guidelines, and/or Privacy requirements. A Synthesizer resolves cross-domain conflicts.

**Supports:** Selective domain checking (choose which guidelines to verify).

**Output:** Severity-rated findings by domain, cross-domain conflicts, prioritized action plan.

## When NOT to Use Agent Teams

Agent teams add coordination overhead and consume more tokens. Prefer a single session or subagents for:

- Single-file bug fixes
- Simple UI changes
- Boilerplate generation
- Clear, sequential tasks
- Tasks requiring same-file edits by multiple agents

## License

MIT
