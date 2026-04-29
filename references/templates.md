# prompt55 Templates

Use these patterns as starting points. Keep only sections that change behavior.

## Compact Prompt Structure

```text
Role: [1-2 sentences defining the assistant's job and context.]

# Goal
[State the user-visible outcome.]

# Success Criteria
- [Observable condition 1]
- [Observable condition 2]
- [Required completed action, decision, or artifact]

# Constraints
- [Safety, policy, business, evidence, privacy, and side-effect limits]
- [What not to change or invent]

# Evidence And Tools
- [Available sources or tools]
- [When to retrieve more evidence]
- [What to do if evidence is missing]

# Output
[Required shape, length, tone, and fields.]

# Stop Rules
[When to answer, ask, retry, fallback, or abstain.]
```

## Prompt Revision Checklist

- Preserve the user's actual goal, audience, and product contract.
- Preserve required output format, length, structure, safety rules, and business rules.
- Remove duplicated process instructions and broad `ALWAYS` or `NEVER` language unless it protects an invariant.
- Replace step-by-step micromanagement with success criteria and decision rules.
- Replace "search a lot" or "research thoroughly" with a retrieval budget. Do not preserve over-retrieval as a virtue.
- Add missing evidence, retrieval, validation, and stop behavior.
- Keep personality short and separate from task behavior.
- Prefer one compact replacement prompt over critique plus scattered suggestions.

## Plan Revision Checklist

Make plans decision-complete enough for another engineer or agent to implement.

Include:

- goal and success criteria
- important interfaces, APIs, schemas, inputs, and outputs
- data flow or state transitions when relevant
- changed systems or files when needed to avoid ambiguity
- failure behavior and fallbacks
- privacy and security considerations
- validation commands, acceptance tests, or review checks
- open questions only when they materially affect implementation

Avoid:

- vague "improve X" bullets
- unstated defaults
- implementation choices left to the reader
- exhaustive file inventories when behavior-level guidance is clearer

## Retrieval Budget Snippet

```text
Use the minimum retrieval needed to answer correctly.

Start with one focused search or source read using short, discriminative terms. Search again only when the top results do not answer the core question, a required fact or source is missing, the user asked for exhaustive coverage, a specific artifact must be read, or the answer would otherwise contain an important unsupported factual claim.

Do not search again only to improve phrasing, add nonessential examples, or support wording that can safely be made more generic. Do not translate "search a lot" into "thoroughly research"; translate it into a clear stopping rule.
```

## Grounded Drafting Snippet

```text
Separate source-backed facts from creative wording.

Use provided or retrieved evidence for concrete product, customer, metric, roadmap, date, capability, competitive, legal, medical, or financial claims. Do not invent specifics to make the draft stronger. If support is thin, write a useful generic draft with placeholders or clearly labeled assumptions.
```

## Validation Snippet

```text
Before finalizing, run the most relevant validation available: targeted tests for changed behavior, type or lint checks when applicable, build checks for affected packages, rendered-output inspection for visual work, or a minimal smoke test when full validation is too expensive.

If validation cannot run, say why and describe the next best check.
```

## Preamble Snippet

```text
Before tool calls for a multi-step task, send a short user-visible update that acknowledges the request and states the first concrete step. Keep it to one or two sentences.
```

## Responses Phase Snippet

```text
For Responses workflows, use previous_response_id for multi-turn state when available.

If manually replaying assistant output items, preserve returned phase values exactly. Use phase: "commentary" for intermediate user-visible updates and phase: "final_answer" for the completed answer. Do not add phase to user messages.
```

## Outcome-First Rewrite Example

Prefer:

```text
Resolve the user's request end to end.

Success means:
- the decision or artifact is complete from the available evidence
- required side effects are completed before responding
- missing evidence is requested as the smallest useful question
- the final answer includes completed work, blockers, and validation
```

Avoid:

```text
First inspect A, then inspect B, then think through every possible exception, then decide which tool to call, then call the tool, then explain every step.
```
