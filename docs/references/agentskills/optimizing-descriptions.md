# Optimizing skill descriptions

> How to improve your skill's description so it triggers reliably on relevant prompts.

A skill only helps if it gets activated. The `description` field in your `SKILL.md` frontmatter is the primary mechanism agents use to decide whether to load a skill for a given task. An under-specified description means the skill won't trigger when it should; an over-broad description means it triggers when it shouldn't.

This guide covers how to systematically test and improve your skill's description for triggering accuracy.

## How skill triggering works

Agents use [progressive disclosure](specification.md#progressive-disclosure) to manage context. At startup, they load only the `name` and `description` of each available skill — just enough to decide when a skill might be relevant. When a user's task matches a description, the agent reads the full `SKILL.md` into context and follows its instructions.

This means the description carries the entire burden of triggering. If the description doesn't convey when the skill is useful, the agent won't know to reach for it.

One important nuance: agents typically only consult skills for tasks that require knowledge or capabilities beyond what they can handle alone. A simple, one-step request like "read this PDF" may not trigger a PDF skill even if the description matches perfectly, because the agent can handle it with basic tools. Tasks that involve specialized knowledge — an unfamiliar API, a domain-specific workflow, or an uncommon format — are where a well-written description can make the difference.

## Writing effective descriptions

Before testing, it helps to know what a good description looks like. A few principles:

- **Use imperative phrasing.** Frame the description as an instruction to the agent: "Use this skill when..." rather than "This skill does..." The agent is deciding whether to act, so tell it when to act.
- **Focus on user intent, not implementation.** Describe what the user is trying to achieve, not the skill's internal mechanics. The agent matches against what the user asked for.
- **Err on the side of being pushy.** Explicitly list contexts where the skill applies, including cases where the user doesn't name the domain directly: "even if they don't explicitly mention 'CSV' or 'analysis.'"
- **Keep it concise.** A few sentences to a short paragraph is usually right — long enough to cover the skill's scope, short enough that it doesn't bloat the agent's context across many skills. The [specification](specification.md#description-field) enforces a hard limit of 1024 characters.

## Designing trigger eval queries

To test triggering, you need a set of eval queries — realistic user prompts labeled with whether they should or shouldn't trigger your skill.

```json
[
  { "query": "I've got a spreadsheet in ~/data/q4_results.xlsx with revenue in col C and expenses in col D — can you add a profit margin column and highlight anything under 10%?", "should_trigger": true },
  { "query": "whats the quickest way to convert this json file to yaml", "should_trigger": false }
]
```

Aim for about 20 queries: 8-10 that should trigger and 8-10 that shouldn't.

### Should-trigger queries

These test whether the description captures the skill's scope. Vary them along several axes:

- **Phrasing**: some formal, some casual, some with typos or abbreviations.
- **Explicitness**: some name the skill's domain directly ("analyze this CSV"), others describe the need without naming it ("my boss wants a chart from this data file").
- **Detail**: mix terse prompts with context-heavy ones.
- **Complexity**: vary the number of steps and decision points.

### Should-not-trigger queries

The most valuable negative test cases are **near-misses** — queries that share keywords or concepts with your skill but actually need something different.

For a CSV analysis skill, strong negative examples:

- `"I need to update the formulas in my Excel budget spreadsheet"` — shares "spreadsheet" and "data" concepts, but needs Excel editing, not CSV analysis.
- `"can you write a python script that reads a csv and uploads each row to our postgres database"` — involves CSV, but the task is database ETL, not analysis.

### Tips for realism

Include file paths, personal context, specific details, casual language, abbreviations, and occasional typos.

## Testing whether a description triggers

Run each query through your agent with the skill installed and observe whether the agent invokes it. A query "passes" if:

- `should_trigger` is `true` and the skill was invoked, or
- `should_trigger` is `false` and the skill was not invoked.

### Running multiple times

Model behavior is nondeterministic — run each query multiple times (3 is a reasonable starting point) and compute a **trigger rate**: the fraction of runs where the skill was invoked.

## Avoiding overfitting with train/validation splits

If you optimize the description against all your queries, you risk overfitting.

- **Train set (~60%)**: the queries you use to identify failures and guide improvements.
- **Validation set (~40%)**: queries you set aside and only use to check whether improvements generalize.

## The optimization loop

1. **Evaluate** the current description on both *train and validation sets*.
2. **Identify failures** in the *train set*: which should-trigger queries didn't trigger? Which should-not-trigger queries did?
3. **Revise the description.** Focus on generalizing:
   - If should-trigger queries are failing, the description may be too narrow.
   - If should-not-trigger queries are false-triggering, the description may be too broad.
   - Avoid adding specific keywords from failed queries — that's overfitting.
   - If stuck, try a structurally different approach to the description.
4. **Repeat** steps 1-3 until all *train set* queries pass.
5. **Select the best iteration** by its validation pass rate.

## Applying the result

```yaml
# Before
description: Process CSV files.

# After
description: >
  Analyze CSV and tabular data files — compute summary statistics,
  add derived columns, generate charts, and clean messy data. Use this
  skill when the user has a CSV, TSV, or Excel file and wants to
  explore, transform, or visualize the data, even if they don't
  explicitly mention "CSV" or "analysis."
```
