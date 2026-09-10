# Candidate Register

Use this file after all member submissions arrive. Do not treat `Under review` as acceptance into the final report.

## Candidate Issues

| ID | Candidate | Source member | Current-revision evidence | Concrete developer consequence | Maintainer context checked | Duplicate/root cause | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| I-A1 | Mobile development configuration may still have multiple sources of truth | Arthur Gao | Required | Required | Required | To assess | Under review |
| I-A2 | Delayed editor callbacks may lack a consistent stale-work policy | Arthur Gao | Required | Required beyond fixed #1087 | Required | To assess | Under review |
| I-A3 | Query failure recovery contracts may be distributed across layers | Arthur Gao | Required | Required beyond covered scalar rendering | Required | To assess | Under review |
| I-M2-1 | To be submitted | Member 2 |  |  |  |  | Awaiting submission |
| I-M3-1 | To be submitted | Member 3 |  |  |  |  | Awaiting submission |
| I-M4-1 | To be submitted | Member 4 |  |  |  |  | Awaiting submission |

## Candidate Positive Design Choices

| ID | Candidate | Source member | Specific decision | Repository evidence | Developer-level benefit | Status |
| --- | --- | --- | --- | --- | --- | --- |
| P-A1 | Focused renderer-contract regression tests | Arthur Gao | Recorded in Arthur's submission | Verified | Requires wider-project comparison | Under review |
| P-A2 | Opt-in development HTTPS preserves normal HTTP workflows | Arthur Gao | Recorded in Arthur's submission | Verified on merged revision | Android verified; iOS boundary stated | Under review |
| P-A3 | Root-cause guards are independently testable | Arthur Gao | Recorded in Arthur's submission | Verified on merged revision | Maintainer-approved example | Under review |
| P-M2-1 | To be submitted | Member 2 |  |  |  | Awaiting submission |
| P-M3-1 | To be submitted | Member 3 |  |  |  | Awaiting submission |
| P-M4-1 | To be submitted | Member 4 |  |  |  | Awaiting submission |

## Selection Guidance

Before accepting a candidate, confirm:

- it applies to the reviewed revision;
- it is supported by specific upstream repository links;
- it has a concrete developer consequence or benefit;
- it is not merely an isolated bug, style preference, feature request, or framework preference;
- issue-tracker, roadmap, design-document, and contributing-guide context has been checked;
- the recommendation is actionable and its impact, cost, and risk can be explained;
- it is not a duplicate symptom of another selected root cause.
