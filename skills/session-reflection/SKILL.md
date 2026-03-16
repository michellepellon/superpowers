---
name: session-reflection
description: >
  Analyze session history to identify inefficiency patterns and workflow gaps.
  Two modes: lightweight end-of-session reflection from conversation context,
  or comprehensive periodic analysis across multiple sessions via jq-extracted
  JSONL summaries. Produces structured reports with copy-paste-ready improvement
  proposals for CLAUDE.md, skills, commands, and automation.
compatibility: Requires jq installed for periodic mode. Session mode has no external dependencies.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  adapted-from: harperreed/dotfiles/.claude/skills/session-reflection-analysis
allowed-tools: Read Write Grep Glob Bash Agent
---

# Session Reflection

## Overview

Analyze session history to find inefficiency patterns and workflow gaps, then propose concrete improvements. Two modes: quick end-of-session reflection or comprehensive periodic multi-session analysis.

**Core principle:** Evidence-based improvement. Quantify waste, propose specific fixes, implement nothing without approval.

## When This Skill Activates

- End of a session (user asks to reflect)
- Periodic review (weekly/monthly)
- After a particularly frustrating or inefficient session
- When onboarding to a new project (analyze early sessions for missing docs)

**Trigger phrases:** "reflect on this session", "session review", "what went wrong", "workflow audit", "analyze my sessions", "how can we improve"

## Mode Selection

| Situation | Mode | Why |
|---|---|---|
| "Reflect on this session" | Session | Single session, context available |
| "What went wrong today" | Session | Current work, immediate feedback |
| "Review my sessions this week" | Periodic | Cross-session patterns need JSONL data |
| "How can I improve my workflow" | Periodic | Patterns emerge across sessions |
| "Analyze the last 3 days" | Periodic | Multi-session, specific time range |

| | Session mode | Periodic mode |
|---|---|---|
| **Scope** | Current session context only | JSONL files from last N days (default: 7, max: 14) |
| **Data source** | Agent's own conversation context | `jq`-extracted summaries from `~/.claude/projects/` |
| **Depth** | Quick patterns, 5-10 min | Comprehensive cross-session analysis, 15-30 min |
| **Dependencies** | None | `jq` must be installed |

## Process

1. **Determine mode** — Session or periodic based on user request
2. **Gather data** — From context (session) or JSONL extraction (periodic — see [references/extraction-patterns.md](references/extraction-patterns.md))
3. **Detect patterns** — Walk through all analysis categories below
4. **Score findings** — High/Medium/Low impact based on frequency and token/time cost
5. **Generate report** — `docs/audits/SESSION_REFLECTION_YYYY-MM-DD.md`
6. **Present findings** — Summarize 3-5 key findings to the user
7. **User review gate** — Implement only changes the user approves

### Session Mode: Reflecting From Context

The agent reviews its own conversation history within the current session:

- **Tool call inventory** — Which tools were called, how many times, on what targets
- **Decision trace** — Where did the agent change direction, backtrack, or get corrected
- **User corrections** — Any place the user said "no, not that" or redirected the approach
- **Time spent** — Which steps took disproportionately long

No file I/O needed. The agent introspects on its own behavior.

### Periodic Mode: JSONL Extraction

**CRITICAL: Never read raw JSONL files directly.** They are massive and will consume the entire context budget. Always use the `jq` summary extraction pipeline documented in [references/extraction-patterns.md](references/extraction-patterns.md).

Use a subagent (Agent tool) to perform the extraction and analysis, keeping the main context clean.

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

| Pattern | Signal | Impact | Example |
|---------|--------|--------|---------|
| Skill ignored | Agent rationalized skipping an applicable skill | High — skill needs stronger triggers | Fresh-eyes applicable but agent went straight to commit |
| Skill partially followed | Agent started skill process but shortcut steps | Medium — skill may be too heavy | Started TDD but skipped red-green-refactor cycle |
| Skill fought | Agent followed skill but produced poor results | Medium — skill content needs revision | Doc audit ran but missed obvious false claims |

## Report Format

Generate `docs/audits/SESSION_REFLECTION_YYYY-MM-DD.md`:

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

## Proposed Changes

### CLAUDE.md Updates
| # | Change | Rationale | Priority |
|---|--------|-----------|----------|
| 1 | Add section on X | Discovered 3 times | High |

#### Change 1: [Title]
**Add to:** CLAUDE.md > [section]
[exact text to add — copy-paste ready]

### New Skills
[complete skill content if applicable]

### New Slash Commands
[complete command content if applicable]

### Automation (scripts, hooks)
[complete script/hook content if applicable]
```

**Key principle:** All proposals are copy-paste ready with complete text/code. No vague "consider adding documentation about X."

## Anti-Patterns

| Anti-Pattern | Why It's Wrong |
|---|---|
| Reading raw JSONL files | Will consume entire context budget — always use jq extraction |
| Auto-implementing changes | Proposals need human review — reflection without consent is noise |
| Shallow pattern matching | "Read file 3 times" might be intentional (file changed) — check context |
| Reflecting mid-task | Reflection is a distinct activity, not a sidebar — finish work first |
| Boiling the ocean | Periodic mode across 30 days will still be too much — default 7, max 14 |

## Resistance Patterns

| Rationalization | Reality |
|---|---|
| "This session went fine" | Fine sessions still have patterns worth catching |
| "I already know what went wrong" | Intuition misses frequency — you remember the big blocker, not the 5 small wastes |
| "I'll just remember for next time" | You won't. Write it down or it's lost |
| "The improvements are too small to bother" | 500 tokens saved x 50 sessions = 25k tokens. Small compounds. |
| "I don't have time to reflect" | 10 minutes of reflection prevents 60 minutes of repeated mistakes |

## Detailed References

- [references/extraction-patterns.md](references/extraction-patterns.md) — JSONL schema, jq extraction commands, session file discovery for periodic mode
