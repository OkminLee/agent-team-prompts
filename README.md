# Agent Team Prompts

Claude Code [Agent Teams](https://code.claude.com/docs/en/agent-teams) plugin with 7 slash commands for parallel workflows.

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

```bash
claude plugin add /path/to/agent-team-prompts
```

## Commands

| Command | Description | Teammates | Use When |
|---------|-------------|-----------|----------|
| `/team-review` | Parallel code review | 2 / 3 / 5 | PR review, branch review |
| `/team-debug` | Competing hypothesis debugging | 2 / 3 / 5 | Bugs with unclear root cause |
| `/team-impact` | Change impact analysis | 3 | Before modifying shared code |
| `/team-adr` | Architecture decision review | 3 | Choosing between tech options |
| `/team-refactor` | Parallel module refactoring | 2-4 | Cross-module refactoring |
| `/team-test` | Parallel test generation | 3-4 | Expanding test coverage |
| `/team-research` | Feature design research | 2-3 | Pre-implementation research |

## Usage

### With arguments

```
/team-review PR #142
/team-debug App crashes after background resume
/team-impact Renaming UserService.login() to UserService.authenticate()
/team-adr State management: Redux vs Zustand vs Jotai
/team-refactor Migrate auth module to Clean Architecture
/team-test Payment module test coverage
/team-research Implement push notification deep linking
```

### Without arguments

All commands support interactive mode. Run the command without arguments and it will ask clarifying questions via `AskUserQuestion`.

```
/team-review
→ Asks: review scale (2/3/5 reviewers), base branch

/team-debug
→ Asks: symptom description, hypothesis mode (manual/auto), investigator count

/team-impact
→ Asks: change target, change description

/team-adr
→ Asks: decision topic, options, constraints

/team-refactor
→ Asks: goal, scope, teammate count, path assignments

/team-test
→ Asks: target scope, test types, framework preference

/team-research
→ Asks: feature description, research depth (quick/deep)
```

## Command Details

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

## When NOT to Use Agent Teams

Agent teams add coordination overhead and consume more tokens. Prefer a single session or subagents for:

- Single-file bug fixes
- Simple UI changes
- Boilerplate generation
- Clear, sequential tasks
- Tasks requiring same-file edits by multiple agents

## License

MIT
