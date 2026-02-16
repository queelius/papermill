---
name: experiment
description: >-
  Use to design experiments, simulations, or computational studies for a
  research paper. Covers hypothesis formulation, variable identification,
  methodology selection, and result interpretation. Produces a structured
  experiment plan with reproducibility in mind.
---

# Experiment Design

You are helping a researcher design rigorous experiments or computational studies. Good experiments are hypothesis-driven, reproducible, and have clear success criteria before they are run.

## Step 1: Read Context

Read `.papermill.md` for:
- **Thesis**: The claim the experiments should support or test.
- **Existing experiments**: Any previously registered experiments.
- **Format and tools**: What languages/tools are in the repo (R, Python, C++, etc.).

Scan the repository for existing code in `research/`, `code/`, `scripts/`, `experiments/`, or `analysis/` directories.

## Step 2: Identify What Needs Testing

Ask the user: "What specific claim or aspect of your thesis do these experiments need to support?"

Different contribution types need different experimental approaches:

| Contribution | Experimental approach |
|-------------|----------------------|
| Theorem/proof | Numerical validation of theoretical predictions |
| Algorithm | Runtime/accuracy benchmarks against baselines |
| Statistical method | Monte Carlo simulations with known ground truth |
| Empirical finding | Controlled experiments with statistical tests |
| Framework/model | Case studies demonstrating applicability |

## Step 3: Design the Experiment

For each experiment, specify:

### Hypothesis
State the expected outcome in falsifiable terms. "We expect X to be Y under conditions Z" -- not "we want to show our method works."

### Variables
- **Independent variables**: What you manipulate (parameters, dataset size, algorithm choice).
- **Dependent variables**: What you measure (accuracy, runtime, error rate).
- **Control variables**: What you hold constant (hardware, random seeds, data preprocessing).

### Methodology
- Data generation or collection procedure
- Algorithm/method configuration
- Number of replications or samples
- Statistical tests to apply (t-test, bootstrap CI, etc.)
- Baseline comparisons

### Success Criteria
Define **before running** what constitutes support for the hypothesis. This prevents post-hoc rationalization.

### Reproducibility
- Random seed strategy
- Hardware/software environment
- Data availability
- Script that runs the full experiment end-to-end

## Step 4: Address Common Pitfalls

Check for and warn about:

- **Cherry-picking**: Are you testing one configuration or sweeping parameters fairly?
- **Multiple comparisons**: If running many tests, apply correction (Bonferroni, FDR).
- **Overfitting to test data**: Is there a held-out validation set?
- **Computational budget**: Is this feasible given available hardware and time?
- **Missing baselines**: Every method needs comparison to something. Even "no method" is a baseline.

## Step 5: Register the Experiment

Update `.papermill.md` by adding to the `experiments` list:

```yaml
experiments:
  - name: "descriptive-name"
    type: "simulation | benchmark | case-study | ablation"
    hypothesis: "Expected outcome in one sentence"
    status: "planned | running | completed | failed"
    script: "path/to/script.R"
    last_run: null
```

Append a timestamped note documenting the experiment design.

## Step 6: Suggest Next Steps

> The experiment is designed. Next steps:
>
> - **Implement**: Write the experiment script. Consider `/papermill:simulation` if this is a Monte Carlo study.
> - **Run**: Execute the experiment and record results.
> - **Analyze**: Interpret results against the success criteria.
> - **`/papermill:review`**: Once results are written up, get feedback.
