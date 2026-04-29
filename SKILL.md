---
name: prompt55
description: This skill should be used when the user asks to "optimize this prompt for GPT-5.5", "revise this plan for gpt-5.5", "migrate this prompt to 5.5", "tighten this agent prompt", "improve retrieval budget", "improve validation rules", "add preamble behavior", or otherwise improve prompts, plans, agent instructions, workflow specs, or Responses API guidance for GPT-5.5.
version: 0.1.0
---

# prompt55

Revise prompts, plans, agent instructions, and workflow specs so they fit GPT-5.5 instead of carrying forward older, process-heavy prompt stacks.

Use this skill to produce concrete rewritten artifacts. Prefer compact, outcome-first instructions that preserve the product contract, remove stale control text, and add GPT-5.5-specific guidance only where it changes behavior.

## Quick Triage

- Prompt rewrite: load `references/templates.md`, then return a ready-to-paste prompt.
- Plan rewrite: load `references/templates.md`, then return a decision-complete implementation plan.
- GPT-5.5 model behavior, API behavior, or Responses workflow detail: load `references/gpt-55-prompting-guide.md`; use the `openai-docs` skill before making current OpenAI/API claims that may drift.
- Generic copyediting with no GPT-5.5, agent, planning, retrieval, validation, or API angle: do not use this skill.

## Workflow

1. Identify the target surface.
   Classify the source as a prompt, implementation plan, `AGENTS.md`, product assistant instruction set, tool workflow, eval spec, or API harness guidance.
2. Preserve the contract.
   Keep the user's goal, audience, required output format, domain constraints, safety rules, business rules, and side-effect limits before changing style.
3. Remove legacy prompt weight.
   Cut duplicated rules, exhaustive step lists, unnecessary `ALWAYS` or `NEVER` language, vague personality padding, and process instructions that do not protect correctness.
4. Rebuild around the outcome.
   State the role, goal, success criteria, constraints, available evidence, output shape, and stop rules. Let the model choose the efficient solution path unless a step is truly required.
5. Add GPT-5.5 controls selectively.
   Add reasoning effort, `text.verbosity`, preamble behavior, retrieval budget, citation rules, structured output guidance, tool-description placement, `phase` replay, or validation loops only when the target surface needs them.
6. Return the revised artifact.
   Provide concrete replacement text first. Add a short note only when placement, removed behavior, or API/runtime changes matter.

## Rewrite Rules

- Prefer shorter prompts that define what good looks like over long procedural scripts.
- Use absolute rules only for invariants: safety, required fields, side effects, compliance, citations, or output schema.
- Convert judgment calls into decision rules: when to search, when to ask, when to retry, when to stop, and when to validate.
- Replace vague over-retrieval instructions such as "search a lot", "research thoroughly", or "keep looking" with a retrieval budget that defines the minimum evidence needed, when another retrieval call is justified, and when to stop.
- For grounded workflows, define what claims need evidence, what counts as enough evidence, and what to do when evidence is missing.
- For coding or production workflows, include the most relevant validation commands or checks and require a brief explanation if validation cannot run.
- For visual artifacts, require rendering and inspection before finalizing when the environment makes that possible.
- For Responses API harnesses, prefer `previous_response_id` for state. If assistant output items are manually replayed, preserve returned `phase` values exactly.
- For Structured Outputs, recommend schema enforcement through the API instead of describing complex schemas only in natural language.

## Output Expectations

Default output for a prompt rewrite:

1. Revised prompt
2. Short notes on important changes, only if useful

Default output for a plan rewrite:

1. Revised plan
2. Assumptions and open questions that materially affect implementation
3. Validation checks

Keep output concise. Do not include a long critique unless the user asks for diagnosis.

## Reference Files

- `references/gpt-55-prompting-guide.md` - compact source-backed GPT-5.5 prompting guidance and OpenAI doc links.
- `references/templates.md` - reusable prompt, plan, retrieval, validation, and Responses workflow templates.
