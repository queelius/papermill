---
name: init
description: >-
  This skill should be used when the user asks to "initialize a paper",
  "start a new paper project", "set up papermill", "onboard this repo",
  or needs to create the .papermill.md state file. Discovers repo structure,
  infers format (LaTeX, Markdown, R Markdown), gathers author info, and
  creates the initial state file. Idempotent -- safe to run on
  already-initialized repos.
---

# Papermill Init

Initialize a paper repository for use with papermill. Follow every step below in order. Be thorough in discovery but conservative in action -- never overwrite existing state.

---

## Step 1: Check for Existing State

Look for a file called `.papermill.md` in the repository root (Read tool).

- **If `.papermill.md` exists**: Read it and display a summary of its contents to the user (title, stage, format, authors). Do NOT overwrite or modify it. Tell the user:
  > This repository is already initialized. Current state is shown above. Use `/papermill:status` to see full details, or delete `.papermill.md` and re-run `/papermill:init` to start fresh.
- **If `.papermill.md` does not exist**: Continue to Step 2.

---

## Step 2: Discover Repository Structure

Search the repository to understand what kind of paper project this is (Glob tool). Look for all of the following and report what you find:

### Format detection (check in this order)

1. **LaTeX**: Search for `*.tex` files (Glob tool). If found, set `format: latex`.
2. **R Markdown**: Search for `*.Rmd` files (Glob tool). If found, set `format: rmarkdown`.
3. **Markdown**: Search for `paper.md`, `manuscript.md`, or any markdown file that looks like a JOSS-style paper (contains `title:` in YAML frontmatter) (Glob/Grep tools). If found, set `format: markdown`.
4. If none of the above are found, ask the user what format they plan to use. Default to `format: latex` if they are unsure.

### Additional assets

- `*.bib` files (bibliography)
- `research/` or `code/` or `scripts/` or `analysis/` directories (computational work)
- `CITATION.cff` (citation metadata)
- `figures/` or `img/` or `images/` directories (graphics)
- `Makefile`, `latexmkrc`, or build configuration files
- `README.md` or `CLAUDE.md` (project documentation)

Report a brief inventory of what was found, for example:

> **Discovered structure:**
> - Format: latex (found `paper/main.tex`)
> - Bibliography: `paper/references.bib` (12 entries)
> - Research code: `research/` directory present
> - Figures: `figures/` directory with 3 PDF files
> - Build system: `latexmk` configuration found

---

## Step 3: Get Author Information

Try to obtain the primary author's identity using the `deets` CLI tool, which manages personal metadata (Bash tool).

Run these commands (each may fail if deets is not installed -- that is fine):

```bash
deets get identity.name
deets get contact.email
deets get academic.orcid
```

- **If `deets` is available** and returns values: Use them as the primary author. Show the user what was found and ask for confirmation.
- **If `deets` is not available** or returns errors: Ask the user directly:
  > I could not find author information via `deets`. Please provide:
  > 1. Your full name (as it appears on papers)
  > 2. Your email address
  > 3. Your ORCID (optional, e.g., 0000-0002-1234-5678)

Also check for author info in existing files (Read/Grep/Bash tools):
- `\author{}` blocks in `.tex` files
- `CITATION.cff` author fields
- Git config (`git config user.name`, `git config user.email`)

If multiple sources conflict, prefer: deets > tex file > CITATION.cff > git config. Always confirm with the user.

---

## Step 4: Infer the Paper Title

Attempt to extract the title automatically (Grep/Read tools):

- **LaTeX**: Search for `\title{...}` in `.tex` files. Handle multiline titles and macro-containing titles.
- **Markdown/R Markdown**: Look for the first `# heading` or a `title:` field in YAML frontmatter.
- **CITATION.cff**: Check the `title:` field.

If a title is found, show it to the user and ask for confirmation. If no title is found, ask:

> I could not detect a paper title. What is the working title for this paper?

---

## Step 5: Infer the Current Stage

Determine the project stage based on what exists in the repository:

| Condition | Stage |
|-----------|-------|
| No paper content files exist (empty or new repo) | `idea` |
| Only outline/notes exist, minimal prose | `outlining` |
| Substantial paper content exists (multiple sections with prose) | `drafting` |
| Paper appears complete with abstract, conclusion, references | `review` |

Use your judgment. When in doubt, default to `drafting` if paper content exists, or `idea` if the repo is new or nearly empty. Tell the user what stage you inferred and why.

---

## Step 6: Create `.papermill.md`

Create the file `.papermill.md` in the repository root with the following structure (Write tool). Fill in all values gathered from the previous steps. Use the exact YAML schema shown below.

```markdown
---
title: "<title from Step 4>"
stage: <stage from Step 5>
format: <format from Step 2>
authors:
  - name: "<name from Step 3>"
    email: "<email from Step 3>"
    orcid: "<orcid from Step 3, or empty string if not available>"

thesis:
  claim: ""
  novelty: ""
  refined: null

prior_art:
  last_survey: null
  key_references: []
  gaps: ""

experiments: []

venue:
  target: null
  candidates: []

review_history: []
---

## Notes

Initialized by papermill on <today's date in YYYY-MM-DD format>.
```

**Important schema notes:**
- `stage` must be one of: `idea`, `thesis`, `literature`, `outlining`, `drafting`, `review`, `submission`
- `format` must be one of: `latex`, `markdown`, `rmarkdown`
- `orcid` should be the bare identifier (e.g., `0000-0002-1234-5678`), not a URL
- `thesis.refined` starts as `null` and is populated by the thesis skill
- `prior_art.last_survey` is a date string (`YYYY-MM-DD`) or `null`
- `review_history` is a list of objects added by the review skill
- Leave empty fields as empty strings `""`, not `null`, unless the schema above specifies `null`

---

## Step 7: Report and Suggest Next Steps

After creating `.papermill.md`, display a summary:

> **Papermill initialized.**
>
> | Field   | Value              |
> |---------|--------------------|
> | Title   | <title>            |
> | Stage   | <stage>            |
> | Format  | <format>           |
> | Author  | <name> (<email>)   |
>
> State file: `.papermill.md`

Then suggest the next skill based on the inferred stage:

| Stage | Suggestion |
|-------|------------|
| `idea` | "Run `/papermill:thesis` to develop your core claim and novelty statement." |
| `thesis` | "Run `/papermill:thesis` to refine your thesis, or `/papermill:prior-art` to survey related work." |
| `literature` | "Run `/papermill:prior-art` to survey related work and identify gaps." |
| `outlining` | "Run `/papermill:outline` to structure your paper sections." |
| `drafting` | "Run `/papermill:thesis` to articulate your core claim, or `/papermill:status` to review your progress." |
| `review` | "Run `/papermill:review` to get feedback on your draft." |
| `submission` | "Run `/papermill:status` to review submission readiness." |

---

## Error Handling

- If the repository root cannot be determined, ask the user which directory to use.
- If file creation fails due to permissions, report the error clearly.
- Never silently skip steps. If any inference fails, ask the user explicitly.
- If the user interrupts or provides corrections during any step, incorporate them before proceeding.
