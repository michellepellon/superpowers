---
name: documentation-audit
description: >
  Systematically verify documentation claims against codebase reality. Use when auditing docs,
  before releases, after refactors, or when documentation drift is suspected. Two-pass approach
  with pattern expansion ensures comprehensive detection of false claims, dead references, and gaps.
compatibility: Requires Plan agent support and parallel Task execution. Best for projects with user-facing markdown documentation.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  upstream: "https://github.com/2389-research/claude-plugins/tree/main/documentation-audit"
allowed-tools: Read Write Grep Glob Bash Task EnterPlanMode ExitPlanMode
---

# Documentation Audit Skill

Systematically verify claims in documentation against the actual codebase using a two-pass approach.

## When This Skill Activates

- Auditing documentation for accuracy
- Before release cycles (verify docs match current behavior)
- After major refactors (code changed, docs may be stale)
- When users report docs don't match behavior
- Periodic hygiene (quarterly documentation review)

**Trigger phrases**: "audit docs", "verify documentation", "check docs", "docs accurate", "documentation drift"

## Core Principle

**Low recall is worse than false positives** — missed claims stay invisible and mislead users.

**Two-pass process:**
1. **Pass 1:** Extract and verify claims directly from docs
2. **Pass 2A:** Expand patterns from false claims to find similar issues
3. **Pass 2B:** Compare codebase inventory vs documented items (gap detection)

## Quick Start

1. Identify target docs (user-facing only, skip `plans/`, `audits/`)
2. Note current git commit for report header
3. Run Pass 1 extraction using parallel agents (one per doc)
4. Analyze false claims for patterns
5. Run Pass 2 expansion searches
6. Generate `docs/audits/AUDIT_REPORT_YYYY-MM-DD.md`

## Claim Types

| Type | Example | Verification |
|------|---------|--------------|
| `file_ref` | `scripts/foo.py` | File exists? |
| `config_default` | "defaults to 'AI Radio'" | Check schema/code |
| `env_var` | `STATION_NAME` | In .env.example + code? |
| `cli_command` | `--normalize` flag | Script supports it? |
| `behavior` | "runs every 2 minutes" | Check timers/code |

### Verification Confidence Tiers

- **Tier 1 (auto):** file_ref, config_default, env_var, cli_command
- **Tier 2 (semi-auto):** symbol_ref, version_req
- **Tier 3 (human review):** behavior, constraint

## Pass 2 Pattern Expansion

After Pass 1, analyze false claims and search for similar patterns:

```
Dead script found: diagnose_track_selection.py
  → Search: all script references → Found 8 more dead scripts

Wrong interval: "every 10 seconds"
  → Search: "every \d+ (seconds?|minutes?)" → Found 3 more

Wrong service name: ai-radio-break-gen.service
  → Search: service/timer names → Found naming inconsistencies
```

### Common Patterns to Check

Always verify these patterns across all docs:
- Dead scripts: `scripts/*.py` references
- Timer intervals: `every \d+ (seconds?|minutes?)`
- Service names: `ai-radio-*.service`, `*.timer`
- Config vars: `RADIO_*` environment variables
- CLI flags: `--flag` patterns in bash blocks

## Output Format

Generate `docs/audits/AUDIT_REPORT_YYYY-MM-DD.md`:

```markdown
# Documentation Audit Report
Generated: YYYY-MM-DD | Commit: abc123

## Executive Summary
| Metric | Count |
|--------|-------|
| Documents scanned | 12 |
| Claims verified | ~180 |
| Verified TRUE | ~145 (81%) |
| **Verified FALSE** | **31 (17%)** |

## False Claims Requiring Fixes
### CONFIGURATION.md
| Line | Claim | Reality | Fix |
|------|-------|---------|-----|
| 135 | `claude-sonnet-4-5` | Actual: `claude-3-5-sonnet-latest` | Update |

## Pattern Summary
| Pattern | Count | Root Cause |
|---------|-------|------------|
| Dead scripts | 9 | Scripts deleted, docs not updated |

## Human Review Queue
- [ ] Line 436: behavior claim needs verification
```

## Execution Approach

This skill runs in **Plan mode** with a **forked context** to keep extraction artifacts separate from your main conversation:

1. **Parallel extraction** - Use one Task agent per document for efficiency
2. **Evidence-based verdicts** - Record file:line for every claim verification
3. **Pattern matching** - After Pass 1, systematically search for similar issues
4. **Gap detection** - List actual artifacts vs documented ones

## Anti-Patterns to Avoid

1. **Skipping Pass 2** — Pattern expansion catches 10-20% more issues
2. **Trusting "looks correct"** — Always verify with evidence
3. **Fixing without evidence** — Always cite file:line or "not found"
4. **Auditing design docs** — Focus on user-facing docs, skip historical artifacts
5. **Batching verification early** — Complete extraction first

## Decision Framework

### What to Audit

| Include | Skip |
|---------|------|
| User-facing README.md | `docs/plans/` |
| `docs/` directory | `docs/audits/` |
| Getting started guides | Design documents |
| Configuration docs | Historical artifacts |
| API documentation | Internal brainstorms |

### When to Run

| Trigger | Frequency |
|---------|-----------|
| Before release | Every release |
| After refactor | As needed |
| User reports mismatch | Immediate |
| Periodic hygiene | Quarterly |

## Real-World Results

From a representative audit:
- 12 documents scanned
- ~180 claims verified
- 31 false claims found (17% error rate)
- Common patterns: dead scripts (9), wrong intervals (4), wrong service names (3)

## Detailed References

For comprehensive coverage of specific topics, see:

- [references/checklist.md](references/checklist.md) — Step-by-step execution checklist with TodoWrite integration
- [references/extraction-patterns.md](references/extraction-patterns.md) — Detailed patterns for claim recognition and verification
