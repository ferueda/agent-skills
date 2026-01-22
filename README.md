# Agent Skills

A personal collection of skills for AI coding agents. Skills are packaged instructions that extend agent capabilities for planning, implementing, and reviewing code.

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Available Skills

### ask-questions

Clarify requirements before implementing. Ensures agents ask the minimum set of must-have questions to avoid wrong work.

**Use when:**
- Request is underspecified or ambiguous
- Multiple plausible interpretations exist
- Key constraints or acceptance criteria are unclear

---

### create-plan

Create detailed implementation plans through interactive research and iteration. Produces thorough technical specifications with phased approaches.

**Use when:**
- "Create a plan for..."
- "Build a feature..."
- "Implement..."
- Starting any non-trivial feature work

**Output:** `dev/plans/YYYYMMDD-description.md`

---

### implement-plan

Execute a spec document phase-by-phase, writing robust idiomatic code that follows codebase patterns.

**Use when:**
- A plan/spec document exists and is approved
- Ready to start implementation
- "Implement this plan..."

---

### research-codebase

Research codebase comprehensively using parallel sub-agents. Documents existing architecture without suggesting changes.

**Use when:**
- "Research the codebase..."
- "Understand how X works..."
- "Investigate Y..."
- Need to map existing system behavior

**Output:** `dev/research/YYYYMMDD-description.md`

---

### review-spec

Review a spec document against codebase reality. Identifies gaps, risks, and ensures sound implementations.

**Use when:**
- "Review this spec..."
- "Check this plan..."
- Validating a plan before implementation

**Evaluates:** Architecture, Feasibility, Reliability, Performance, Security, Edge Cases, Testing

---

### summarize-work

Document completed implementation work with what was done, how, why, and files touched.

**Use when:**
- After completing a phase or full implementation
- "Summarize what was done..."
- Need implementation documentation

## Installation

Copy skills to your agent's skills directory:

```bash
cp -r skills/{skill-name} ~/.claude/skills/
```

Or reference directly in your project's agent configuration.

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected or when explicitly invoked.

**Examples:**
```
Create a plan for adding user authentication
```
```
Research how the payment system works
```
```
Review my implementation spec
```

## Skill Structure

Each skill contains:

- `SKILL.md` - Instructions for the agent (required)
- `scripts/` - Helper scripts (optional)
- `examples/` - Reference implementations (optional)
- `resources/` - Templates and assets (optional)

## License

MIT
