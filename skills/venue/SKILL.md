---
name: venue
description: >-
  Use to identify and evaluate publication venues for a research paper.
  Considers scope, impact factor, review timeline, acceptance rate, and
  fit with the paper's contribution. Produces a ranked shortlist with
  submission guidance.
---

# Venue Selection

You are helping a researcher identify the best publication venue for their paper. The right venue maximizes impact and minimizes wasted review cycles.

## Step 1: Read Context

Read `.papermill.md` for:
- **Thesis**: The core contribution (determines which communities care).
- **Prior art**: Where key references are published (reveals relevant venues).
- **Review history**: Paper quality assessment (determines venue tier).

Read the manuscript to understand scope, length, and contribution type.

## Step 2: Identify Candidate Venues

Use multiple signals to generate candidates:

### From references
Look at where the paper's key references are published. These venues are likely in scope.

### From the contribution type
- **Theoretical results**: Mathematics or statistics journals (Annals of Statistics, JASA, Bernoulli)
- **Algorithms**: CS conferences (NeurIPS, ICML, AAAI) or journals (JMLR, Algorithmica)
- **Applied methods**: Domain-specific journals (Biometrics, Technometrics, JRSS-B)
- **Systems**: Systems conferences (OSDI, SOSP) or journals (TOCS)
- **Short/preliminary results**: Workshop papers or letters

### From web search
Search for "best venues for [topic]" and check recent calls for papers.

## Step 3: Evaluate Each Venue

For each candidate, assess:

| Factor | What to check |
|--------|--------------|
| **Scope** | Does the paper's topic match the venue's published scope? |
| **Impact** | Impact factor, h-index, or community prestige |
| **Acceptance rate** | How selective is the venue? |
| **Review timeline** | Time from submission to decision (weeks to months) |
| **Open access** | Is it OA? Are there APC fees? |
| **Page limits** | Does the paper fit? |
| **Formatting** | What template/format is required? |

## Step 4: Present Recommendations

Present a ranked shortlist of 3-5 venues:

> **Venue Recommendations**
>
> | Rank | Venue | Fit | Impact | Timeline | Notes |
> |------|-------|-----|--------|----------|-------|
> | 1 | ... | High | ... | ... | ... |
> | 2 | ... | Good | ... | ... | ... |
> | 3 | ... | Good | ... | ... | ... |

For each recommended venue, explain:
- Why this venue is a good fit
- What aspects of the paper the reviewers will care about most
- Any adjustments needed for this venue (length, emphasis, formatting)

## Step 5: Submission Strategy

Discuss strategy with the user:

- **Top-down**: Submit to the best venue first, revise and resubmit if rejected.
- **Calibrated**: Target the venue where acceptance is most likely given the paper's current quality.
- **Dual-track**: Prepare a short version for a conference and a full version for a journal.

There is no universally correct strategy. Help the user think through the trade-offs.

## Step 6: Update State File

Update `.papermill.md`:

```yaml
venue:
  target: "Selected venue name"
  candidates:
    - name: "Venue 1"
      fit: "high"
      deadline: "YYYY-MM-DD or rolling"
    - name: "Venue 2"
      fit: "good"
      deadline: "YYYY-MM-DD or rolling"
```

Append a timestamped note documenting the venue analysis.

## Step 7: Suggest Next Steps

> Venue selected. Next steps:
>
> - **`/papermill:polish`**: Prepare the paper for submission to the target venue.
> - **Check formatting**: Download the venue's template and verify compliance.
> - **Write cover letter**: Some venues require a cover letter; draft one if needed.
