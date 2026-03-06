# Context Engineering Skill Design

**Date**: 2026-03-05
**Status**: Approved

## Summary

A single operational skill distilling actionable content from [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) into the superpowers pattern: situation-driven diagnostics, decision frameworks, anti-patterns, and resistance patterns.

## Decisions

- **Plugin group**: New `agent-engineering` group in marketplace.json
- **Skill name**: `context-engineering`
- **Organization**: Situation-driven (Approach A) — entry points are "what am I facing?" not "what concept do I need?"
- **Tool design content**: Folded into this skill (not a separate skill)
- **Attribution**: `upstream` field in YAML frontmatter
- **Reference files**: 3 deep-dives (degradation-patterns, compression-strategies, tool-design-principles)
- **Allowed tools**: Read, Glob, Grep (guidance skill, not execution)

## File Structure

```
skills/context-engineering/
├── SKILL.md
└── references/
    ├── degradation-patterns.md
    ├── compression-strategies.md
    └── tool-design-principles.md
```

## SKILL.md Structure

1. **Overview** — Context is a finite resource; this skill provides diagnostics and frameworks
2. **When This Skill Activates** — Degraded behavior, tool/skill design, multi-agent decisions, context limits, LLM project starts
3. **Core Principles** — Tokens-per-task, structure forces preservation, finite with diminishing returns, isolate before growing, tools should enable not constrain
4. **Diagnostic: Is My Context Degrading?** — Symptom → Pattern → Mitigation table (5 degradation types)
5. **Diagnostic: Do I Need Multi-Agent?** — Criteria checklist + token economics table
6. **Framework: Context Recovery (Four Buckets)** — Write / Select / Compress / Isolate as sequenced procedure
7. **Framework: Compression Strategy Selection** — Strategy comparison table + trigger table + structured summary template
8. **Framework: Tool & Skill Design Quality** — Consolidation litmus test + description checklist + architectural reduction question
9. **Anti-Patterns** — 8-10 items with name, symptom, failure mode, fix
10. **Resistance Patterns** — 4-5 rationalizations with rebuttals
11. **References** — Links to 3 deep-dive files

## Reference Files

### degradation-patterns.md
- Lost-in-Middle (U-shaped attention, 10-40% recall drop)
- Context Poisoning (3 entry pathways, feedback loops, recovery)
- Context Distraction (step-function degradation from single distractors)
- Context Confusion (wrong-task symptoms, task segmentation)
- Context Clash (contradictory valid sources, resolution approaches)
- Model-Specific Thresholds (Claude Opus 4.5, Sonnet 4.5, GPT-5.2, Gemini 3)

### compression-strategies.md
- Three strategies compared (anchored iterative, regenerative, opaque) with ratios and quality scores
- Anchored iterative summarization step-by-step
- The artifact trail problem (2.2-2.5/5.0 universally)
- Compression trigger strategies table
- Probe-based evaluation (4 probe types)
- Three-phase workflow for large codebases

### tool-design-principles.md
- Consolidation principle with examples
- Architectural reduction (Vercel d0 case study, when reduction wins vs when complexity is necessary)
- Description engineering (4-question framework, weak vs strong examples)
- Response format optimization
- Build for future models principle
- Collection design (10-20 tool guideline, namespacing, MCP naming)

## Scope Boundaries

### Including (distilled from source repo)
- Degradation patterns + empirical thresholds
- Compression strategies + artifact trail + probes
- Four-bucket approach
- Consolidation principle + architectural reduction + description engineering
- Multi-agent token economics + context isolation + telephone game
- Task-model fit awareness (brief, in anti-patterns)

### NOT including
- No Python scripts or utility code
- No memory systems framework comparison
- No evaluation/LLM-as-judge content
- No BDI/ontology content
- No hosted agent infrastructure
- No filesystem-context patterns (Claude Code already implements natively)
- No KV-cache optimization details

### Original content (not from source repo)
- Resistance patterns (fresh-eyes-review style)
- Situation-driven diagnostic format
- Four-bucket ordering as sequenced decision procedure
