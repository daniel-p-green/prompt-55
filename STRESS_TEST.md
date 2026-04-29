# prompt55 Stress Test

Ran a fresh Codex process against the skill files on disk:

```bash
codex -a never -s read-only exec --ephemeral --skip-git-repo-check --output-last-message /tmp/prompt55-stress-answer.md 'Stress test the skill at ~/.codex/skills/prompt55/SKILL.md. Read only the skill files needed. Apply it to these cases and return: PASS/FAIL per case, revised artifact, and one sentence explaining whether prompt55 helped.

Case 1 prompt: "Be helpful and search a lot before answering."
Case 2 plan: "Build the dashboard. First inspect every file, then create abstractions for all data sources, then add charts, then write a long report."
Case 3 Responses workflow prompt: "Always call tools before responding. Replay prior messages manually. Tell users when done."

Evaluation criteria: GPT-5.5 outcome-first, concise, avoids process-heavy prompt stacks, uses retrieval budget instead of search a lot, includes validation/stop rules where useful, preserves the user intent.'
```

## Results

### Case 1: PASS

The skill turned "search a lot" into a concrete retrieval budget and stop rule.

```text
Be helpful, accurate, and concise.

Use the minimum retrieval needed to answer correctly. Start with one focused search or source read when the answer depends on current, specific, or source-backed facts. Search again only if a required fact is missing, sources conflict, or the user asked for exhaustive coverage.

If enough evidence is available, answer directly. If evidence is missing and retrieval cannot resolve it, state the uncertainty and give the best supported answer.
```

### Case 2: PASS

The skill preserved the dashboard goal while cutting process-heavy inspection, premature abstraction, and the long-report requirement.

```text
Build the dashboard.

Goal:
Deliver a working dashboard that shows the required charts from the available data sources with clear loading, empty, and error states.

Plan:
1. Inspect the existing dashboard entry points, data-fetching code, charting patterns, and relevant tests.
2. Identify the data sources needed for the dashboard and keep the integration direct unless repeated behavior justifies a small shared helper.
3. Implement the dashboard UI and charts using existing project conventions.
4. Add focused tests or smoke checks for data loading, rendering, and failure states.
5. Provide a concise handoff: what changed, how to run it, and what was validated.

Stop rules:
- Do not inspect unrelated files once the dashboard surface, data flow, and conventions are clear.
- Do not create broad abstractions unless at least two data sources need the same behavior now.
- Do not write a long report; summarize only decisions, validation, and remaining risks.
```

### Case 3: PASS

The skill replaced unconditional tool calls and manual replay with Responses-specific state rules, selective tool use, and a cleaner completion contract.

```text
Use the Responses API workflow efficiently.

State:
Use previous_response_id for multi-turn continuity when available. For stateless or Zero Data Retention flows, replay only the relevant returned output items needed for continuity. If replaying assistant output items, preserve returned phase values exactly. Do not add phase values to user messages.

Tools:
Call tools only when they are needed to answer correctly, perform a requested action, inspect external state, or validate work. For multi-step tool work, send a brief commentary preamble before the first tool call.

Final response:
When the task is complete, answer with the completed result, relevant validation, and any blocker or uncertainty. Do not say "done" unless the requested work was actually completed.
```
