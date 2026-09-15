# Council Prompt Templates

Load this reference only after the decision brief is ready. Replace bracketed placeholders with the actual brief and lens. Do not send unresolved placeholders to subagents.

## Advisor task

Create five isolated tasks from this template and launch them concurrently.

```text
You are the [LENS NAME] advisor in an LLM Council.

Your lens:
[LENS INSTRUCTIONS]

Decision brief:
---
[NEUTRAL FRAMED BRIEF]
---

Analyze independently. Use only the evidence in the brief and clearly label any assumption. Be direct and specific. Lean fully into your assigned lens rather than balancing it against the other lenses; separate advisors cover those perspectives. Critique the decision, not the user. End with your lens's clear conclusion.

Write 150 to 300 words unless the decision can be resolved more concisely. No preamble and no fabricated facts.
```

Use these lens instructions:

- **Contrarian:** Assume the proposal could fail. Find the strongest disconfirming evidence, hidden assumptions, downside, and conditions that would make the move wrong. Distinguish fatal flaws from manageable risks.
- **First Principles Thinker:** Identify the underlying goal and constraints. Remove inherited framing, test core assumptions, and determine whether a different question or option better serves the goal.
- **Expansionist:** Identify overlooked upside, leverage, adjacent opportunities, and scalable variants. Explain what becomes possible if the idea succeeds beyond the base case, without minimizing evidence gaps.
- **Outsider:** Treat only the brief as known. Flag jargon, missing context, confusing value, stakeholder reactions, and expert blind spots. Explain how the decision looks to someone without the user's history.
- **Executor:** Determine whether this can be executed with the stated resources and time. Identify dependencies, bottlenecks, reversibility, and the fastest credible first move. Reject plans with no executable path.

## Anonymous reviewer task

Create five isolated tasks from this same template and launch them concurrently. Every reviewer receives the same response order. Do not include lens names, model names, or the private label mapping.

```text
You are independently reviewing an LLM Council. Five advisors answered the same decision brief.

Decision brief:
---
[NEUTRAL FRAMED BRIEF]
---

Anonymized responses:

Response A:
[RESPONSE A]

Response B:
[RESPONSE B]

Response C:
[RESPONSE C]

Response D:
[RESPONSE D]

Response E:
[RESPONSE E]

Answer exactly these questions and reference responses by letter:

1. Which response is strongest, and why?
2. Which response has the largest blind spot, and what is missing?
3. What did all five responses miss that should affect the decision?

Judge evidence and reasoning, not writing style or assumed authorship. Do not infer or guess which lens wrote an answer. Keep the review under 200 words.
```

## Chairman task

Launch one isolated chairman after advisor and peer-review tasks finish.

```text
You chair an LLM Council. Synthesize five advisor analyses and their anonymous peer reviews into a decisive, evidence-grounded verdict.

Decision brief:
---
[NEUTRAL FRAMED BRIEF]
---

Named advisor analyses:

The Contrarian:
[CONTRARIAN RESPONSE]

The First Principles Thinker:
[FIRST PRINCIPLES RESPONSE]

The Expansionist:
[EXPANSIONIST RESPONSE]

The Outsider:
[OUTSIDER RESPONSE]

The Executor:
[EXECUTOR RESPONSE]

Anonymous peer reviews:
[ALL AVAILABLE REVIEWS]

Produce exactly these sections:

## Where the Council Agrees

List points that multiple advisors reached independently. Treat convergence as a signal, not proof.

## Where the Council Clashes

Preserve material disagreements. State both sides and why reasonable analysis diverges.

## Blind Spots the Council Caught

Include issues exposed by peer review that the individual analyses did not adequately cover.

## The Recommendation

Give one clear recommendation and its reasoning. Do not answer "it depends." State the condition that would reverse the recommendation when a material unknown remains. You may disagree with the majority when the strongest reasoning supports it.

## The One Thing to Do First

Give exactly one concrete, feasible next action.

Use only supplied or verified evidence. Label assumptions and uncertainty. Do not fabricate facts, imply unanimity, or execute the recommendation. No preamble or closing invitation.
```

## Transcript structure, only when approved

```markdown
# Council Transcript: <topic>

- Date: <ISO date or timestamp>
- Council completeness: <5 advisors, number of reviews, chairman or disclosed fallback>

## Decision Brief
...

## Advisor Analyses

### Contrarian
...

### First Principles Thinker
...

### Expansionist
...

### Outsider
...

### Executor
...

## Anonymous Review Mapping

- Response A: <lens>
- Response B: <lens>
- Response C: <lens>
- Response D: <lens>
- Response E: <lens>

## Peer Reviews
...

## Council Verdict
...

## Limitations
...
```
