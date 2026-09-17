# COMPX574 Code Review Report: Logseq

> Final report placeholder. Do not draft this file until the group has completed evidence collection and recorded the selected five issues and three positive design choices in `working/selection-decisions.md`.

> **Direction note (17 September 2026).** This report is a critical evaluation of Logseq's architecture and design decisions, not a list of bug fixes, issues, or pull requests. Every selected issue must state its current design, repository evidence at a named commit, its impact on developers and users, and a realistic improvement or migration direction expressed as a staged, moderate first step. See the README's "Report Direction" section and `working/teacher-direction-vs-brief-comparison.md`.

## Scope and Orientation

To be written after group selection. Must cover: how Logseq is architected at a high level, **including the split between the file-based Markdown line (`logseq/og`) and the database line (`logseq/logseq`)**, and the role of the separate `logseq/db-test` bug tracker; which parts of the codebase the group worked in; which parts the review covers; and explicitly what was **not** examined, and why. State the reviewed revision.

## Issues and Recommendations

To contain exactly five selected issues. Each issue must present, in order:

1. **Current design** — the architectural or design decision as it exists now.
2. **Evidence** — repository links pinned to a commit, plus contribution experience.
3. **Impact on developers** — the concrete consequence for developing, testing, extending, or maintaining.
4. **Impact on users** — where a user-visible consequence exists.
5. **Recommendation** — a staged, moderate first step, not an end state.
6. **Cost and risk** — implementation effort, compatibility or migration risk, testing requirements, disadvantages.
7. **Maintainer awareness and position** — what the project already knows and how the group engages with it.

## Positive Design Choices

To contain exactly three selected design choices. Each must identify a specific decision and explain what it made easy that would otherwise have been difficult, with developer-level reasoning and a commit-pinned link. Avoid the obvious: "it has tests" or "it uses a widely adopted framework" is an observation, not an analysis.

## Prioritization and Synthesis

To rank the five selected issues and identify the recommendation that would be hardest to land. This section is marked on judgement, not coverage, and must not restate the issue descriptions. Weigh impact against cost, risk, and the practical realities of an open-source project — maintainer capacity, backward compatibility, contributor expectations — and recognise the social and process constraints on change, not only the technical ones.

Note the specific tension this report must address: the maintainers are currently **reducing** the project's surface area by splitting it into two products, so any recommendation that adds a new obligation or compatibility requirement must explain why it does not work against that direction.
