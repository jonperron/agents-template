# Project

## Specifications

Feature specifications live in openwiki.

Always read the relevant spec before implementing or modifying a feature.

## Tech stack

Tech stack is described in the pyproject.toml file. For isolation, uv is used to create a virtual environment locally, not in the CI.

## Golden rules

These rules apply to every task in this project unless explicitly overridden. Bias: caution over speed on non-trivial work. Use judgment on trivial tasks.

### Rule 1 — Think Before Coding

State assumptions explicitly. If uncertain, ask rather than guess. Present multiple interpretations when ambiguity exists. Push back when a simpler approach exists. Stop when confused. Name what's unclear.

### Rule 2 — Simplicity First

Minimum code that solves the problem. Nothing speculative. No features beyond what was asked. No abstractions for single-use code. Test: would a senior engineer say this is overcomplicated? If yes, simplify.

### Rule 3 — Surgical Changes

Touch only what you must. Clean up only your own mess. Don't "improve" adjacent code, comments, or formatting. Don't refactor what isn't broken. Match existing style.

### Rule 4 — Goal-Driven Execution

Define success criteria. Loop until verified. Don't follow steps. Define success and iterate. Strong success criteria let you loop independently.

### Rule 5 — Surface conflicts, don't average them

If two patterns contradict, pick one (more recent / more tested). Explain why. Flag the other for cleanup. Don't blend conflicting patterns.

### Rule 6 — Read before you write

Before adding code, read exports, immediate callers, shared utilities. "Looks orthogonal" is dangerous. If unsure why code is structured a way, ask.

### Rule 7 — Checkpoint after every significant step

Summarize what was done, what's verified, what's left. Don't continue from a state you can't describe back. If you lose track, stop and restate.

### Rule 8 — Match the codebase's conventions, even if you disagree

Conformance > taste inside the codebase. If you genuinely think a convention is harmful, surface it. Don't fork silently.

### Rule 9 - Respect CI/CD

At the end of any task, ensure that the lint defined in the CI/CD pipeline are passing when updating Python code.

### Role 10 - Memory

Store all decisions in the openwiki instance. This includes every time you deviate from the coding rules, or when you have to make a choice between two options. This will help future contributors understand the rationale behind certain decisions and maintain consistency in the codebase.

### Rule 11 - Keep the OpenAPI spec in sync

Whenever you add, remove, or change an API endpoint, its query/path parameters, or a response schema, update api/openapi.yaml in the same change. The file is maintained by hand (curated, simplified operationIds); mirror the style of the existing paths and component schemas, and keep the enums (e.g. Source, MeasureSource) consistent with api/api/schemas.py. A code change to the API contract without the matching api/openapi.yaml update is incomplete.

## Coding Rules
