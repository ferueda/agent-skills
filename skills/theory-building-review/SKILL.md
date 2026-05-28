---
name: theory-building-review
description: Manually triggered weekly or post-change repository audit based on Peter Naur's "Programming as Theory Building." Use this skill only when the user explicitly names `theory-building-review`, asks to run the theory-building review skill, or says to run the weekly/post-change theory-building review. Do not trigger automatically just because the conversation mentions Naur, domain modeling, refactoring, architecture, craft, or code review. When explicitly invoked, inspect recent diffs or scoped repository areas, reconstruct the system theory, find places where code and domain language diverge, and return actionable refactor, test, documentation, and modeling improvements. For active design, implementation, refactor, review, or AI-code acceptance on a specific change, use `theory-building` instead when explicitly invoked.
---

# Theory-Building Review

Use this skill to turn Naur's idea into a practical repository habit: after a week of work, a few merged changes, or a meaningful feature, review whether the code still preserves the team's theory of the domain.

This skill is intentionally manual. Do not apply it unless the user explicitly invokes this skill or asks to run the weekly theory-building review.

Use `theory-building` instead when the user is actively designing,
implementing, refactoring, reviewing, or accepting a specific change and wants
the theory-building lens applied at that point of work.

The goal is not to produce generic architecture advice. The goal is to answer: "Does the code still express the real-world model clearly enough that future programmers can change it intelligently?"

## Workflow Fit

Use this skill as the periodic audit ritual:

1. Run it weekly, after several meaningful changes, after a feature lands, or
   before a larger refactor.
2. Inspect recent diffs, touched modules, tests, docs, and domain-facing code.
3. Reconstruct the current system theory from repository evidence.
4. Identify drift, hidden rules, overloaded terms, weak boundaries, and missing
   theory-preserving tests or docs.
5. Produce small action items the team can do this week.

Pair it with `theory-building` this way:

- Use `theory-building-review` to find theory gaps across recent work.
- Use `theory-building` to design or implement a specific fix from the review.

## When To Use Which Skill

Use this skill when the user wants to step back from one or more changes and
audit whether the repository still expresses the domain theory clearly.

| Situation | Use this skill for |
|---|---|
| Weekly review | Inspect recent diffs and produce one or two theory-preserving improvements. |
| After a feature lands | Check whether new concepts, names, tests, and docs fit the model. |
| Before a refactor | Find the real theory gaps before moving code. |
| After several AI-assisted changes | Detect copied abstractions, hidden assumptions, and vocabulary drift. |
| Personal craft practice | Turn one recent module or feature into a theory-building exercise. |

Do not use this skill for active design, implementation, PR review, debugging,
or accepting one specific generated change. Use `theory-building` for those.

## Core Principle

Treat source code as an artifact of a human theory. Your review should recover that theory, compare it to the code, and identify where the theory is missing, distorted, duplicated, or hard to transfer.

Use this framing throughout:

- **World**: users, workflows, business concepts, policies, constraints, exceptions.
- **Program**: modules, types, functions, APIs, storage, tests, UI, jobs, integrations.
- **Theory**: the human explanation that connects the world to the program and justifies why the implementation is shaped this way.

## Start By Determining Scope

Prefer concrete repository evidence over broad speculation.

If the user points to files, features, commits, or a branch, use that scope.

Use these scope presets when the user does not specify one:

- **Recent diff**: Current `git diff` and changed tests/docs. Best for
  uncommitted work.
- **Last 10 commits**: Recent merged or committed work. Best for weekly review.
- **One feature/module**: A named workflow, bounded context, or directory. Best
  before refactoring.
- **Fallback scan**: High-signal files and decay markers when Git history is
  unavailable or empty.

Keep the review narrow enough that the action items are doable this week. If
the scope is too broad, state the chosen slice before reviewing.

If the user says "weekly review", "after recent changes", or gives no explicit scope, inspect the repository for recent change context:

```bash
git status --short
git diff --stat
git diff
git log --oneline -n 10
```

If the repository has no Git history or the diff is empty, inspect high-signal files instead:

```bash
rg --files
rg -n "TODO|FIXME|HACK|domain|model|policy|rule|invariant|workflow|event|command|service|manager"
```

Then read only the files needed to understand the domain surface. Prefer tests, domain modules, service/application layers, API handlers, schema/migration files, docs, and recent diffs.

## Review Depth

Choose the smallest depth that fits the request.

- **Light weekly pass**: 30-60 minutes of evidence, at most three findings, and
  one practice item.
- **Post-feature pass**: Focus on the feature's concepts, boundaries, tests,
  and any docs or ADRs needed to preserve decisions.
- **Pre-refactor pass**: Identify the domain split and invariants before
  recommending code movement.
- **Personal craft pass**: No repo-wide audit; use one feature or module as a
  practice case.

Avoid turning the review into a broad architecture wishlist. The output should
help the next week of work.

## Build A Theory Sketch

Before making recommendations, write a private or visible theory sketch. Keep it short but specific:

```text
World problem:
Main domain concepts:
Core workflows:
Rules and invariants:
Bounded contexts or ownership areas:
How the current code expresses the model:
Where the model is unclear:
```

If important facts are uncertain, label them as assumptions. Do not invent business rules that are not supported by code, tests, docs, or user-provided context.

## Review Questions

Use these questions to inspect the code. Prioritize findings that affect future modification, not only local cleanliness.

### Mapping Between World And Program

- What real-world concept does this module, type, function, or table represent?
- Can the code be explained using domain language rather than implementation language?
- Are important domain distinctions explicit in names and types?
- Are unrelated meanings sharing one generic word such as `User`, `Account`, `Status`, `Type`, `Manager`, or `Handler`?

### Invariants And Rules

- Where are business rules enforced?
- Are invariants protected at the right boundary, or scattered across callers?
- Do tests encode domain examples and edge cases, or only implementation mechanics?
- Could a future change accidentally bypass an important rule?

### Conceptual Fit

- Is the new code a natural extension of the existing model?
- Did the change introduce a second vocabulary for the same concept?
- Did it place domain behavior in infrastructure, UI, controllers, migrations, or ad hoc utilities where future maintainers may not look?
- Is there an abstraction that exists because of code shape rather than domain meaning?

### Boundaries And Context

- Does the code mix distinct bounded contexts?
- Does one module know too much about another context's internal language?
- Is integration code translating between models, or leaking foreign concepts everywhere?
- Would a domain expert recognize the separation of responsibilities?

### Theory Preservation

- What would a new engineer need to know to change this safely?
- Is that knowledge visible in tests, names, docs, ADRs, examples, or review notes?
- Which decisions are only implicit in the heads of current maintainers?
- Where would a future maintainer likely make a locally correct but globally wrong change?

## Decay Signals

Watch for these signs that the theory is weakening:

- New code duplicates an existing concept with different names.
- A generic service or utility owns business rules from multiple domains.
- Rules are enforced by caller discipline instead of model boundaries.
- Tests assert incidental implementation details but not business outcomes.
- Names describe data shape rather than domain meaning.
- A change is implemented as a special case without explaining the domain reason.
- A single enum or status field tries to represent multiple lifecycle concepts.
- Code comments explain what the code does but not why the domain requires it.
- AI-generated or boilerplate-looking code introduces abstractions not used elsewhere.

## Finding Severity

Use severity to communicate practical risk, not taste.

- **P1**: A hidden or duplicated rule can cause incorrect behavior, security,
  billing, permission, data-loss, or state-transition mistakes.
- **P2**: A naming, boundary, or model mismatch is likely to make future changes
  unsafe or confusing.
- **P3**: A local theory-preservation improvement would help maintainability,
  onboarding, or review quality but is not urgent.

Skip findings that are only style preferences. Every finding should connect
repository evidence to a modification risk.

## Output Format

Use this structure for repository reviews:

```markdown
**Theory Summary**
[Short explanation of the domain theory you recovered.]

**Strong Signals**
- [Where the code expresses the theory well.]

**Findings**
- [Severity] [Finding title] - [file/line reference when possible]
  [Why this weakens the theory or future modification path.]

**Action Items**
- [Concrete change: refactor, rename, test, doc, ADR, or investigation.]

**This Week's Practice**
- [One to three small exercises the engineer/team should do next.]

**Open Questions**
- [Questions that require domain owner or maintainer confirmation.]
```

Keep findings grounded. Each finding should connect code evidence to one of these impacts:

- harder to explain the system
- harder to modify safely
- hidden or duplicated domain rule
- unclear boundary
- mismatch between domain language and code language
- missing test or documentation that would preserve the theory

For a light weekly pass, cap the output at three findings and three action
items. Prefer one high-leverage improvement over a long list.

## Action Item Guidance

Make action items small enough to do. Prefer:

- Rename a misleading concept to match the domain.
- Add a value object or type for a real invariant.
- Move a rule into the model boundary that owns it.
- Split an overloaded status, enum, or service when it represents multiple concepts.
- Add a domain example test for a rule or edge case.
- Write a short ADR for a meaningful modeling choice.
- Add a glossary entry for a contested term.
- Add an anti-corruption mapping when an external API term leaks into the core model.

Avoid vague recommendations like "improve architecture", "add documentation", or "clean this up" unless they are followed by an exact target and reason.

## Improve The Skill From Use

Treat this skill as a review artifact that should evolve from actual reviews.

- If findings are too broad, narrow the scope presets or cap output harder.
- If reviews miss recurring drift, add that drift pattern to Decay Signals.
- If action items are not getting done, make them smaller and tie each one to a
  file, test, doc, or named investigation.
- If the review overlaps with active implementation work, strengthen the
  handoff rule to `theory-building`.
- If personal craft mode is more useful than repo audit, add examples from the
  user's real workflow.

Make one or two edits after a real review, then validate them on the next
weekly or post-feature review.

## If The User Asks You To Implement Improvements

First produce the theory-building review unless the user explicitly asks to skip it. Then implement only tightly scoped changes that are supported by the review.

Good implementation candidates:

- Add or improve tests that encode domain examples.
- Rename local symbols when the blast radius is clear.
- Extract a value object or domain helper when it protects an invariant.
- Add a short ADR or domain note.
- Move duplicated rule logic into an existing domain boundary.

Risky candidates that need extra care:

- Changing persistence shape.
- Redrawing bounded contexts.
- Replacing a public API vocabulary.
- Collapsing two concepts that might be meaningfully separate.

After implementation, run the repository's relevant tests and summarize what changed.

## Personal Craft Mode

If the user is not asking about a specific repository and instead wants to improve their craft, run this as a personal exercise:

1. Pick one feature or module they touched recently.
2. Ask them to explain the real-world workflow without code.
3. Ask for the main invariants and exceptions.
4. Compare that explanation to the code shape.
5. Identify one name, test, doc, or boundary that would better preserve the theory.

Return a short practice plan with one exercise, one artifact to create, and one review question to carry into their next code review.

## Quality Bar

A good theory-building review should feel like a senior engineer looked at the code and asked whether it still makes sense as a model of the world.

It should be concrete, scoped, and useful even if no code is changed immediately. The best output gives the user one or two improvements they can make this week and a sharper vocabulary for future design discussions.
