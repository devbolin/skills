# Evaluating skill output quality

> How to test whether your skill produces good outputs using eval-driven iteration.

## Designing test cases

A test case has three parts:

- **Prompt**: a realistic user message
- **Expected output**: a human-readable description of what success looks like
- **Input files** (optional): files the skill needs to work with

Store test cases in `evals/evals.json` inside your skill directory:

```json
{
  "skill_name": "csv-analyzer",
  "evals": [
    {
      "id": 1,
      "prompt": "I have a CSV of monthly sales data in data/sales_2025.csv. Can you find the top 3 months by revenue and make a bar chart?",
      "expected_output": "A bar chart image showing the top 3 months by revenue, with labeled axes and values.",
      "files": ["evals/files/sales_2025.csv"]
    }
  ]
}
```

Tips for writing good test prompts:

- Start with 2-3 test cases. Expand later.
- Vary the prompts in phrasing, detail, and formality.
- Cover edge cases.
- Use realistic context (file paths, column names, etc.).

## Running evals

Run each test case twice: once **with the skill** and once **without it** (or with a previous version). This gives you a baseline.

### Workspace structure

```
csv-analyzer/
├── SKILL.md
└── evals/
    └── evals.json
csv-analyzer-workspace/
└── iteration-1/
    ├── eval-top-months-chart/
    │   ├── with_skill/
    │   │   ├── outputs/
    │   │   ├── timing.json
    │   │   └── grading.json
    │   └── without_skill/
    │       ├── outputs/
    │       ├── timing.json
    │       └── grading.json
    └── benchmark.json
```

Each eval run should start with a clean context — no leftover state.

## Writing assertions

Good assertions:

- `"The output file is valid JSON"` — programmatically verifiable.
- `"The bar chart has labeled axes"` — specific and observable.
- `"The report includes at least 3 recommendations"` — countable.

Weak assertions:

- `"The output is good"` — too vague.
- `"The output uses exactly the phrase 'Total Revenue: $X'"` — too brittle.

## Grading outputs

Grade each assertion against actual outputs and record PASS or FAIL with evidence.

### Grading principles

- Require concrete evidence for a PASS.
- Review the assertions themselves while grading — fix ones that are too easy, too hard, or unverifiable.

## Aggregating results

Compute summary statistics per configuration and save to `benchmark.json`. The `delta` tells you what the skill costs (more time, more tokens) and what it buys (higher pass rate).

## Analyzing patterns

- Remove or replace assertions that always pass in both configurations.
- Investigate assertions that always fail in both configurations.
- Study assertions that pass with the skill but fail without.
- Tighten instructions when results are inconsistent across runs.
- Check time and token outliers.

## Reviewing results with a human

Record specific feedback for each test case. "The chart is missing axis labels" is actionable; "looks bad" is not.

## Iterating on the skill

After grading and reviewing, you have three sources of signal:

- **Failed assertions** point to specific gaps.
- **Human feedback** points to broader quality issues.
- **Execution transcripts** reveal *why* things went wrong.

### The loop

1. Give the eval signals and current `SKILL.md` to an LLM and ask it to propose improvements.
2. Review and apply the changes.
3. Rerun all test cases in a new `iteration-<N+1>/` directory.
4. Grade and aggregate.
5. Review with a human. Repeat until satisfied.
