# COMPX574 Logseq Code Review

Public collaboration workspace for the COMPX574 Project Code Review report on [Logseq](https://github.com/logseq/logseq).

## Assessment Details

- Opened: Monday, 7 September 2026, 8:00 AM
- Due: Friday, 25 September 2026, 5:00 PM
- Final submission: one PDF submitted through Moodle by one group member
- Expected report length: 3-6 pages of text, excluding front matter, headers, figures, and tables
- Report audience: developers who understand programming and are familiar with Logseq at a high level

## Assignment Brief

The original course instructions are preserved in [`assignment/COMPX574-Code-Review-Brief.pdf`](assignment/COMPX574-Code-Review-Brief.pdf). Treat the PDF as the authoritative assessment brief. This README and the repository templates are the group's working interpretation and collaboration plan.

## Team Roster

The following details were supplied for the course group. Verify every entry before exporting the final PDF.

| Member | Name | Student ID | University email | Final role |
| --- | --- | --- | --- | --- |
| Member 1 | Arthur Gao | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |
| Member 2 | Josten Helsel | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |
| Member 3 | Danny Pham | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |
| Member 4 | Aung Min Myat | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |

## Privacy

This repository is public so that group members can fork it and submit their review material. Student IDs and university email addresses must remain as placeholders here and should be added only to the private final document prepared for Moodle. Do not commit credentials, personal identifiers, private graph data, access tokens, or unrelated personal information.

## What This Repository Is For

This repository collects review evidence from four members and turns it into one coherent code-review report. It is not a place to combine four separate complete reports.

The final report must contain exactly:

1. One `Scope and Orientation` section.
2. Five codebase-level `Issues and Recommendations`.
3. Three `Positive Design Choices`.
4. One `Prioritization and Synthesis` section that ranks the five selected issues.

Individual pull requests and completed fixes are evidence of code familiarity. They do not automatically count as current codebase issues. If a contribution has already fixed a reported bug, the group must either:

- use it as evidence for a broader pattern that still exists on the reviewed revision;
- use the resulting design or test as a candidate positive design choice; or
- keep it only as scope and contribution evidence.

## Repository Structure

```text
.
|-- README.md
|-- assignment/
|   `-- COMPX574-Code-Review-Brief.pdf
|-- templates/
|   `-- member-review-template.md
|-- submissions/
|   |-- member-1-arthur-gao.md
|   |-- member-2-josten-helsel.md
|   |-- member-3-danny-pham.md
|   `-- member-4-aung-min-myat.md
|-- working/
|   |-- candidate-register.md
|   `-- selection-decisions.md
`-- report/
    `-- final-report.md
```

## Member Submission Instructions

Each member should complete only their file in `submissions/`. Use `templates/member-review-template.md` as the canonical structure.

Each submission should include:

- the modules, configuration, documentation, tests, or workflows actually examined;
- areas not examined and the reason for excluding them;
- relevant upstream issues, pull requests, commits, and permanent code links;
- two or three candidate codebase issues;
- one or two candidate positive design choices;
- concrete effects on development, testing, extension, or maintenance;
- a proposed recommendation with implementation cost and risk;
- checks against the upstream issue tracker, roadmap, design documents, and contributing guidance.

Do not submit a candidate merely because it is a style preference, an isolated bug, a feature request, or a proposal to rewrite the project in another technology.

## Fork and Pull Request Workflow

Start from the [direct Fork page](https://github.com/hahaArthur17/compx574-logseq-code-review/fork).

1. Fork `hahaArthur17/compx574-logseq-code-review` into your own GitHub account.
2. In your fork, create a branch named `review/<your-name>`.
3. Update only your assigned file in `submissions/`, using the canonical template.
4. You may ask AI to organize and analyze your verified contribution evidence.
5. Personally check every AI-assisted claim, repository link, test result, and scope statement.
6. Commit and push the updated member file to your fork.
7. Open a pull request from your fork into `hahaArthur17/compx574-logseq-code-review:main`.
8. Ask at least one other member to review the evidence links and reasoning.
9. Resolve factual questions before the pull request is merged.

Suggested branch names:

- `review/arthur-gao`
- `review/josten-helsel`
- `review/danny-pham`
- `review/aung-min-myat`

Do not edit `report/final-report.md` during the evidence-collection stage unless the group has explicitly assigned an editor.

## Proposed Internal Schedule

These are working deadlines and may be adjusted by the group.

| Date | Deliverable |
| --- | --- |
| 14 September | All four member evidence submissions completed |
| 17 September | Cross-review of repository links, scope claims, and maintainer context completed |
| 19 September | Freeze the selected five issues and three positive design choices |
| 22 September | First integrated report draft completed |
| 23 September | Every member verifies statements about their reviewed areas |
| 24 September | Final PDF quality check and Moodle draft upload |
| 25 September, before 5:00 PM | One member completes the final Moodle submission |

## Integration Workflow

### Stage 1: Collect evidence

Each member submits their review material using the same template. Evidence should point primarily to the upstream Logseq repositories, not only to this collaboration repository.

Prefer links fixed to a specific commit when citing code. A moving `master` link can become inaccurate after the code changes.

### Stage 2: Validate project context

For each candidate issue, check whether Logseq maintainers already discuss it in:

- the issue tracker;
- roadmap or project planning;
- design documentation;
- contributing documentation;
- related pull requests and review comments.

Record the result in `working/candidate-register.md`.

### Stage 3: Consolidate and remove duplicates

Combine symptoms that share one structural cause. Do not use multiple report slots for different manifestations of the same underlying problem.

Evaluate candidates against:

- strength and precision of repository evidence;
- concrete developer impact;
- structural or process significance;
- moderate and manageable scope;
- actionability of the recommendation;
- implementation cost and risk;
- relevance to the current reviewed revision;
- overlap with other candidates.

### Stage 4: Select report content

The group should select exactly five issues and three positive design choices. Record accepted and rejected candidates, with short reasons, in `working/selection-decisions.md`.

### Stage 5: Prioritize together

Rank the five selected issues for one realistic release cycle. Consider impact, effort, risk, maintainer capacity, backward compatibility, contributor expectations, and dependencies between changes. Identify the recommendation that would be hardest to land in the Logseq community and explain why.

### Stage 6: Produce one report

One assigned editor may use AI to consolidate the selected material into `report/final-report.md`. The editor must preserve evidence, uncertainty, scope limitations, and the distinction between historical fixed issues and current codebase findings.

### Stage 7: Human verification

Before submission, every member must verify:

- claims about the areas they reviewed;
- issue, PR, commit, and code links;
- technical descriptions of code behavior;
- whether the finding still applies to the reviewed revision;
- the final wording of limitations and unverified platforms;
- their name, student ID, and email address.

## How AI May Be Used

AI may help deduplicate candidates, find missing report fields, normalize writing style, summarize verified evidence, and control report length. It must not invent code behavior, repository links, maintainer positions, test results, or member contributions.

All AI-assisted text must be checked against the source repository by a group member. The group, not the AI, owns the final selection and ranking decisions.

## Definition of Done

- [ ] Four member submissions are complete.
- [ ] Every selected claim has a working upstream repository link.
- [ ] The report states what was and was not examined.
- [ ] Exactly five moderate-scope issues are selected.
- [ ] Every issue explains consequence, recommendation, impact, cost, and risk.
- [ ] Maintainer awareness and project context have been checked.
- [ ] Exactly three specific positive design choices are selected.
- [ ] The five issues are ranked without merely repeating their descriptions.
- [ ] The hardest recommendation to land is identified and justified.
- [ ] All four members have fact-checked the integrated draft.
- [ ] The PDF is 3-6 pages of text and contains the required first-page details.
- [ ] One member has uploaded and finally submitted the PDF through Moodle before the deadline.
