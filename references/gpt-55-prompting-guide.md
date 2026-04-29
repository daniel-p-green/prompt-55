# GPT-5.5 Prompting Guide Reference

Source of truth:

- OpenAI Prompt guidance: https://developers.openai.com/api/docs/guides/prompt-guidance#gpt-5.5-prompting-guide
- OpenAI Using GPT-5.5 guide: https://developers.openai.com/api/docs/guides/latest-model#using-reasoning-models

Use this file as compact local guidance. For volatile model, API, pricing, limit, or release details, use the `openai-docs` skill before finalizing.

## Core Shift

GPT-5.5 should usually be prompted with shorter, outcome-first instructions. Define the desired result, success criteria, constraints, evidence, output shape, and stop rules. Avoid carrying over every instruction from older prompt stacks.

Legacy prompts often over-specify the process. With GPT-5.5, that can add noise, narrow the solution path, and make outputs feel mechanical. Preserve true invariants, but convert most process control into compact decision rules.

## What Changed From GPT-5.4

- Outcome-first prompts usually beat process-heavy prompt stacks.
- `low` and `medium` reasoning effort deserve fresh evaluation before escalating.
- Preambles, `phase` handling, and assistant-item replay remain important for tool-heavy Responses workflows.
- Explicit personality, retrieval budgets, and validation rules are useful for customer-facing and agentic products.
- Tool-specific rules usually belong in tool descriptions unless they apply across tools or materially change operating policy.

## Recommended Prompt Shape

Use a compact structure for complex prompts:

```text
Role: [1-2 sentences defining the function, context, and job]

# Personality
[tone, demeanor, and collaboration style]

# Goal
[user-visible outcome]

# Success criteria
[what must be true before the final answer]

# Constraints
[policy, safety, business, evidence, and side-effect limits]

# Output
[sections, length, and tone]

# Stop rules
[when to retry, fallback, abstain, ask, or stop]
```

Remove sections that do not change behavior.

## Personality And Collaboration

Keep personality short. Separate:

- Personality: how the assistant sounds.
- Collaboration style: how the assistant works.

Define warmth, directness, formality, humor, empathy, polish, when to ask questions, when to make assumptions, how much context to give, and how to handle uncertainty. Do not use personality text to compensate for unclear goals.

## Reasoning, Verbosity, And Output

- Start by testing `reasoning.effort: low` or `medium` for many workloads; escalate when quality requires it.
- Use `text.verbosity` for output length and style. Treat final answer length as separate from reasoning quality.
- Specify word budgets, section counts, table widths, or JSON-only output when needed.
- Use Structured Outputs for schema enforcement instead of describing complex schemas only in the prompt.

## Preambles And Responsiveness

For multi-step or tool-heavy tasks, ask for a short visible preamble before tool calls. The preamble should acknowledge the request and state the first concrete step in one or two sentences.

Use this when perceived latency matters, when tool use needs to be followable, or when the workflow may take multiple loops.

## Grounding And Retrieval Budgets

A retrieval budget is a stopping rule for search. It tells the model when enough evidence is enough.

Good retrieval guidance says:

- what claims need support
- what sources count as sufficient
- when another search is justified
- when to answer from available evidence
- how to behave when evidence is missing

Do not search again just to improve phrasing, add nonessential examples, or support wording that can be made more generic.

## Creative Drafting

For slides, launch copy, blurbs, summaries, talk tracks, and narrative framing, separate source-backed facts from creative wording.

Require sources for concrete product, customer, metric, roadmap, date, capability, and competitive claims. If support is thin, produce a useful generic draft with placeholders or labeled assumptions instead of inventing specifics.

## Validation Loops

Add validation rules when verification is possible.

Coding workflows should run targeted tests, type checks, lint checks, builds, or smoke tests when relevant. Visual workflows should render and inspect layout, clipping, spacing, missing content, and consistency. Planning workflows should make requirements traceable to implementation and acceptance checks.

## Responses API And State

GPT-5.5 works best in the Responses API for agentic workflows. Prefer `previous_response_id` for multi-turn state handling.

For stateless or Zero Data Retention flows, pass back the relevant returned output items each turn. If manually replaying assistant output items, preserve returned `phase` values exactly. Use `phase: "commentary"` for intermediate visible updates and `phase: "final_answer"` for completed answers when the application handles those phases.

## Current Date

GPT-5.5 is aware of the current date in UTC. Add explicit date or timezone context only when the application needs a business-specific timezone, policy-effective date, user-local date, or another non-UTC reference point.
