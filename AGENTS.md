# AGENTS.md

Felipe owns this. Start: say hi + 1 motivating line.
Work style: telegraph; noun-phrases ok; drop grammar; min tokens.

## Agent Protocol

- Bugs: add regression test when it fits.
- Aim to keep files under ~700 LOC; guideline only (not a hard guardrail). Split/refactor when it improves clarity or testability.
- Style: telegraph. Drop filler/grammar. Min tokens.
- Add brief code comments for tricky or non-obvious logic.
- Keep files concise; extract helpers instead of “V2” copies. Use existing patterns preferentially unless you have a better approach.
- When answering questions, respond with high-confidence answers only: verify in code; do not guess.

## Commit Guidelines

- Use `git diff` to preview changes before committing.
- Commit messages: short imperative clauses (e.g., “Improve usage probe”, “Fix icon dimming”).
- Review all staged and unstaged changes and make atomic commits per file. Keep commits scoped.
- Each commit should have a clear, descriptive message that explains what was changed.
- Group related changes; avoid bundling unrelated refactors.
- Conventional Commits (feat|fix|refactor|build|ci|chore|docs|style|perf|test).

## Critical Thinking

- Fix root cause (not band-aid).
- Unsure: read more code; if still stuck, ask w/ short options.
- Conflicts: call out; pick safer path.
- Unrecognized changes: assume other agent; keep going; focus your changes. If it causes issues, stop + ask user.

## Repository Overview

A personal collection of skills for AI coding agents, maintained for use across my projects. Skills are packaged instructions, scripts, and resources that extend agent capabilities for specialized tasks.

## Planning workflow

Implementation plans live in **`dev/plans/`** with a shared **`dev/plans/README.md`** index (execution order, dependencies, status).

| Skill | Role | Plan naming |
|-------|------|-------------|
| `audit` | Codebase survey → prioritized handoff plans | `YYMMDD-short-slug.md` |
| `create-plan` | Single scoped plan from a todo/spec/issue | `YYMMDD-short-slug.md` |
| `implement-plan` | Execute an approved plan phase-by-phase | reads from `dev/plans/` |

`audit` is read-only on source; only `dev/plans/` files may be created or updated. Reconcile with existing `dev/plans/README.md` before adding plans.

**Plan skill discovery (required for `audit` and `create-plan`):** Before writing a plan, check the host available-skills list (if injected), repo skill dirs (`skills/`, `.agents/skills/`, `.cursor/skills/`, `.claude/skills/`), and `AGENTS.md` / `README` for named tools. Read each candidate `SKILL.md`; embed only verified skills in the plan's **Skills for the executor** table, each tied to a specific step. Never invent skill names.

## Directory Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill instructions and usage
    agents/openai.yaml    # Recommended: UI metadata and default prompt
    references/           # Optional: templates, playbooks (progressive disclosure)
    scripts/              # Optional: helper scripts and utilities
    examples/             # Optional: reference implementations
    resources/            # Optional: templates, assets, or data files
dev/plans/                # In target repos: implementation plans + README index
automations/              # Continuous background execution automations
  {automation-name}.md    # Markdown file describing the automation
```

## Automations

Automations are independent, background tasks designed to enforce project standards and prevent regressions.

### Creating a New Automation

- **Location**: Place in the `automations/` folder.
- **Naming**: Use `kebab-case.md` (e.g., `find-bugs.md`).
- **Format**: Include the goal, strategy, rules, and output expectations.

## Creating a New Skill

### Skill Directory Naming

- **Skill directory**: `kebab-case` (e.g., `audit`, `create-plan`, `review-implementation`)
- **SKILL.md**: Always uppercase, always this exact filename
- **Scripts**: `kebab-case.sh` or `kebab-case.py` (e.g., `deploy.sh`, `validate.py`)

### SKILL.md Format

```markdown
---
name: {skill-name}
description: {One sentence describing when to use this skill. Include trigger phrases like "Create a plan", "Browse the web", etc.}
---

# {Skill Title}

{Brief description of what the skill does.}

## When to Use

{Describe the scenarios where this skill should be activated}

## How It Works

{Numbered list explaining the skill's workflow}

## Usage

{Show how to invoke the skill or use its scripts}

**Examples:**
{Show 2-3 common usage patterns}

## Output

{Describe what the skill produces or what the agent should do after}
```

### agents/openai.yaml Format

Recommended for new skills. Keep this metadata aligned with `SKILL.md`.

```yaml
interface:
  display_name: "Create Plan"
  short_description: "Turn instructions into implementation plans"
  default_prompt: "Use $create-plan to turn these requirements into a scoped, code-backed implementation plan."
```

## Best Practices for Context Efficiency

Skills are loaded on-demand—only the skill name and description are loaded at startup. The full `SKILL.md` loads into context only when the agent decides the skill is relevant. To minimize context usage:

- **Keep SKILL.md under 500 lines** — put detailed reference material in separate files
- **Write specific descriptions** — helps the agent know exactly when to activate the skill
- **Use progressive disclosure** — reference supporting files that get read only when needed
- **Prefer scripts over inline instructions** — script execution doesn't consume context (only output does)
- **One skill, one purpose** — keep skills focused on a single capability
- **Self-contained examples** — include complete, runnable examples

## Naming Conventions

| Item | Convention | Example |
|------|------------|---------|
| Skill directory | `kebab-case` | `review-implementation` |
| Script files | `kebab-case.{ext}` | `validate.sh` |
| SKILL.md | Exact name, uppercase | `SKILL.md` |
| UI metadata | `agents/openai.yaml` | `agents/openai.yaml` |

## Contributing

When adding new skills:

1. Follow the naming conventions above
2. Include clear trigger phrases in descriptions
3. Add `agents/openai.yaml` unless there is a specific reason to omit UI metadata
4. Test with at least one AI agent before committing
5. Update this file if introducing new patterns or conventions
