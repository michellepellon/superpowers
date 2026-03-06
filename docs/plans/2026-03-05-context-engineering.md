# Context Engineering Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a `context-engineering` skill in a new `agent-engineering` plugin group, distilling operational content from muratcankoylan/Agent-Skills-for-Context-Engineering into the superpowers pattern.

**Architecture:** Single skill directory with SKILL.md (situation-driven diagnostics, decision frameworks, anti-patterns, resistance patterns) and 3 reference files (degradation-patterns, compression-strategies, tool-design-principles). New plugin group registered in marketplace.json. README updated.

**Tech Stack:** Markdown skill files, JSON plugin config.

**Source material:** This conversation contains the full text of all source skills fetched from the upstream repo. The design doc is at `docs/plans/2026-03-05-context-engineering-design.md`.

---

### Task 1: Create SKILL.md

**Files:**
- Create: `skills/context-engineering/SKILL.md`

**Step 1: Create the skill directory**

Run: `mkdir -p skills/context-engineering/references`

**Step 2: Write SKILL.md**

Write the file at `skills/context-engineering/SKILL.md` with the following structure. Follow the exact frontmatter pattern from `skills/fresh-eyes-review/SKILL.md` and `skills/python/SKILL.md`.

**Frontmatter:**
```yaml
---
name: context-engineering
description: >
  Use when managing long agent sessions, diagnosing degraded agent behavior,
  designing tools or skills, choosing compression strategies, or deciding
  whether to use multi-agent architectures. Provides operational decision
  frameworks for context window management, tool design, and agent coordination.
metadata:
  author: Michelle Pellon
  version: "1.0.0"
  upstream: muratcankoylan/Agent-Skills-for-Context-Engineering
allowed-tools: Read Glob Grep
---
```

**Body sections (write in this order):**

1. `# Context Engineering` — Title

2. `## Overview` — 2-3 sentences. Context is the complete state available to a language model at inference time — a finite resource with diminishing returns. The engineering discipline is curating the smallest high-signal token set that achieves desired outcomes. This skill provides situation-driven diagnostics and decision frameworks for context management, tool design, and agent coordination.

3. `## When This Skill Activates` — Bullet list:
   - Agent behavior degrades during long sessions (repetition, ignoring instructions, wrong tool calls)
   - Designing tools, skills, or MCP servers
   - Choosing whether to use sub-agents vs single agent
   - Context window approaching capacity limits
   - Starting a new LLM-powered project or pipeline
   - Debugging unexpected agent failures that may relate to context

4. `## Core Principles` — Numbered list of 5:
   1. **Tokens-per-task, not tokens-per-request** — Optimize total tokens consumed from task start to completion, including re-fetching costs when compression loses critical information
   2. **Structure forces preservation** — Dedicated sections in summaries act as checklists the summarizer must populate, preventing silent information drift
   3. **Context is finite with diminishing returns** — Every token depletes attention budget. Performance degrades non-linearly beyond model-specific thresholds
   4. **Isolate before growing** — Partition work across sub-agents with fresh contexts before stuffing more into a single window
   5. **Tools should enable, not constrain** — Ask whether each tool enables new capabilities or constrains reasoning the model could handle on its own

5. `## Diagnostic: Is My Context Degrading?` — Table with 3 columns: Symptom, Pattern, Mitigation.

   | Symptom | Pattern | Mitigation |
   |---------|---------|------------|
   | Agent ignores information you provided mid-conversation | **Lost-in-Middle** — U-shaped attention curve; 10-40% recall drop for content in middle positions | Move critical info to beginning or end of context. Use explicit section headers. Summarize key points at attention-favored positions |
   | Agent repeats earlier errors despite correction | **Context Poisoning** — Errors enter via tool outputs, stale docs, or hallucinated summaries, then compound through repeated reference | Truncate context to before the poisoning point, explicitly note the error, or restart with clean context preserving only verified information |
   | Agent produces lower quality after adding reference documents | **Context Distraction** — Even a single irrelevant document triggers step-function degradation; model must attend to everything provided | Remove irrelevant documents. Apply relevance filtering before loading. Use tool calls for on-demand access instead of pre-loading |
   | Agent mixes up requirements from different tasks | **Context Confusion** — Model cannot determine which context applies to current task, especially when switching tasks within a session | Segment different tasks into separate contexts. Use clear transitions. Isolate with sub-agents if tasks are independent |
   | Agent produces contradictory outputs across turns | **Context Clash** — Multiple valid but contradictory pieces of information accumulated in context | Mark conflicts explicitly. Establish priority rules (most recent wins). Filter outdated information before it enters context |

6. `## Diagnostic: Do I Need Multi-Agent?` — First, a checklist of criteria (use multi-agent when ANY of these are true):
   - Context has exceeded or is approaching degradation onset for your model
   - Task decomposes into parallelizable subtasks with independent contexts
   - Different subtasks require different tool sets or system prompts
   - Single agent is exhibiting degradation symptoms from the table above

   Then a key principle: **"Sub-agents exist primarily to isolate context, not to anthropomorphize role division."**

   Then the token economics table:

   | Architecture | Token Multiplier | Use When |
   |---|---|---|
   | Single agent | 1x baseline | Simple queries, single-turn tasks |
   | Single agent + tools | ~4x baseline | Tool-using tasks within context limits |
   | Multi-agent | ~15x baseline | Complex tasks exceeding single-context capacity |

   Then the telephone game warning: Supervisor architectures lose fidelity when paraphrasing sub-agent responses. Prefer direct pass-through of sub-agent results when the response is final and complete.

7. `## Framework: Context Recovery (The Four Buckets)` — When context is growing too large, apply these strategies in order:

   **1. Write** — Save context outside the window. Write large tool outputs, plans, and intermediate results to the filesystem. Return references instead of full content. *Try this first — it's the cheapest intervention.*

   **2. Select** — Pull only relevant context into the window. Apply relevance filtering before loading documents. Use targeted retrieval (grep, line-specific reads) instead of loading entire files. Remove information whose purpose has been served.

   **3. Compress** — Reduce tokens while preserving information. Summarize older conversation turns. Replace verbose tool outputs with compact summaries. Use structured summary templates to prevent silent information loss. See [Compression Strategies](./references/compression-strategies.md).

   **4. Isolate** — Split work across sub-agents with fresh context windows. Each agent operates in clean context focused on its subtask. Results aggregate at the coordinator without any single context bearing full burden. *Most aggressive strategy, but often most effective for complex tasks.*

8. `## Framework: Compression Strategy Selection` — Strategy comparison table:

   | Strategy | Compression | Quality | Use When |
   |---|---|---|---|
   | **Anchored Iterative** | 98.6% | 3.70/5.0 | Long sessions (100+ messages), file tracking matters, need to verify what was preserved |
   | **Regenerative Full** | 98.7% | 3.44/5.0 | Clear phase boundaries, summary interpretability critical, full context review acceptable |
   | **Opaque** | 99.3% | 3.35/5.0 | Maximum token savings needed, short sessions, re-fetching costs are low |

   Then compression trigger table:

   | Trigger | When It Fires | Trade-off |
   |---|---|---|
   | Fixed threshold | 70-80% context utilization | Simple but may compress too early |
   | Sliding window | Keep last N turns + summary | Predictable context size |
   | Task-boundary | At logical task completions | Clean summaries but unpredictable timing |

   Then the structured summary template (use for anchored iterative):
   ```
   ## Session Intent
   [What the user is trying to accomplish]

   ## Files Modified
   - file.ts: Description of change

   ## Decisions Made
   - Decision and rationale

   ## Current State
   - What's working, what's failing

   ## Next Steps
   1. Immediate next action
   ```

   Then: **The artifact trail is the weakest dimension** — file tracking scores 2.2-2.5/5.0 across all methods. If file tracking is critical, maintain a separate artifact index outside the summary.

9. `## Framework: Tool & Skill Design Quality` — Three checks:

   **The Consolidation Litmus Test:** "If a human engineer cannot definitively say which tool should be used in a given situation, an agent cannot be expected to do better." If two tools overlap in purpose, consolidate them.

   **The Description Engineering Checklist** — Every tool/skill description must answer:
   1. What does it do? (specific, not "helps with" or "can be used for")
   2. When should it be used? (concrete triggers, not vague scenarios)
   3. What inputs does it accept? (types, constraints, defaults)
   4. What does it return? (format, structure, error conditions)

   **The Architectural Reduction Question:** "Are your tools enabling new capabilities, or constraining reasoning the model could handle on its own?" Production evidence: Vercel's d0 agent went from 17 specialized tools to 2 primitives (bash + SQL) and improved from 80% to 100% success rate.

10. `## Anti-Patterns` — Table with 4 columns: Pattern, What It Looks Like, Why It Fails, Fix.

    8 anti-patterns:
    1. **Context stuffing** — Pre-loading all possible context "just in case" → Dilutes attention, triggers distraction → Load on-demand via tool calls
    2. **Optimizing tokens-per-request** — Aggressively compressing to minimize per-request tokens → Loses critical details, forces re-fetching → Optimize tokens-per-task (total cost including recovery)
    3. **Single-pass compression** — Regenerating full summary on each compression → Details lost across repeated cycles → Use anchored iterative: summarize only new content, merge into existing sections
    4. **Vague tool descriptions** — "Search the database for information" → Agent guesses at parameters, calls wrong tools → Answer the four description questions (what, when, inputs, returns)
    5. **Premature multi-agent** — Splitting into sub-agents before context is actually degrading → 15x token multiplier for no benefit → Try Write/Select/Compress first. Isolate only when single context is genuinely failing
    6. **Tools that constrain reasoning** — Building wrappers to "protect" the model from complexity → Becomes a liability as models improve, locks in current limitations → Prefer primitive general-purpose tools. Build minimal architectures that benefit from model improvements
    7. **Ignoring single-distractor impact** — Assuming one irrelevant document is harmless → Step-function degradation; any distractor triggers attention reallocation → Curate aggressively. Remove documents whose purpose has been served
    8. **Anthropomorphizing sub-agents** — Creating "Researcher", "Analyzer", "Writer" roles → Organizational metaphor drives architecture instead of context needs → Design for context isolation. Choose architecture based on what needs separate context, not org chart metaphors

11. `## Resistance Patterns` — Table with 2 columns: Rationalization, Reality.

    | Rationalization | Reality |
    |---|---|
    | "The context window is 200K tokens, I have plenty of room" | Meaningful degradation begins at 80-100K for most models. The window size is a ceiling, not an operating point |
    | "I just need to add one more tool" | Each tool adds description tokens competing for attention. 10-20 tools is the guideline; beyond that, consolidate or namespace |
    | "Let me include this document just in case" | A single irrelevant document triggers step-function degradation. If it's not needed for the current decision, access it on-demand instead |
    | "This session is fine, I don't need to compress yet" | By the time you notice degradation, quality has already declined. Trigger compression at 70-80% utilization, not at failure |
    | "More tools means more capable" | Vercel improved from 80% to 100% success by reducing from 17 tools to 2. Capability comes from model reasoning, not tool count |

12. `## References` — Links:
    - [Degradation Patterns](./references/degradation-patterns.md) — The five degradation types, symptoms, recovery, and model-specific thresholds
    - [Compression Strategies](./references/compression-strategies.md) — Anchored iterative summarization, artifact trail problem, probe-based evaluation
    - [Tool Design Principles](./references/tool-design-principles.md) — Consolidation, architectural reduction, description engineering, collection design

**Step 3: Verify SKILL.md structure**

Manually check:
- Frontmatter YAML is valid (starts and ends with `---`)
- All 3 reference links will resolve to files created in later tasks
- No code examples in the SKILL.md (those belong in references)
- Total length is under 500 lines (the source repo's own guideline)

**Step 4: Commit**

```bash
git add skills/context-engineering/SKILL.md
git commit -m "Add context-engineering skill"
```

---

### Task 2: Create references/degradation-patterns.md

**Files:**
- Create: `skills/context-engineering/references/degradation-patterns.md`

**Step 1: Write degradation-patterns.md**

Write file at `skills/context-engineering/references/degradation-patterns.md`.

Content sections:

1. `# Context Degradation Patterns` — Title + intro sentence: Language models exhibit predictable degradation as context grows. These patterns are diagnosable and mitigable.

2. `## Lost-in-Middle` —
   - What it is: U-shaped attention curve where middle content receives 10-40% lower recall
   - Why it happens: Attention sink on first token, training data distribution favors shorter sequences, attention budget stretched thin
   - How to detect: Agent misses information you placed mid-conversation, answers questions with information from beginning/end but not middle
   - Mitigation: Place critical information at beginning or end. Use explicit section headers. For long documents, surface key findings at attention-favored positions. Use summary structures that recapitulate key points.

3. `## Context Poisoning` —
   - What it is: Errors enter context and compound through repeated reference, creating feedback loops
   - Three entry pathways: (1) Tool outputs with errors/unexpected formats accepted as ground truth (2) Retrieved documents with incorrect/outdated information (3) Model-generated summaries introducing hallucinations that persist
   - Symptoms: Degraded output quality on previously-successful tasks, tool misalignment (wrong tools or parameters), persistent hallucinations despite correction
   - Recovery procedure: (1) Truncate context to before poisoning point (2) Explicitly note the error in context and request re-evaluation (3) Restart with clean context, preserving only verified information

4. `## Context Distraction` —
   - What it is: Irrelevant information overwhelms relevant content, degrading quality even when relevant info is present
   - Key finding: Even a single irrelevant document reduces performance via step function, not proportionally. Multiple distractors compound degradation.
   - Why it happens: Models have no mechanism to "skip" irrelevant context. They attend to everything provided regardless of relevance.
   - Mitigation: Curate aggressively before loading. Apply relevance filtering. Use on-demand tool calls instead of pre-loading. Remove documents whose purpose has been served.

5. `## Context Confusion` —
   - What it is: Model cannot determine which context applies to current situation, mixing requirements from different tasks
   - Signs: Responses address wrong aspect of query, tool calls appropriate for different task, outputs mix requirements from multiple sources
   - When it happens: Multiple task types in single session, switching between tasks, accumulated instructions from different contexts
   - Mitigation: Explicit task segmentation. Clear transitions between task contexts. State management that isolates context per objective. Sub-agent isolation for truly independent tasks.

6. `## Context Clash` —
   - What it is: Accumulated information directly contradicts, creating conflicting guidance. Distinct from poisoning (where one piece is wrong) — in clash, multiple pieces are individually correct but incompatible.
   - Sources: Multi-source retrieval with contradictory information, version conflicts (outdated + current both present), perspective conflicts (valid but incompatible viewpoints)
   - Resolution: Explicit conflict marking with request for clarification, priority rules (most recent wins, primary source wins), version filtering to exclude outdated information

7. `## Model-Specific Degradation Thresholds` — Table:

   | Model | Degradation Onset | Severe Degradation | Notes |
   |---|---|---|---|
   | Claude Opus 4.5 | ~100K tokens | ~180K tokens | 200K window. Strong attention management. Tends to refuse or ask clarification rather than fabricate |
   | Claude Sonnet 4.5 | ~80K tokens | ~150K tokens | Optimized for agents and coding tasks |
   | GPT-5.2 | ~64K tokens | ~200K tokens | Best degradation resistance in thinking mode |
   | Gemini 3 Pro | ~500K tokens | ~800K tokens | 1M window. Native multimodality |
   | Gemini 3 Flash | ~300K tokens | ~600K tokens | 3x speed of previous generation |

   Caveat: These thresholds are approximate and task-dependent. Complex reasoning tasks degrade earlier than simple retrieval. Near-perfect scores on needle-in-haystack tests do not translate to real long-context understanding.

8. `## Counterintuitive Findings` —
   - Shuffled (incoherent) haystacks outperform coherent ones — coherent context may create false associations
   - Lower similarity between needle and question shows faster degradation — tasks requiring inference across dissimilar content are most vulnerable
   - Performance remains stable up to a threshold, then degrades rapidly (non-linear). For many models, meaningful degradation begins at 8-16K even when windows support much larger sizes.

**Step 2: Commit**

```bash
git add skills/context-engineering/references/degradation-patterns.md
git commit -m "Add degradation patterns reference"
```

---

### Task 3: Create references/compression-strategies.md

**Files:**
- Create: `skills/context-engineering/references/compression-strategies.md`

**Step 1: Write compression-strategies.md**

Write file at `skills/context-engineering/references/compression-strategies.md`.

Content sections:

1. `# Compression Strategies` — Title + intro: When agent sessions generate millions of tokens of conversation history, compression becomes mandatory. The correct optimization target is tokens per task — total tokens consumed to complete a task, including re-fetching costs when compression loses critical information.

2. `## Why Tokens-Per-Task Matters` —
   - Traditional compression targets tokens-per-request (wrong optimization)
   - When compression loses file paths or error messages, agent must re-fetch, re-explore, waste tokens recovering context
   - A strategy saving 0.5% more tokens but causing 20% more re-fetching costs more overall
   - Right metric: tokens-per-task = total tokens from task start to completion

3. `## Three Production-Ready Approaches` —

   **Anchored Iterative Summarization:**
   - Maintain structured, persistent summaries with explicit sections
   - On compression trigger, summarize only newly-truncated span
   - Merge new summary into existing sections (don't regenerate from scratch)
   - Structure forces preservation — dedicated sections act as checklists
   - Scores: 98.6% compression, 3.70/5.0 quality

   **Regenerative Full Summary:**
   - Generate detailed structured summary on each compression
   - Produces readable output but may lose details across repeated cycles
   - Full regeneration rather than incremental merging
   - Scores: 98.7% compression, 3.44/5.0 quality

   **Opaque Compression:**
   - Compressed representations optimized for reconstruction fidelity
   - Highest compression ratio but sacrifices interpretability
   - Cannot verify what was preserved
   - Scores: 99.3% compression, 3.35/5.0 quality

   Summary: The 0.7% additional tokens retained by anchored iterative buys 0.35 quality points. For any task where re-fetching costs matter, this trade-off favors structured approaches.

4. `## The Structured Summary Template` —

   ```markdown
   ## Session Intent
   [What the user is trying to accomplish]

   ## Files Modified
   - auth.controller.ts: Fixed JWT token generation
   - config/redis.ts: Updated connection pooling
   - tests/auth.test.ts: Added mock setup for new config

   ## Decisions Made
   - Using Redis connection pool instead of per-request connections
   - Retry logic with exponential backoff for transient failures

   ## Current State
   - 14 tests passing, 2 failing
   - Remaining: mock setup for session service tests

   ## Next Steps
   1. Fix remaining test failures
   2. Run full test suite
   3. Update documentation
   ```

   Each section must be explicitly addressed during summarization. Empty sections indicate information that should have been preserved.

5. `## The Artifact Trail Problem` —
   - File tracking scores 2.2-2.5/5.0 across ALL compression methods — universally weak
   - Coding agents need: which files created, which modified and what changed, which read but not changed, function names, variable names, error messages
   - This likely requires specialized handling beyond general summarization
   - Mitigation: Maintain a separate artifact index or explicit file-state tracking outside the summary

6. `## Compression Triggers` —

   | Strategy | Trigger Point | Trade-off |
   |---|---|---|
   | Fixed threshold | 70-80% context utilization | Simple but may compress too early |
   | Sliding window | Keep last N turns + summary | Predictable context size |
   | Importance-based | Compress low-relevance sections first | Complex but preserves signal |
   | Task-boundary | Compress at logical task completions | Clean summaries but unpredictable timing |

   Sliding window with structured summaries provides the best balance for most coding agent use cases.

7. `## Probe-Based Evaluation` —

   Traditional metrics (ROUGE, embedding similarity) fail to capture functional compression quality. A summary may score high on lexical overlap while missing the one file path the agent needs. Test compression with probes:

   | Probe Type | What It Tests | Example Question |
   |---|---|---|
   | Recall | Factual retention | "What was the original error message?" |
   | Artifact | File tracking | "Which files have we modified?" |
   | Continuation | Task planning | "What should we do next?" |
   | Decision | Reasoning chain | "What did we decide about the Redis issue?" |

   If the agent answers correctly, compression preserved the right information. If not, it guesses or hallucinates.

8. `## Three-Phase Workflow for Large Codebases` —

   For codebases exceeding context windows (5M+ tokens):

   1. **Research Phase**: Explore architecture, documentation, key interfaces. Output: single research document compressing exploration into structured analysis of components and dependencies.
   2. **Planning Phase**: Convert research into implementation specification with function signatures, type definitions, data flow. A 5M token codebase compresses to ~2,000 words of spec.
   3. **Implementation Phase**: Execute against the spec. Context remains focused on the specification rather than raw codebase exploration.

   Use example artifacts (reference PRs, manual migrations) as seeds — they reveal constraints static analysis cannot surface.

**Step 2: Commit**

```bash
git add skills/context-engineering/references/compression-strategies.md
git commit -m "Add compression strategies reference"
```

---

### Task 4: Create references/tool-design-principles.md

**Files:**
- Create: `skills/context-engineering/references/tool-design-principles.md`

**Step 1: Write tool-design-principles.md**

Write file at `skills/context-engineering/references/tool-design-principles.md`.

Content sections:

1. `# Tool Design Principles` — Title + intro: Tools define the contract between deterministic systems and non-deterministic agents. Unlike traditional APIs designed for developers, tool APIs must be designed for language models that reason about intent, infer parameter values, and generate calls from natural language. Poor tool design creates failure modes that no amount of prompt engineering can fix.

2. `## The Consolidation Principle` —
   - Statement: "If a human engineer cannot definitively say which tool should be used in a given situation, an agent cannot be expected to do better."
   - Leads to preference for single comprehensive tools over multiple narrow tools
   - Instead of list_users, list_events, create_event → implement schedule_event that handles the full workflow
   - Why it works: Reduces token consumption (fewer descriptions), eliminates ambiguity (one tool per workflow), shrinks tool selection complexity
   - When NOT to consolidate: Fundamentally different behaviors, used in different contexts, called independently

3. `## Architectural Reduction` —
   - The consolidation principle taken to its logical extreme: removing most specialized tools in favor of primitive, general-purpose capabilities
   - **Case study: Vercel d0** — Text-to-SQL agent. Before: 17 specialized tools, 80% success, 274s average. After: 2 tools (bash + SQL), 100% success, 77s average.
   - Key insight: The semantic layer was already good documentation. The model just needed access to read files directly.
   - **When reduction outperforms complexity**: Data layer is well-documented, model has sufficient reasoning capability, specialized tools were constraining rather than enabling, more time maintaining scaffolding than improving outcomes
   - **When complexity is necessary**: Underlying data is messy/poorly documented, domain requires specialized knowledge model lacks, safety constraints require limiting capability, operations truly benefit from structured workflows
   - **The question to ask**: "Are your tools enabling new capabilities, or constraining reasoning the model could handle on its own?"

4. `## Description Engineering` —
   - Tool descriptions are prompt engineering — they collectively steer agent behavior
   - The four questions every description must answer:
     1. **What does it do?** Clear, specific. Avoid "helps with" or "can be used for." State exactly what it accomplishes.
     2. **When should it be used?** Specific triggers. Include direct triggers ("User asks about pricing") and indirect signals ("Need current market rates").
     3. **What inputs does it accept?** Types, constraints, defaults. Explain what each parameter controls.
     4. **What does it return?** Output format, structure, error conditions.

   - **Weak vs Strong example:**

     Weak: `"Search the database for customer information."`

     Strong: `"Retrieve customer information by ID or email. Use when user asks about specific customer details, history, or status. Returns customer object with basic info, order history summary, and support ticket count. Returns null if not found. Returns error if database unreachable."`

   - Default parameters should reflect common use cases, reduce agent burden, prevent errors from omission

5. `## Response Format Optimization` —
   - Tool response size impacts context usage significantly
   - Implement response format options: concise (essential fields only) vs detailed (complete objects)
   - Include guidance in descriptions about when to use each format
   - Agents learn to select appropriate formats based on task requirements

6. `## Build for Future Models` —
   - Models improve faster than tooling
   - Architecture optimized for today's model may be over-constrained for tomorrow's
   - Build minimal architectures that benefit from model improvements
   - Don't lock in current limitations with sophisticated scaffolding
   - Stop constraining reasoning — pre-filtering, option constraining, validation wrapping often become liabilities as models improve
   - The Bitter Lesson: general methods that leverage computation ultimately win over hand-crafted approaches

7. `## Collection Design` —
   - 10-20 tools is the guideline for most applications
   - If more are needed, use namespacing to create logical groupings
   - Tool description overlap causes model confusion — audit for overlapping purpose
   - MCP tools: Always use fully qualified names (ServerName:tool_name) to avoid "tool not found" errors
   - Use agents to optimize tools: collect failure modes, analyze with agent, propose improved descriptions. Production testing shows 40% reduction in task completion time.

8. `## Anti-Patterns` —
   - **Vague descriptions**: "Search the database" — leaves too many questions unanswered
   - **Cryptic parameter names**: x, val, param1 — forces guessing
   - **Missing error recovery guidance**: Generic errors with no correction path
   - **Inconsistent naming**: id in some tools, identifier in others, customer_id in yet others
   - **Over-tooling**: Building specialized wrappers for operations the model handles better with primitives

**Step 2: Commit**

```bash
git add skills/context-engineering/references/tool-design-principles.md
git commit -m "Add tool design principles reference"
```

---

### Task 5: Register in marketplace.json and update README

**Files:**
- Modify: `.claude-plugin/marketplace.json`
- Modify: `README.md`

**Step 1: Update marketplace.json**

Add a new plugin group entry to the `plugins` array in `.claude-plugin/marketplace.json`:

```json
{
  "name": "agent-engineering",
  "description": "Skills for designing and operating AI agent systems.",
  "source": "./",
  "strict": false,
  "skills": ["./skills/context-engineering"]
}
```

The full plugins array should now have 3 entries: software-development-skills, quality-gates, agent-engineering.

**Step 2: Update README.md**

Add a new section for the context-engineering skill after the Fresh-Eyes Review section, following the same format:

```markdown
### Context Engineering
Operational decision frameworks for context window management, tool design, and agent coordination. Distills empirical research into situation-driven diagnostics and actionable checklists.

**Use when**: Managing long agent sessions, diagnosing degraded agent behavior, designing tools or skills, choosing compression strategies, or deciding whether to use multi-agent architectures.

**Delivers**: Degradation diagnostics (5 patterns with symptoms and mitigations), compression strategy selection, tool design quality checklists, anti-patterns, and resistance patterns.
```

**Step 3: Verify marketplace.json parses**

Run: `python3 -c "import json; json.load(open('.claude-plugin/marketplace.json')); print('Valid JSON')"`
Expected: `Valid JSON`

**Step 4: Commit**

```bash
git add .claude-plugin/marketplace.json README.md
git commit -m "Register context-engineering in agent-engineering plugin group"
```

---

### Task 6: Final verification

**Step 1: Verify all files exist**

Run: `ls -la skills/context-engineering/SKILL.md skills/context-engineering/references/`
Expected: SKILL.md exists, references/ contains 3 .md files

**Step 2: Verify reference links resolve**

Run: `grep -o '\./references/[a-z-]*\.md' skills/context-engineering/SKILL.md`
Expected: 3 paths, each matching a file in the references directory

**Step 3: Verify SKILL.md is under 500 lines**

Run: `wc -l skills/context-engineering/SKILL.md`
Expected: Under 500 lines

**Step 4: Verify marketplace.json has 3 plugin groups**

Run: `python3 -c "import json; d=json.load(open('.claude-plugin/marketplace.json')); print(len(d['plugins']), 'plugin groups:', [p['name'] for p in d['plugins']])"`
Expected: `3 plugin groups: ['software-development-skills', 'quality-gates', 'agent-engineering']`

**Step 5: Run documentation-audit skill against the new skill (optional)**

If time permits, invoke the documentation-audit skill targeting `skills/context-engineering/` to verify all claims in the SKILL.md are accurate and reference links resolve.
