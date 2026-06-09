---
name: create-plan
description: Create a scoped, code-backed implementation plan from a todo, spec, issue, review notes, or raw user instructions. Use when the user asks to convert requirements into a proper plan, phased implementation plan, execution plan, or reviewable planning artifact before coding.
---

# Create Plan

Create a portable implementation plan from requirements. The input may be a file, issue, review thread, design note, or plain instructions from the user.

## When to Use

Use this skill when the user asks to:

- "Create a plan..."
- "Turn this todo/spec into a plan..."
- "Make this into implementation phases..."
- "Plan what to build and how to test it..."
- "Write a detailed implementation plan..."

Do not use it when the user has already approved a plan and wants implementation. Use an implementation skill instead.

## Core Principles

- **Requirements first**: Treat user requirements and source artifacts as the source of truth.
- **Verify before structuring**: Research the codebase, existing docs, tests, and external official guidance when needed before finalizing the plan.
- **Challenge source claims**: Do not treat a todo, spec, issue, or review as fact. Validate it against current system behavior.
- **Decisions, not code**: Capture approach, boundaries, files, dependencies, risks, and test scenarios. Do not pre-write implementation code or shell command choreography.
- **Right-size the artifact**: Small work gets a compact plan. Large or cross-area work gets more structure.
- **Separate planning from execution discovery**: Resolve planning-time questions. Explicitly defer implementation-time unknowns.
- **Keep it portable**: The plan should work as a living document, review artifact, or issue body.

## Workflow

1. Identify inputs and output shape.
   - Source may be a file path, pasted text, issue, review comments, or direct instructions.
   - If the user names an output path, write there.
   - If no output path is named, infer the repo's planning-doc convention when one exists; otherwise ask whether to write a file or return the plan inline.
   - Preserve source artifacts unless the user explicitly asks to edit them.

2. Build context.
   - Read repository guidance files such as `AGENTS.md`, `README.md`, architecture docs, and local learnings when present.
   - Read the source artifact fully.
   - Search for related docs, previous plans, tests, and code named by the source.
   - Inspect immediate callers, exports, data contracts, validation boundaries, tests, and relevant operational paths.
   - Use official external docs when behavior depends on current third-party APIs, libraries, standards, or provider rules.

3. Reconcile requirements with reality.
   - Separate verified current behavior from requested changes.
   - Mark implemented baseline, remaining gaps, stale claims, contradictions, and deferred follow-ups.
   - Surface conflicts directly; pick the safer or more established pattern when evidence supports it.
   - State assumptions explicitly.
   - List open questions only when they materially change implementation. Include a recommendation and why for each.

4. Design the plan.
   - Define what is being built, why it matters, and expected behavior.
   - Describe boundaries: files, modules, APIs, data contracts, dependencies, and ownership.
   - Split work into logical, atomic phases that can be implemented and reviewed independently when practical.
   - Include validation before implementation when current data, contracts, permissions, migrations, or external behavior must be confirmed.
   - Include tests that verify intent, not just surface behavior.
   - Include manual QA or operational checks when automated tests cannot cover the risk.

5. Keep the plan non-executable.
   - Avoid production code snippets.
   - Avoid step-by-step shell command transcripts.
   - Directional pseudocode, SQL sketches, API shapes, or DSL examples are acceptable only when they help reviewers validate the approach.
   - Label sketches as directional guidance, not implementation specification.

## Default Plan Structure

Trim or expand this structure based on task size:

```markdown
# <Title> Implementation Plan

Status: planned.

Source reference: <file, issue, notes, or "user instructions">.

## Goal
<What we are building and the expected behavior.>

## Contract
<Invariants, user-visible behavior, API/data contracts, or success criteria.>

## Why
<Why this change matters. Tie it to product, reliability, performance, security, maintainability, or developer experience.>

## Current Reality
<Verified facts from code/docs/tests. Distinguish implemented baseline from gaps.>

## Scope
<What is included.>

## Non-Goals
<What is explicitly excluded.>

## Assumptions And Open Questions
<Only material unknowns. Include recommendation + why.>

## Pre-Implementation Validation
<Checks or facts to confirm before editing. Include expected results, not command choreography.>

## Implementation Phases

### Phase 1: <Name>
What:
How:
Why:
Where:
Tests:
Exit Criteria:
Risks:

### Phase 2: <Name>
...

## Verification
<Automated tests, lint/type checks, migration checks, integration tests, manual QA, and acceptance scenarios.>

## Documentation And Cleanup
<Docs or follow-up cleanup required after implementation.>
```

## Quality Bar

- Prefer existing project patterns over new abstractions.
- Keep phases scoped and self-contained.
- Make testing proportional to risk and blast radius.
- Include regression tests for bug fixes when practical.
- Put parsing and validation at system boundaries; keep core logic described in precise domain terms.
- Avoid over-planning speculative future work.
- If source material is stale, say so and plan from verified current reality.
- If the plan should be written to a file, create the file and report its path.

## Output

End with:

- plan path or note that the plan was returned inline;
- key corrections or assumptions made against the source;
- open questions or risks;
- verification or doc-only checks performed.
