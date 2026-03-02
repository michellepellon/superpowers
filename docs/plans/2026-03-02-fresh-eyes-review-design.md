# Fresh-Eyes Review Skill Design

## Summary

A standalone discipline-enforcing skill that performs a systematic re-read of changed files against 5 checklists before code ships. Adapted from `2389-research/claude-plugins/fresh-eyes-review` (MIT license).

## Decisions

- **Language:** Agnostic — general checklists, no code examples
- **Trigger ordering:** Sequential gate after `verification-before-completion`, before `requesting-code-review`
- **Density:** Lean checklists only (~300 words) — no code examples, no time commitment table
- **Location:** New `quality-gates` plugin group, separate from `software-development-skills`
- **Attribution:** `adapted-from` field in SKILL.md frontmatter metadata

## Workflow Position

```
verification-before-completion  →  fresh-eyes-review  →  requesting-code-review
         (prove it runs)            (re-read for bugs)     (architectural feedback)
```

## File Structure

```
skills/
  fresh-eyes-review/
    SKILL.md          # ~300 words, self-contained
```

## SKILL.md Structure

1. **Overview** — Core principle. Cross-reference `verification-before-completion` as prerequisite.
2. **Process** — 4 steps: announce, walk checklists, fix immediately, declare results.
3. **Checklists** — 5 compact tables:
   - Security (SQL injection, XSS, path traversal, command injection, auth bypass)
   - Logic (off-by-one, race conditions, null handling, error swallowing)
   - Business rules (calculations, conditions, edge cases, defaults)
   - Input validation (type checks, range checks, format checks)
   - Performance (N+1 queries, unbounded loops, memory leaks, missing pagination)
4. **Resistance patterns** — Rationalization table (6 excuses + rebuttals)
5. **Red flags** — Self-check list for when about to skip the review

## Marketplace Update

Add new plugin group to `marketplace.json`:

```json
{
  "name": "quality-gates",
  "description": "Pre-commit quality gates that catch issues before code ships.",
  "source": "./",
  "strict": false,
  "skills": ["./skills/fresh-eyes-review"]
}
```
