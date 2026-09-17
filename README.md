# COMPX574 Logseq Code Review

Public collaboration workspace for the COMPX574 Project Code Review report on [Logseq](https://github.com/logseq/logseq).

## Assessment Details

- Opened: Monday, 7 September 2026, 8:00 AM
- Due: Friday, 25 September 2026, 5:00 PM
- Final submission: one PDF submitted through Moodle by one group member
- Expected report length: 3-6 pages of text, excluding front matter, headers, figures, and tables
- Report audience: developers who understand programming and are familiar with Logseq at a high level

## Assignment Brief

The original course instructions are preserved in [`assignment/COMPX574-Code-Review-Brief.pdf`](assignment/COMPX574-Code-Review-Brief.pdf). **Treat the PDF as the authoritative assessment brief.** This README and the repository templates are the group's working interpretation and collaboration plan.

---

## Situation Note: Direction Change, 17 September 2026

At the weekly meeting the supervisor gave additional direction for the next report. The group compared that direction against the PDF brief. The result is recorded in full in [`working/teacher-direction-vs-brief-comparison.md`](working/teacher-direction-vs-brief-comparison.md). Summary:

- The supervisor's direction **does not replace the brief and does not exceed it in content**. It is a **concretisation of one section only: Issues and Recommendations** (13 of the 25 marks).
- The supervisor said nothing about *Scope and Orientation*, *Positive Design Choices*, *Prioritization and Synthesis*, or the format and submission rules. **The PDF brief still governs all four of those areas in full.** Do not restructure the whole report around the supervisor's note.
- The supervisor **adds** two things: (a) impact on **users**, not only developers; (b) a **"realistic improvement or migration direction"**. Both must still respect the brief's instruction that each issue has **moderate scope**.
- The supervisor **narrows** one thing: he says "architectural or design decisions", while the brief says "structure, design, and **development practices**" and explicitly permits findings about required developer processes and tooling. His list is introduced with "useful directions **include**", so it is illustrative, not exclusive.
- The practical consequence: the report must move **from per-member, per-pull-request, module-local findings toward architecture-level findings** — integration layers, glue and conversion code, the coexistence of two architectures, duplicated compatibility layers, and decisions that make ordinary changes harder than expected.

**This is a correction of weighting, not of structure.** The four-section shape of the report, the counts (five issues, three positive design choices), and every format rule stay exactly as the PDF specifies.

---

## What This Repository Is For

This repository collects review evidence from four members and turns it into one coherent code-review report. It is not a place to combine four separate complete reports.

The final report must contain exactly:

1. One `Scope and Orientation` section.
2. Five codebase-level `Issues and Recommendations`.
3. Three `Positive Design Choices`.
4. One `Prioritization and Synthesis` section that ranks the five selected issues and names the hardest recommendation to land.

Individual pull requests and completed fixes are evidence of code familiarity. They do not automatically count as current codebase issues. If a contribution has already fixed a reported bug, the group must either:

- use it as evidence for a broader pattern that still exists on the reviewed revision;
- use the resulting design or test as a candidate positive design choice; or
- keep it only as scope and contribution evidence.

---

## Report Direction: Architecture-Level Findings

Every selected issue must be an **architectural or design decision that makes development, extension, maintenance, or onboarding harder** — not a bug list. Each issue must state: the current design, repository evidence, the impact on developers and users, and a realistic improvement or migration direction with its cost and risk.

The following are the directions the group is actively pursuing. They are drawn from the supervisor's guidance and from evidence the group already holds.

### The multi-repository map (required context for every issue)

Logseq is no longer one repository. Contributors must move between at least these, and issue numbers change when a report is transferred:

| Repository | Role |
| --- | --- |
| [`logseq/logseq`](https://github.com/logseq/logseq) | Main app; **the database (DB) line** |
| [`logseq/db-test`](https://github.com/logseq/db-test) | **The bug tracker for the DB version** — the main repo's README directs bug reports here |
| [`logseq/og`](https://github.com/logseq/og) | **The file-based Markdown line** ("Logseq OG") |
| [`logseq/datascript`](https://github.com/logseq/datascript), [`logseq/docs`](https://github.com/logseq/docs), [`logseq/publish-spa`](https://github.com/logseq/publish-spa), [`logseq/mldoc`](https://github.com/logseq/mldoc), [`logseq/marketplace`](https://github.com/logseq/marketplace), [`logseq/logseq-plugin-samples`](https://github.com/logseq/logseq-plugin-samples), [`logseq/logseq_journal`](https://github.com/logseq/logseq_journal) | Supporting libraries, docs, publishing, plugins |

Two precision points that the group must get right, because they are easy to state wrongly:

- **`db-test` is a repository for bug reports, not a database technology.** The DB version stores data in **SQLite** (via `sqlite-wasm`). The original Markdown version is file-based. Do not describe the change as "SQLite to DBTest".
- **Users are not distinguished by version numbers but by product identity and download path.** Since the 24 April 2026 announcement, the two products are "Logseq OG" (file-based) and "Logseq" (database). Confusion comes from naming and download path, not from a version string.

### Candidate architecture-level issue themes

| # | Theme | Why it is structural | Anchor evidence |
| --- | --- | --- | --- |
| A1 | **Dual-architecture burden: Markdown and DB lines** | One product carries two data models; features, fixes, and UX changes are considered twice. The maintainers' own April 2026 split announcement states this directly and is the strongest possible evidence. | [Split announcement](https://logseq.io/page/b2ad9ce1-9cb7-4436-8083-54cb4516d324/df4dc09d-0a12-4c87-904e-22a9bf4c350a), `logseq/og` vs `logseq/logseq` |
| A2 | **Cross-language integration contract drift** | The CLI is OCaml and the DB worker is ClojureScript; the payload shape between them is implicit, so a mismatch produced a silent empty export. There is no shared schema at the seam. | PR [#13200](https://github.com/logseq/logseq/pull/13200), `cli/lib/graph.ml`, `deps/db/src/logseq/db/sqlite/export.cljs` |
| A3 | **Silent failure instead of surfaced errors at boundaries** | Three separate experiences produced a *successful-looking* outcome with wrong or absent data: an empty export with exit code 0, a latched error boundary, and a value silently re-added. This is a **policy-to-practice gap**, not a missing policy: `AGENTS.md` already states "Prefer fail-fast over fallback" and "Do not silently recover from programmer errors", but nothing enforces it at these boundaries. | PR [#13200](https://github.com/logseq/logseq/pull/13200), PR [#13118](https://github.com/logseq/logseq/pull/13118), db-test [#1179](https://github.com/logseq/db-test/issues/1179), `AGENTS.md` section "Error handling and compatibility" |
| A4 | **Repository and issue-tracking fragmentation** | Issues are transferred between repositories, so numbers change and historical links break; a contributor must search several repositories before knowing where a bug belongs. This is onboarding and maintenance friction caused by structure. | #12951 → db-test #1089; #12979 → db-test #1087 |
| A5 | **Moving-target churn cost for contributors** | A contributor's production fix became obsolete because the upstream query pipeline was refactored while the work was in flight, forcing a test-only contribution. There is no stable internal contract or deprecation signal for the paths contributors depend on. | Local baseline `ab57092` vs upstream `3b9c0d0b92`; refactor `fb1047d1f8` |

### Candidate positive design choices

| # | Theme | The specific decision | Anchor evidence |
| --- | --- | --- | --- |
| P1 | **Contract-level tests that decode the real cross-language payload** | CLI parity tests assert the actual Transit payload sent to the DB worker, not just that a flag parses. This makes an otherwise invisible seam testable. | `cli/test/cli_parity_test_cases.ml`, PR [#13200](https://github.com/logseq/logseq/pull/13200) |
| P2 | **A written design policy that actually constrains implementation** | `AGENTS.md` section "Error handling and compatibility" states a deliberate failure-mode and compatibility policy (remove compatibility layers, prefer fail-fast over fallback, no new backward compatibility, no default values masking invalid state, do not silently recover from programmer errors, one clear code path). It changed a real implementation decision away from a compatibility shim. | `AGENTS.md`, PR [#13200](https://github.com/logseq/logseq/pull/13200) |
| P3 | **Opt-in, environment-controlled development configuration** | `LOGSEQ_SHADOW_HTTPS` enables HTTPS for mobile development while leaving the default HTTP workflow untouched — a switch, not a fork. | PR [#13121](https://github.com/logseq/logseq/pull/13121), `shadow-cljs.edn` |
| P4 | **State guards that are testable without the timing-sensitive symptom** | The editor refocus policy is expressed as explicit state conditions with focused tests, so a race can be verified deterministically. | PR [#13154](https://github.com/logseq/logseq/pull/13154), `src/test/frontend/handler/events_test.cljs` |

The group selects **three** of these for the final report. A1-A5 and P1-P4 are themes, not finished findings: each must be converted into a finding with a fixed commit, a locatable link, a stated consequence, and a moderate-scope recommendation.

---

## Team Roster

The following details were supplied for the course group. Verify every entry before exporting the final PDF.

| Member | Name | Student ID | University email | Final role |
| --- | --- | --- | --- | --- |
| Member 1 | Arthur Gao | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | Evidence baseline; A1-A5 and P1-P4 originate here |
| Member 2 | Josten Helsel | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |
| Member 3 | Danny Pham | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |
| Member 4 | Aung Min Myat | `[STUDENT ID]` | `[UNIVERSITY EMAIL]` | To be assigned |

## Privacy

This repository is public so that group members can fork it and submit their review material. Student IDs and university email addresses must remain as placeholders here and should be added only to the private final document prepared for Moodle. Do not commit credentials, personal identifiers, private graph data, access tokens, or unrelated personal information.

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
|   |-- teacher-direction-vs-brief-comparison.md
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
- relevant upstream issues, pull requests, commits, and permanent code links, **with the correct repository named**;
- **two or three candidate architecture-level issues**;
- one or two candidate positive design choices;
- concrete effects on development, testing, extension, or maintenance, **and on users where relevant**;
- a proposed recommendation with implementation cost and risk, expressed as a **staged, moderate first step** rather than a rewrite;
- checks against the upstream issue tracker, roadmap, design documents, and contributing guidance.

Do not submit a candidate merely because it is a style preference, an isolated bug, a feature request, or a proposal to rewrite the project in another technology.

### Evidence baseline

Every finding must name the repository and commit it was verified against. Logseq moves quickly: the group observed roughly 450 upstream commits landing during a single contribution, one of which refactored the very path under review. A finding without a fixed commit is not evidence.

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

## Internal Schedule

Revised on 17 September 2026 after the direction change. The original 14 September milestone has slipped and the remaining work is compressed.

| Date | Deliverable |
| --- | --- |
| 14 September (slipped) | All four member evidence submissions completed — only Member 1 has submitted |
| **18 September** | Members 2-4 confirm their reviewed areas and candidate themes; group agrees the five issue themes |
| **19 September** | Freeze the selected five issues and three positive design choices |
| **21 September** | Every selected finding has a fixed commit and a working link |
| **22 September** | First integrated report draft completed |
| **23 September** | Every member verifies statements about their reviewed areas; prioritization and hardest-to-land sections written |
| **24 September** | Final PDF quality check and Moodle draft upload |
| **25 September, before 5:00 PM** | One member completes the final Moodle submission |

## Integration Workflow

### Stage 1: Collect evidence

Each member submits their review material using the same template. Evidence should point primarily to the upstream Logseq repositories, not only to this collaboration repository. Prefer links fixed to a specific commit when citing code.

### Stage 2: Validate project context

For each candidate issue, check whether Logseq maintainers already discuss it in:

- the issue tracker (remember `logseq/db-test` for DB-version bugs);
- roadmap or project planning;
- design documentation, including the April 2026 product-split announcement;
- contributing documentation, including `AGENTS.md`;
- related pull requests and review comments.

Record the result in `working/candidate-register.md`.

### Stage 3: Consolidate and remove duplicates

Combine symptoms that share one structural cause. Do not use multiple report slots for different manifestations of the same underlying problem. In particular, do not spend two slots on A3-style silent failures that share one missing-contract root cause.

Evaluate candidates against:

- strength and precision of repository evidence;
- concrete developer impact, and user impact where relevant;
- structural or process significance;
- moderate and manageable scope, expressed as a staged first step;
- actionability of the recommendation;
- implementation cost and risk;
- relevance to the current reviewed revision, at a named commit;
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
- issue, PR, commit, and code links, including which repository each belongs to;
- technical descriptions of code behavior;
- whether the finding still applies to the reviewed revision;
- the final wording of limitations and unverified platforms;
- their name, student ID, and email address.

## How AI May Be Used

AI may help deduplicate candidates, find missing report fields, normalize writing style, summarize verified evidence, and control report length. It must not invent code behavior, repository links, maintainer positions, test results, or member contributions.

All AI-assisted text must be checked against the source repository by a group member. The group, not the AI, owns the final selection and ranking decisions.

## Definition of Done

- [ ] Four member submissions are complete.
- [ ] Every selected claim has a working upstream repository link naming the correct repository.
- [ ] Every selected claim names the commit or revision it was verified against.
- [ ] The report states what was and was not examined.
- [ ] Exactly five moderate-scope issues are selected, and each is an architecture-level or design finding rather than an isolated bug.
- [ ] Every issue explains current design, evidence, developer impact, user impact where relevant, recommendation, and cost and risk.
- [ ] Every recommendation is a staged first step, not a rewrite.
- [ ] Maintainer awareness and project context have been checked, including the April 2026 product split and the `AGENTS.md` design policy.
- [ ] Exactly three specific positive design choices are selected.
- [ ] The five issues are ranked without merely repeating their descriptions.
- [ ] The hardest recommendation to land is identified and justified.
- [ ] All four members have fact-checked the integrated draft.
- [ ] The PDF is 3-6 pages of text and contains the required first-page details.
- [ ] One member has uploaded and finally submitted the PDF through Moodle before the deadline.
