# Session Reflection Skill Design

## Overview

Analyze session history to identify inefficiency patterns and workflow gaps, then propose concrete improvements to CLAUDE.md, skills, and automation. Two modes: lightweight end-of-session reflection or comprehensive periodic multi-session analysis.

**Adapted from:** [harperreed/dotfiles/.claude/skills/session-reflection-analysis](https://github.com/harperreed/dotfiles/tree/master/.claude/skills/session-reflection-analysis)

**Key adaptations:** Restructured from a bash-heavy procedural script into a decision-framework skill matching superpowers conventions. Added session mode (reflect from context, no file I/O), analysis category tables, resistance patterns, and references architecture.

## Identity

- **Name:** `session-reflection`
- **Plugin group:** `quality-gates` (description updated to: "Quality gates and retrospective analysis for catching issues and improving workflows.")
- **Trigger phrases:** "reflect on this session", "session review", "what went wrong", "workflow audit", "analyze my sessions", "how can we improve"

### When It Activates

- End of a session (user asks to reflect)
- Periodic review (weekly/monthly)
- After a particularly frustrating or inefficient session
- When onboarding to a new project (analyze early sessions for missing docs)

## Core Process

### Two Modes

| | Session mode | Periodic mode |
|---|---|---|
| **Scope** | Current session context only | JSONL files from last N days (default: 7) |
| **Data source** | Agent's own conversation context | `jq`-extracted summaries from `~/.claude/projects/` |
| **Depth** | Quick patterns, 5-10 min | Comprehensive cross-session analysis, 15-30 min |
| **Typical trigger** | "Reflect on this session" | "Review my sessions from this week" |

### Shared Workflow

1. **Gather data** — From context (session mode) or JSONL extraction (periodic mode)
2. **Detect patterns** — Walk through analysis categories
3. **Score findings** — High/Medium/Low impact based on frequency and token/time cost
4. **Generate report** — `docs/audits/SESSION_REFLECTION_YYYY-MM-DD.md`
5. **Propose changes** — Concrete, copy-paste-ready improvements grouped by type
6. **User review gate** — Present findings, implement only what's approved

### Critical Rules

- **Never read raw JSONL directly** in periodic mode. Always use `jq` summary extraction first. Raw files are massive and will blow context.
- **In session mode, reflect from own context** — no file I/O needed. This keeps it lightweight.
- **Never auto-implement changes** — all proposals require user approval.

## Analysis Categories

### Agent Behavior Patterns

| Pattern | Signal | Impact | Example |
|---------|--------|--------|---------|
| Repeated file reads | Same file read 3+ times | High — token waste | Read `config.py` 7 times across session |
| Wrong path taken | Implementation then reversal | High — time waste | Built feature, discovered existing code |
| Unnecessary tool calls | Redundant or no-op calls | Medium — token waste | Glob + Grep for something already in context |
| Context loss recovery | Re-discovering info after compaction | High — fragile workflows | Key architecture detail lost mid-session |
| Assumption without verification | Decision made, then corrected | High — rework | Assumed API shape, had to refactor |

### Workflow Gaps

| Pattern | Signal | Impact | Example |
|---------|--------|--------|---------|
| Repeated manual steps | Same commands across sessions | Medium — automation opportunity | Running same 3-command setup sequence |
| Missing documentation | Agent had to discover what should be documented | Medium — onboarding friction | Figured out test setup from scratch |
| Missing skill coverage | Agent did something a skill should guide | Medium — quality risk | Ad-hoc code review without fresh-eyes |
| Recurring blockers | Same type of error/obstacle reappearing | High — systemic issue | Permission errors, env setup failures |

### Lightweight Skill Effectiveness

| Pattern | Signal | Impact |
|---------|--------|--------|
| Skill ignored | Agent rationalized skipping an applicable skill | High — skill needs stronger triggers |
| Skill partially followed | Agent started skill process but shortcut steps | Medium — skill may be too heavy |
| Skill fought | Agent followed skill but produced poor results | Medium — skill content may need revision |

## Output Format

### Report: `docs/audits/SESSION_REFLECTION_YYYY-MM-DD.md`

```markdown
# Session Reflection: YYYY-MM-DD
Mode: session | periodic (N days, M sessions)
Generated: YYYY-MM-DD | Commit: abc123

## Summary
| Metric | Count |
|--------|-------|
| Sessions analyzed | 1 or N |
| Patterns detected | X |
| High impact | X |
| Medium impact | X |

## Findings

### High Impact

#### 1. [Pattern Title]
**Category:** Agent Behavior | Workflow Gap | Skill Effectiveness
**Frequency:** N occurrences across M sessions
**Evidence:** [Specific examples from session data]
**Estimated cost:** ~Nk tokens wasted | ~N minutes lost per session

---

### Medium Impact
...
```

### Proposed Changes (in the same report)

Grouped by type, all copy-paste ready:

```markdown
## Proposed Changes

### CLAUDE.md Updates
| # | Change | Rationale | Priority |
|---|--------|-----------|----------|
| 1 | Add section on X | Discovered 3 times | High |

#### Change 1: [Title]
**Add to:** CLAUDE.md > [section]
[exact text to add]

### New Skills
...

### New Slash Commands
...

### Automation (scripts, hooks)
...
```

**Key principle:** All proposals are copy-paste ready with complete text/code. No vague "consider adding documentation about X."

## Anti-Patterns

| Anti-Pattern | Why It's Wrong |
|---|---|
| Reading raw JSONL files | Will consume entire context budget — always use jq extraction |
| Auto-implementing changes | Proposals need human review — reflection without consent is noise |
| Shallow pattern matching | "Read file 3 times" might be intentional (file changed) — check context |
| Reflecting mid-task | Reflection is a distinct activity, not a sidebar — finish work first |
| Boiling the ocean | Periodic mode across 30 days of heavy sessions will still be too much — cap at 7 days default |

## Resistance Patterns

| Rationalization | Reality |
|---|---|
| "This session went fine" | Fine sessions still have patterns worth catching |
| "I already know what went wrong" | Intuition misses frequency — you remember the big blocker, not the 5 small wastes |
| "I'll just remember for next time" | You won't. Write it down or it's lost |
| "The improvements are too small to bother" | 500 tokens saved x 50 sessions = 25k tokens. Small compounds. |
| "I don't have time to reflect" | 10 minutes of reflection prevents 60 minutes of repeated mistakes |

## File Structure

```
skills/
  session-reflection/
    SKILL.md                              # Core skill — process, categories, decision tables, anti-patterns
    references/
      extraction-patterns.md              # jq commands, JSONL parsing, data preparation for periodic mode
      report-template.md                  # Full report template with all sections
```

### marketplace.json Changes

- Add `"./skills/session-reflection"` to `quality-gates` plugin skills array
- Update quality-gates description to: "Quality gates and retrospective analysis for catching issues and improving workflows."
