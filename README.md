# Agent Skills

A personal collection of skills for AI coding agents. Skills are packaged instructions that extend agent capabilities for planning, implementing, and reviewing code.

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Available Skills

### ask-questions

Clarify requirements before implementing. Ensures agents ask the minimum set of must-have questions to avoid wrong work. Explicitly invoked only.

**Use when:**
- Request is underspecified or ambiguous
- Multiple plausible interpretations exist
- Key constraints or acceptance criteria are unclear

---

### implement-plan

Implement a spec document phase-by-phase, writing robust idiomatic code that follows codebase patterns.

**Use when:**
- A plan/spec document exists and is approved
- Ready to start implementation
- "Implement this plan..."

---

### interrogator

Interview the user to extract knowledge from their head and synthesize it into a structured document. Asks one question at a time, depth-first, to produce specs, design docs, briefs, or decision records.

**Use when:**
- "Interview me about..."
- "Help me think through..."
- "I need to spec out..."
- User has a vague concept and needs help turning it into a concrete artifact

---

### react-to-review

Evaluate, analyze, and systematically react to an adversarial code review report. Decide on the action for each finding, justify the decision, and plan implementation.

**Use when:**
- An adversarial review report has been provided
- "React to this review..."
- Evaluating review findings (Implement, Adapt, Decline) and planning fixes

---

### review-implementation

Review code changes critically and adversarially. Looks for antipatterns, unnecessary complexity, bugs, policy violations, and opportunities to simplify. Default posture: assume every change adds unnecessary complexity until proven otherwise.

**Use when:**
- "Review this implementation..."
- "Review these changes..."
- "Review this branch..."
- "Adversarial review..."
- Code changes need scrutiny before merging or accepting

**Evaluates:** Correctness, Complexity, Style, Architecture, Reliability, Performance, Policy

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

Summarize work done in a spec/plan document with what was done, how, why, and files touched.

**Use when:**
- After completing a phase or full implementation
- "Summarize what was done..."
- Need implementation documentation

---

### theory-building

Point-of-work workflow for applying Peter Naur's "programming as theory building" during active design, implementation, refactoring, or review. Explicitly invoked only.

**Use when:**
- "Use the theory-building skill..."
- "Apply theory building to this design..."
- "Check whether this belongs in the system..."
- Need to verify a change strengthens the system's shared theory

---

### theory-building-review

Weekly or post-change repository audit through the theory-building lens. Inspects recent diffs, reconstructs the system theory, and finds places where code and domain language diverge. Explicitly invoked only.

**Use when:**
- "Run the theory-building review..."
- Weekly review of recent changes
- After a feature lands
- Before a larger refactor

## Automations

In addition to skills, this repository includes automations designed for continuous background execution to ensure codebase quality and reliability:

### find-bugs

A deep bug-finding automation focused on high-severity issues. It inspects recent commits to identify critical correctness bugs (data loss, crashes, security holes) that escaped review.

**Use when:**
- Running continuous background checks on newly merged code
- Looking for high-impact issues rather than stylistic nits

---

### test-coverage

A test coverage automation focused on preventing regressions. It inspects recently merged code and adds missing tests where coverage is weak and business risk is meaningful.

**Use when:**
- Automatically patching coverage gaps on new code paths
- Enforcing test requirements on critical core flows

## Installation

Install using the [skills CLI](https://skills.sh):

```bash
npx skills add ferueda/agent-skills
```

The skills CLI works with: Amp, Antigravity, Claude Code, Clawdbot, Codex, Cursor, Droid, Gemini, Gemini CLI, GitHub Copilot, Goose, Kilo, Kiro CLI, OpenCode, Roo, Trae, and Windsurf.

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected or when explicitly invoked.

**Examples:**
```
Review this implementation adversarially
```
```
Research how the payment system works
```
```
Interview me about this new feature
```

## Skill Structure

Each skill contains:

- `SKILL.md` - Instructions for the agent (required)
- `scripts/` - Helper scripts (optional)
- `examples/` - Reference implementations (optional)
- `resources/` - Templates and assets (optional)

## License

MIT
