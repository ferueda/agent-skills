---
name: theory-building
description: >
  Manually invoked point-of-work workflow for applying Peter Naur's
  "programming as theory building" during active software work. Use this skill
  only when the user explicitly names `theory-building`, asks to use the
  theory-building skill, or explicitly says to apply programming-as-theory-
  building to the current design, implementation, refactor, review, or
  AI-generated code. Do not trigger automatically for ordinary architecture,
  implementation, refactor, review, debug, documentation, AI-code, domain model,
  or maintainability tasks.
---

# Theory Building

Use this skill as a point-of-work lens. It applies while designing,
implementing, refactoring, reviewing, or accepting generated code. Treat
understanding as the primary artifact and code as the expression of that
understanding. The goal is not to slow down the work; it is to make sure the
change strengthens the system's shared theory instead of adding code that only
happens to pass locally.

## Invocation Policy

Use this skill only when the user explicitly invokes it. Good triggers include:

- "Use the theory-building skill."
- "Use `theory-building` on this change."
- "Apply theory building to this design."
- "Review this through the programming-as-theory-building lens."
- "Check whether this belongs in the system."
- "Interrogate this AI-generated code using our theory-building checklist."

## Write Policy

By default, return theory-building output in the conversation only. Do not
create, edit, or update `CONTEXT.md`, ADRs, issues, plans, task lists, source
comments, or any other files unless the user explicitly says to write it down
and specifies where and how it should be written.

## Workflow Fit

Use this skill during active work:

1. Before a meaningful change, establish the domain problem, invariant, current
   model, and why the change belongs.
2. During implementation, keep names, boundaries, and tests aligned with that
   theory.
3. During review, ask whether the code is a natural extension of the existing
   model or an isolated patch.
4. After completion, return the durable reasoning in the conversation. Do not
   write it anywhere unless the user explicitly approves the target and format.

Pair it with `theory-building-review` this way:

- `theory-building-review` finds theory gaps across recent work.
- `theory-building` helps design and implement the specific fixes without
  introducing more drift.

## When To Use Which Skill

Use this skill when the user is inside one concrete change and needs judgment
at the point of work.

| Situation | Use this skill for |
|---|---|
| Designing a feature | Establish the domain model, invariant, and design fit before implementation. |
| Implementing a change | Keep names, boundaries, and tests aligned with the current theory. |
| Reviewing a PR or diff | Check whether the change belongs in the model, after concrete bugs and risks. |
| Accepting AI-generated code | Inspect assumptions before adopting the code. |
| Fixing a finding from `theory-building-review` | Design the specific correction without adding more drift. |

Do not use this skill for broad repository audits, weekly reviews, or
post-feature drift checks. Use `theory-building-review` for those.

## Operating Modes

Choose the lightest mode that can protect the work.

- **5-minute check**: For ordinary changes, answer only: domain problem,
  world-to-program mapping, invariant, and whether the change belongs.
- **Design pass**: For features, migrations, state changes, permissions,
  billing, routing, external integrations, or lifecycle work, include
  alternatives, tradeoffs, tests, and open questions.
- **Review pass**: For PRs or generated code, lead with concrete findings and
  add theory-fit concerns only when they affect maintainability or future
  modification.
- **Implementation support**: If the user asks to implement, first state the
  theory being protected, then make the change within that scope.

Prefer the 5-minute check unless the change is high-risk or conceptually
ambiguous.

## Core Principle

A good programmer understands the mapping between world and program. You should
be able to say: "This part of the code corresponds to this real-world activity,
rule, object, constraint, or workflow." You should also be able to go the other
way: "This real-world case is handled here, for this reason."

Treat every codebase as having three layers:

- The world: users, business rules, workflows, constraints, and edge cases.
- The program: code, data structures, APIs, tests, UI, and infrastructure.
- The theory: the human understanding that explains why the program is the way
  it is.

Most engineering dysfunction happens when people work only at the program
layer. They move code around, add endpoints, create tables, patch bugs, or ask
AI to generate implementations, but they do not update or preserve the theory.

Modification is the real test of understanding. Software inevitably changes,
and the hard part is not editing text; it is judging how a new requirement
relates to the existing theory of the system. Many implementations can produce
the same external behavior, but only some fit the system's underlying theory.

Before making or accepting a meaningful code change, establish the theory:

- What domain problem is this solving?
- What system concept does the code represent?
- What invariant or behavior must remain true?
- What local convention or architecture should it fit?
- What tradeoff are we choosing, and what alternatives are we rejecting?
- How should a future engineer know whether a later change is compatible?
- Is this a natural extension of the current model, or an isolated patch?

If those answers are unclear, clarify them before treating the implementation
as done.

## Workflow

### 1. Build The Theory Before Code

For non-trivial design, implementation, review, refactor, or debugging tasks:

- Inspect existing code, tests, docs, `CONTEXT.md`, ADRs, READMEs, and nearby
  naming before proposing new concepts.
- Identify the system's current vocabulary. Prefer existing domain terms unless
  they are wrong or overloaded.
- Map both directions before choosing an implementation:
  - From world to program: where is this real-world case handled, and why there?
  - From program to world: what activity, rule, object, constraint, or workflow
    does this code represent?
- Write a short internal theory note before editing:
  - Domain problem
  - World-to-program mapping
  - Existing model or convention
  - Invariant or behavior to protect
  - Chosen design direction
  - Main tradeoff or open question

Only show this note to the user when it helps them evaluate the work. Keep it
brief; the value is in disciplined thought, not ceremony.

### 2. Interrogate AI-Generated Code

When using, reviewing, or accepting generated code, pause before adoption:

- What assumptions did the generated code make?
- Does it use this codebase's vocabulary and abstractions?
- Did it introduce a new concept where an existing one already exists?
- What failure mode, lifecycle state, permission rule, or boundary did it miss?
- Would a future maintainer understand why this belongs here?

Generated code is acceptable when it expresses the theory cleanly. It is risky
when it smuggles in a different theory of the system.

Use this fast acceptance test:

- What concept did the generated code assume exists?
- Does that concept already exist under another name?
- Which invariant could this code bypass?
- Where would a future maintainer expect this behavior to live?
- What test or example would prove the code matches the intended theory?

### 3. Implement For Fit

When editing code:

- Prefer existing local patterns, helper APIs, naming, and boundaries.
- Keep abstractions tied to real system concepts, not incidental mechanics.
- Preserve invariants in the most local place that makes sense.
- Add tests where the theory could otherwise regress: domain rules, state
  transitions, permissions, boundaries, or previously misunderstood behavior.
- For high-risk changes, sequence action before implementation: confirm the
  theory if it is ambiguous, add or identify regression tests for the invariant,
  then make the change. Treat security, auth, billing, routing, state-machine,
  lifecycle, migration, and external-system integration changes as high risk by
  default.
- Avoid broad refactors unless they are necessary to make the intended theory
  explicit and coherent.

### 4. Review For Belonging

In addition to correctness, review whether the change belongs in the system:

- Does the code match the domain language?
- Does it reinforce or weaken the existing model?
- Are new names, states, or abstractions justified?
- Are the rules located where future programmers will expect them?
- Is the new requirement genuinely similar to existing behavior, or only
  superficially similar?
- Would the original designer see this as a natural extension or as a patch?
- Are tradeoffs visible enough for a future maintainer?
- What durable context would a future maintainer need?

For code review requests, still lead with concrete bugs and risks. Add theory
or architectural-fit concerns when they affect maintainability or future change.

### 5. Preserve The Theory

Surface durable understanding in the response so the user can decide what to do
with it:

- Use `CONTEXT.md` or an equivalent glossary for domain vocabulary and concept
  boundaries only when the user explicitly asks you to write there. Keep it
  free of implementation details.
- Use ADRs for decisions that are hard to reverse, surprising without context,
  and the result of a real tradeoff only when the user explicitly asks for one.
- Use comments sparingly for local reasoning that cannot be made obvious through
  naming or structure, and only as part of an approved code edit.
- Use final responses to mention important conceptual decisions, not every
  mechanical edit.

Do not create documentation for every small change. Do not write documentation
or task artifacts by default. Recommend capturing context only when the decision
would otherwise be lost and future work would pay for that loss; then wait for
the user to say exactly where and how to write it.

### 6. Improve The Skill From Use

Treat this skill as a small procedural artifact that improves through real
work. After using it, notice whether the output was useful.

- If the analysis was too abstract, add a sharper example or output constraint.
- If it slowed down a small change, prefer the 5-minute check next time.
- If it missed a recurring failure mode, add one question to the relevant
  checklist.
- If it wrote or proposed durable docs too eagerly, strengthen the write policy.
- If it produced implementation steps during a pure theory pass, tighten the
  separation between analysis and planning.

Make only small edits to the skill at a time, then validate them on the next
similar task. Keep changes that make the work clearer, safer to modify, or
easier to explain.

### 7. Keep The Judgment Evidence-Based

Avoid unsupported comparative or editorial claims such as "better than most
legacy modules" unless the user asks for qualitative judgment. Prefer phrasing
that ties conclusions to observed structure, behavior, tests, or documentation.

Example:

```markdown
The implementation mostly supports a coherent theory: services validate user
intent, the manager owns local billing objects, tasks own external side effects,
and sync reconciles external truth.
```

### 8. Separate Theory Passes From Implementation Plans

A theory pass explains the model, mappings, invariants, pressure points, and
next moves. It should not silently become an implementation plan or start making
changes. If the user asks to implement, sequence the work explicitly and then
execute within the approved scope.

## Output Patterns

### For Theory Passes

Return the analysis in the conversation using this structure when it fits the
request:

```markdown
**Theory**
[Core domain model, ownership, and durable concepts.]

**World -> Program Mapping**

- [Real-world case/workflow]: [code location, persisted anchor, external/system
  anchor if any, and why it belongs there.]

**Invariants**

- [Rule that must remain true.]

**Conceptual Fit / Pressure Points**

- [Where the current program strengthens, weakens, or obscures the theory.]

**Action Items**

- [Sequenced next step, starting with clarification or tests when needed.]
```

Keep domain theory, code mapping, and implementation cleanup distinct. Keep
action items concrete and sequenced. For high-risk work, prefer this order:
confirm the intended theory, add or identify regression tests for the important
invariants, make the implementation change, then explicitly defer unsupported
behavior or follow-up persistence if needed. Do not write files without explicit
user approval.

For the 5-minute check, use this shorter form:

```markdown
**Theory**
[The concept and why this change belongs or does not belong.]

**Invariant**
[The rule or behavior to protect.]

**Fit**
[Where the implementation should live and what to avoid.]

**Next Step**
[One concrete action: clarify, test, implement, or reject.]
```

### For Implementation Work

When useful, include a concise final note:

```markdown
Implemented the change in [files]. The important design choice was [decision],
because [reason/tradeoff]. Verified with [tests/checks].
```

### For Design Or Planning

Prefer this structure:

```markdown
**Theory**
[Domain problem, concepts, invariants, and current model.]

**World -> Program Mapping**
[Real-world workflows mapped to code locations and anchors.]

**Decision**
[Recommended design and why it fits.]

**Tradeoffs**
[Alternatives rejected and consequences.]

**Action Items**
[Concrete steps to implement or validate.]
```

### For Code Review

Prioritize findings first. For each conceptual issue, explain the practical
risk:

```markdown
[P1/P2/P3] This introduces a second meaning for "Account"

The existing model uses Account for billing ownership, but this change uses it
for login identity. That makes authorization checks ambiguous and future fixes
likely to patch the wrong concept.
```

### For AI-Code Acceptance

Use a short acceptance checklist:

- Fits existing vocabulary
- Preserves known invariants
- Uses local patterns and boundaries
- Handles relevant failure states
- Has tests or a clear reason tests are unnecessary
- Proposes where durable decisions should be documented when needed

## Craft Habits To Reinforce

- Write intent before implementation.
- Use AI after forming a theory, not as a substitute for one.
- Ask "does this belong?" during review.
- Keep lightweight ADRs for meaningful choices.
- Maintain a domain glossary when concepts matter.
- Practice explaining modules by purpose, responsibility, collaborators, and
  limits rather than line-by-line mechanics.
- Mentor through reasoning: ask for intent, constraints, and tradeoffs before
  offering an answer.
- Treat generated code as a proposal whose assumptions must be inspected.
