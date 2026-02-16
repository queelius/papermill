---
name: outline
description: >-
  Use to design or refine the structure of a research paper. Generates a
  section-by-section outline with purpose, key arguments, and estimated length
  for each section. Adapts to the paper's format and venue conventions.
  Updates .papermill.md with the outline.
---

# Paper Outline

You are helping a researcher design the structure of their paper. A good outline is the skeleton that determines whether the paper's argument flows logically. Your goal is to produce a section-by-section plan that is specific to this paper, not a generic template.

## Step 1: Read Context

Read `.papermill.md` for:
- **Thesis**: The central claim and novelty (critical -- the outline serves the thesis).
- **Prior art**: Key references and gaps (these shape the related work section).
- **Format**: `latex`, `markdown`, or `rmarkdown` (affects section conventions).
- **Stage**: Current progress.
- **Venue**: Target venue (affects length, section naming, and emphasis).

If `.papermill.md` does not exist, suggest running `/papermill:init` first.

Also scan existing paper files. If there is already a partial draft, read it to understand what exists.

## Step 2: Determine the Paper Type

Different paper types have different conventional structures. Identify which applies:

- **Theoretical/mathematical**: Introduction, Preliminaries, Main Results, Proofs, Discussion, Conclusion
- **Empirical/experimental**: Introduction, Related Work, Method, Experiments, Results, Discussion, Conclusion
- **Systems/engineering**: Introduction, Background, Design, Implementation, Evaluation, Related Work, Conclusion
- **Survey/tutorial**: Introduction, Taxonomy/Framework, Section per topic, Discussion, Open Problems
- **Short paper/workshop**: Compressed -- Introduction, Approach, Results, Conclusion

Ask the user if you are unsure. The structure should match the contribution type.

## Step 3: Draft the Outline

For each section, specify:

| Field | Description |
|-------|-------------|
| **Section title** | The heading as it will appear in the paper |
| **Purpose** | What this section accomplishes for the reader (1 sentence) |
| **Key content** | Bullet points of what goes here (3-5 items) |
| **Estimated length** | Approximate page/paragraph count |
| **Dependencies** | What must be established before this section |

Example:

> ### 3. Preliminaries
> **Purpose**: Establish notation and recall the key definitions the reader needs.
> **Key content**:
> - Series system model (Definition 2.1)
> - Masked failure time observation model
> - Exponential lifetime assumption and its implications
> - Notation table for symbols used throughout
>
> **Estimated length**: 1.5 pages
> **Dependencies**: None (self-contained background)

## Step 4: Check the Narrative Arc

After drafting the outline, verify the story:

1. **Does the introduction motivate the problem clearly?** The reader should understand why this matters by the end of the first page.
2. **Does related work position the contribution?** It should make clear what gap this paper fills.
3. **Does the technical content flow logically?** Each section should build on the previous one.
4. **Does the paper deliver on the thesis?** The main results section should directly address the claim.
5. **Does the conclusion do more than summarize?** It should state implications and future work.

Raise any structural issues with the user. Common problems:
- Related work that reads like a list rather than a narrative
- Main results section that is too dense with no intuition
- Missing "so what?" in the conclusion
- Sections that could be merged or reordered for better flow

## Step 5: Present and Iterate

Present the complete outline to the user. Ask:

> Does this structure capture the argument you want to make? Would you reorder, merge, or split any sections?

Iterate until the user approves.

## Step 6: Update State File

Once approved, update `.papermill.md`:

- Set `stage` to `outlining` (or `drafting` if progressing).
- Append the outline to the markdown body under a `## Outline` heading.

Append a timestamped note documenting the outline creation.

## Step 7: Suggest Next Step

> The outline is set. Suggested next steps:
>
> - **Start writing**: Begin with the section you feel most confident about. Many authors start with the method/results, not the introduction.
> - **`/papermill:experiment`**: If experiments are part of the paper, design them now.
> - **`/papermill:prior-art`**: If the related work section needs more references, survey now.
