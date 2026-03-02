---
name: fresh-eyes-review
description: >
  Use when about to commit, create a PR, or declare work complete — after
  verification-before-completion confirms tests pass but before code ships.
  Catches security vulnerabilities, logic errors, and business rule bugs
  that slip through despite passing tests.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  adapted-from: 2389-research/claude-plugins/fresh-eyes-review
---

# Fresh-Eyes Review

## Overview

Deliberate re-reading of changed code with psychological distance. Catches what you assumed was correct.

**Core principle:** 100% test coverage can coexist with critical bugs.

**Prerequisite:** `verification-before-completion` must pass first.

## Process

1. **Announce:** "Starting fresh-eyes review of [N] files."
2. **Walk each changed file** through the five checklists below.
3. **Fix immediately.** Re-run tests after each fix.
4. **Declare:** "Fresh-eyes complete. [N] issues found and fixed: [brief description of each]." Include this even for zero findings.

## Checklists

### Security

| Check | Look For |
|-------|----------|
| Injection | Unsanitized input in queries, commands, or templates |
| Path traversal | Unvalidated file paths, `../` sequences |
| Auth gaps | Unprotected endpoints, missing authorization checks |
| Secrets | Hardcoded credentials, tokens, or keys |

### Logic

| Check | Look For |
|-------|----------|
| Boundaries | Off-by-one in indices, loops, pagination |
| Race conditions | Concurrent access to shared state |
| Null handling | Unguarded access chains that could throw |
| Error swallowing | Empty catch blocks, ignored rejections |

### Business Rules

| Check | Look For |
|-------|----------|
| Calculations | Formulas matching requirements, correct rounding |
| Conditions | AND/OR logic correct, negations applied properly |
| Edge cases | Empty input, single item, zero, maximum values |
| Defaults | Sensible values when optional fields omitted |

### Input Validation

| Check | Look For |
|-------|----------|
| Type checks | Expected types enforced at boundaries |
| Range checks | Numeric bounds, string lengths, array sizes |
| Format checks | Email, URL, date formats validated |

### Performance

| Check | Look For |
|-------|----------|
| N+1 queries | Loops making individual database/API calls |
| Unbounded work | Missing limits on iterations, result sets, payloads |
| Resource leaks | Unclosed connections, streams, event listeners |

## Resistance Patterns

| Rationalization | Reality |
|-----------------|---------|
| "Tests are comprehensive" | Tests validate design, not correctness |
| "I'm confident it's correct" | Confidence is inversely correlated with bugs |
| "It's just a small change" | Small changes cause large outages |
| "Partner is waiting" | 3 minutes now saves 3 hours debugging later |
| "Senior dev already approved" | They reviewed intent, not implementation details |
| "Production is blocked" | Rushing causes the outages being rushed to fix |

## Red Flags — STOP

- "I already looked at this code while writing it"
- "The tests cover everything"
- "This is too trivial to review"
- "I just need to commit this quickly"
