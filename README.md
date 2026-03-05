# Superpowers

A curated set of [agent skills](https://agentskills.io/home) to support my daily
workflows.

## Skills

### Python
Deep fluency in modern Python (3.14+) for production systems. Covers typing, async patterns, data engineering, performance optimization, and security best practices.

**Use when**: Writing Python code, designing APIs, building data pipelines, creating async services, or optimizing performance.

**Key topics**: uv/pyright/ruff tooling, FastAPI, Polars, structured concurrency, type safety, profiling.

### Documentation Audit
Systematically verify documentation claims against codebase reality using a two-pass approach with pattern expansion.

**Use when**: Before releases, after refactors, or when documentation drift is suspected.

**Delivers**: Timestamped audit reports with false claims, pattern analysis, and fix suggestions.

### Fresh-Eyes Review
Deliberate re-reading of changed code with psychological distance. Catches security vulnerabilities, logic errors, and business rule bugs that slip through despite passing tests.

**Use when**: About to commit, create a PR, or declare work complete — after `verification-before-completion` confirms tests pass but before code ships.

**Delivers**: Five-checklist sweep (security, logic, business rules, input validation, performance) with immediate fixes and a completion summary.
