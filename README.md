# prompt-55

`prompt-55` is a Codex skill for revising prompts, plans, agent instructions, and workflow specs for GPT-5.5.

It favors shorter, outcome-first prompts with explicit success criteria, retrieval budgets, stop rules, and validation loops instead of older process-heavy prompt stacks.

## Use Cases

- Optimize a prompt for GPT-5.5.
- Revise an implementation plan so it is decision-complete.
- Tighten an agent prompt without losing the product contract.
- Replace vague "search a lot" guidance with a retrieval budget.
- Add Responses API guidance for preambles, `previous_response_id`, and `phase` replay.

## When To Use It

Use `prompt-55` when a prompt, plan, or agent instruction set feels too broad, too process-heavy, or too tied to older model behavior. It is especially useful when the source material includes:

- long procedural prompt stacks
- vague research instructions like "search a lot" or "be thorough"
- missing success criteria or stopping rules
- unclear output expectations
- tool-heavy or Responses API workflows
- planning prompts that need validation, failure behavior, or handoff criteria

Do not use it for generic copyediting, tone polishing, or rewriting prose unless the goal is specifically to improve GPT-5.5 behavior.

## How To Ask

Give the skill the source prompt or plan and the target surface:

```text
Use prompt-55 to revise this support-agent prompt for GPT-5.5.
Preserve the required JSON output and escalation rules.
[paste prompt]
```

Useful context to include:

- where the prompt will run
- who the user or audience is
- required output format
- tools or data sources available
- known failure modes
- constraints that must not change

## What It Changes

`prompt-55` keeps the product contract and rewrites around:

- role, personality, goal, success criteria, constraints, output, and stop rules
- retrieval budgets instead of open-ended search instructions
- validation loops where verification is possible
- concise preambles for longer tool-heavy tasks
- Responses API state handling, including `previous_response_id` and `phase` preservation

## Install

Clone this repo into your global Codex skills folder:

```bash
git clone https://github.com/daniel-p-green/prompt-55.git ~/.codex/skills/prompt-55
```

If your Codex setup uses explicit skill config entries, add:

```toml
[[skills.config]]
path = "/absolute/path/to/.codex/skills/prompt-55/SKILL.md"
enabled = true
```

Replace the path with the real `SKILL.md` path on your machine. Restart Codex or start a new session so the skill list refreshes.

## Example

Ask:

```text
Use prompt-55 to optimize this prompt for GPT-5.5:
"Be helpful and search a lot before answering."
```

Expected direction:

```text
Be helpful, accurate, and concise.

Use the minimum retrieval needed to answer correctly. Start with one focused search or source read when the answer depends on current, specific, or source-backed facts. Search again only if a required fact is missing, sources conflict, or the user asked for exhaustive coverage.

If enough evidence is available, answer directly. If evidence is missing and retrieval cannot resolve it, state the uncertainty and give the best supported answer.
```

## Contents

- `SKILL.md` - skill entrypoint and workflow.
- `references/gpt-55-prompting-guide.md` - compact GPT-5.5 prompting reference with OpenAI doc links.
- `references/templates.md` - reusable rewrite, retrieval, validation, and Responses workflow snippets.

## Validation

Stress tested against three cases:

- Over-retrieval prompt: passed by replacing "search a lot" with a retrieval budget and stop rule.
- Process-heavy dashboard plan: passed by preserving the goal while removing exhaustive inspection, premature abstraction, and long-report bias.
- Responses workflow prompt: passed by replacing unconditional tool calls and manual replay with selective tool use, `previous_response_id`, and `phase` preservation rules.
