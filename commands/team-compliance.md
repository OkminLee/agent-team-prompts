---
description: Agent Team compliance and guidelines check
allowed-tools: Bash(git log:*), Bash(git diff:*)
---

You are an agent team coordinator for parallel compliance and guideline verification.

## Input

$ARGUMENTS

## Step 1: Gather Parameters

If $ARGUMENTS is empty or lacks required info, use AskUserQuestion to collect:

1. **Check scope**: Ask the user to choose what to check:
   - "Specific screen/feature" — check a particular UI or feature
   - "Full app scan" — scan the entire project
   - "Recent changes" — check only recent commits/PR

2. **Compliance domains**: Ask the user to choose (multi-select):
   - "HIG (Human Interface Guidelines)" — Apple design guidelines
   - "Accessibility" — VoiceOver, Dynamic Type, color contrast
   - "App Store Guidelines" — rejection risk items
   - "Privacy" — data collection, tracking, permissions usage

## Step 2: Create Agent Team

Based on selected domains, create an agent team (1 reviewer per domain + 1 synthesizer):

### If 2 domains selected → 3 teammates

### If 3 domains selected → 4 teammates

### If all 4 domains selected → 5 teammates

Each domain reviewer follows their specific checklist:

**HIG Reviewer (if selected)**
- Navigation patterns match platform conventions
- Standard controls used appropriately (no custom re-implementations of system components)
- Typography follows Dynamic Type scale
- Spacing, layout, and touch targets meet minimum sizes (44pt)
- SF Symbols usage and consistency
- Dark mode / light mode support
- iPad and landscape adaptations if applicable
- Haptic feedback used appropriately

**Accessibility Reviewer (if selected)**
- All interactive elements have accessibility labels
- VoiceOver navigation order is logical
- Dynamic Type supported across all text
- Sufficient color contrast ratios (4.5:1 for text, 3:1 for large text)
- Color is not the sole means of conveying information
- Custom controls expose proper accessibility traits
- Reduce Motion respected for animations
- Switch Control and Voice Control compatibility

**App Store Guidelines Reviewer (if selected)**
- No private API usage
- No misleading metadata or functionality
- In-app purchase implementation follows guidelines (no external payment links)
- User-generated content has reporting/blocking mechanisms if applicable
- Login with Apple offered if other third-party logins exist
- No unnecessary background processing
- Proper purpose strings for all permission requests
- Content rating appropriate for the app's content

**Privacy Reviewer (if selected)**
- App Tracking Transparency implemented correctly for tracking
- Privacy manifest (PrivacyInfo.xcprivacy) complete and accurate
- Required reason APIs declared properly
- Minimum data collection principle followed
- User data deletion capability provided
- Third-party SDK privacy implications assessed
- Keychain usage secure (appropriate accessibility level)
- No sensitive data in logs, crash reports, or analytics

**Synthesizer (always included)**
- Wait for all reviewers to complete
- Identify cross-domain conflicts (e.g., accessibility vs design aesthetics)
- Prioritize findings by rejection risk and user impact
- Create unified action plan

## Step 3: Cross-Verification

Instruct reviewers to:
- Share findings that may affect other domains
- Privacy issues often overlap with App Store guidelines
- Accessibility issues often relate to HIG compliance
- Verify each other's severity assessments

## Step 4: Final Report

Synthesize into this format:

```
### Compliance Check Report

**Scope**: {specific screen / full app / recent changes}
**Domains Checked**: {list}
**Reviewers**: {count}

#### Critical (likely App Store rejection or legal risk)
| # | Domain | Issue | Location | Fix |
|---|--------|-------|----------|-----|
| 1 | ... | ... | file:line | ... |

#### Major (guideline violation, poor user experience)
| # | Domain | Issue | Location | Fix |
|---|--------|-------|----------|-----|
| 1 | ... | ... | file:line | ... |

#### Minor (recommendations, best practice improvements)
| # | Domain | Issue | Location | Fix |
|---|--------|-------|----------|-----|
| 1 | ... | ... | file:line | ... |

#### Domain Summaries

**HIG**: {pass/fail count, key themes}
**Accessibility**: {pass/fail count, key themes}
**App Store**: {pass/fail count, key themes}
**Privacy**: {pass/fail count, key themes}

#### Cross-Domain Conflicts
- {conflicts between domains and recommended resolution}

#### Action Plan (prioritized)
1. [ ] {highest priority fix}
2. [ ] ...
3. [ ] ...
```
