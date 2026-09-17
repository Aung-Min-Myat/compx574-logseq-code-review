# Candidate Register

Use this file after all member submissions arrive. Do not treat `Under review` as acceptance into the final report.

> **Direction note (17 September 2026).** Candidates must be **architecture-level or design findings**, not isolated bugs, style preferences, feature requests, or rewrite proposals. See the README's "Report Direction" section. Each candidate must name a repository and a commit.
>
> **Timing note.** Member 1 is submitting candidates as a **pool**, not as a pre-selected set. Nothing below is accepted or rejected until all four member submissions are in and the group meets to freeze the five issues and three positive design choices. Do not resolve the merge questions in this file before that meeting.

## Candidate Issues

| ID | Candidate | Source member | Repository | Verified commit | Theme | Current-revision evidence | Developer consequence | User impact | Maintainer context checked | Duplicate / root cause | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| I-A-A | The OCaml CLI and the ClojureScript DB worker share an implicit, unvalidated data contract | Arthur Gao | `logseq/logseq` | `be800f17` / fixed `883331e` | Integration layer and glue code | [PR #13200](https://github.com/logseq/logseq/pull/13200), `cli/lib/graph.ml`, `deps/db/.../export.cljs` | Two languages, two directories, one unwritten agreement; nothing fails on mismatch | Silent empty exports consumed as success by automation | Maintainers accepted the CLI fix; `AGENTS.md` policy engaged | Overlaps I-A-B (same seam) | Under review |
| I-A-B | Integration boundaries default to silent failure rather than a surfaced error | Arthur Gao | `logseq/logseq`, `logseq/db-test` | `be800f17`, `ab57092`, `e963b91d` | Error handling and failure-mode strategy | PR #13200, PR #13118, db-test #1179; #13296 closed as duplicate of maintainer #13287; `AGENTS.md` "Error handling and compatibility" | Developers cannot trust success signals; manual data verification is mandatory | Highest user-visible risk: empty exports, latched error boundary, silently re-added values | **Written policy exists and is explicit** ("Do not silently recover from programmer errors", "Prefer fail-fast over fallback") but is not enforced at these boundaries; instances tracked separately in two repos | Shares the seam with I-A-A | Under review |
| I-A-C | Two coexisting architectures make ordinary changes cost more than expected | Arthur Gao | `logseq/logseq`, `logseq/og`, `logseq/db-test` | contributions `ab57092`-`e963b91d` | Module boundaries, coupling, duplicated compatibility layers | April 2026 split announcement; issue transfers #12951→#1089, #12979→#1087; query path refactor `fb1047d1f8` | Same change twice; internal paths rewritten under contributors; tracker location is an onboarding step | Users get different products by download path; support confusion | Maximally aware — the split is the maintainers' own decision | Absorbs the old I-A1 (mobile config drift) | Under review; must be sliced to moderate scope |
| I-M2-1 | To be submitted | Josten Helsel | | | | | | | | | Awaiting submission |
| I-M3-1 | To be submitted | Danny Pham | | | | | | | | | Awaiting submission |
| I-M4-1 | To be submitted | Aung Min Myat | | | | | | | | | Awaiting submission |

## Superseded Candidates

Recorded so the group can see the change of direction and reuse the evidence.

| Old ID | Candidate | New status | Reason |
| --- | --- | --- | --- |
| I-A1 | Mobile development configuration may still have multiple sources of truth | Superseded, evidence retained | Module-local. The cross-layer configuration drift is real but narrower than the architecture-level themes; its evidence now supports I-A-C. |
| I-A2 | Delayed editor callbacks may lack a consistent stale-work policy | Rejected as a report issue | Module-local and close to an isolated bug. Retained only as scope evidence and as a positive-choice candidate. |
| I-A3 | Query failure recovery contracts may be distributed across layers | Merged into I-A-B | The latched error boundary is one instance of the silent-failure pattern. Do not spend a separate slot on it. |

## Candidate Positive Design Choices

The group selects exactly **three**. All four below are drawn from different subsystems.

| ID | Candidate | Source member | Repository | Specific decision | Evidence | Developer-level benefit | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| P-A1 | Contract-level tests that decode the real cross-language payload | Arthur Gao | `logseq/logseq` | Parity tests assert the actual Transit payload sent to the DB worker, not just that a flag parses | [PR #13200](https://github.com/logseq/logseq/pull/13200), `cli/test/cli_parity_test_cases.ml` | Makes an invisible two-language seam directly testable | Under review |
| P-A2 | A written design policy that actually constrains implementation | Arthur Gao | `logseq/logseq` | `AGENTS.md` section "Error handling and compatibility" states a deliberate failure-mode and compatibility policy | `AGENTS.md`, [PR #13200](https://github.com/logseq/logseq/pull/13200) | Changed a real implementation decision away from a compatibility shim | Under review; wording now quoted verbatim in Member 1's submission |
| P-A3 | Opt-in, environment-controlled development configuration | Arthur Gao | `logseq/logseq` | `LOGSEQ_SHADOW_HTTPS` enables HTTPS for mobile without changing default HTTP workflows | [PR #13121](https://github.com/logseq/logseq/pull/13121), `shadow-cljs.edn` | A switch rather than a fork; no SSL forced on unrelated workflows | Under review; iOS unverified |
| P-A4 | State guards testable without the timing-sensitive symptom | Arthur Gao | `logseq/logseq` | The refocus policy is expressed as explicit state conditions with focused tests | [PR #13154](https://github.com/logseq/logseq/pull/13154), `events_test.cljs` | A race is verified deterministically instead of by reproducing typing speed | Under review |
| P-M2-1 | To be submitted | Josten Helsel | | | | | Awaiting submission |
| P-M3-1 | To be submitted | Danny Pham | | | | | Awaiting submission |
| P-M4-1 | To be submitted | Aung Min Myat | | | | | Awaiting submission |

## Selection Guidance

Before accepting a candidate, confirm:

- it applies to the reviewed revision, at a **named commit**;
- it is supported by specific upstream repository links, with the **correct repository** named;
- it is an **architectural or design finding**, not an isolated bug, style preference, feature request, or rewrite proposal;
- it has a concrete developer consequence, and a stated user impact where one exists;
- the recommendation is a **staged, moderate first step**, with cost and risk, rather than an end state;
- issue-tracker, roadmap, design-document, and contributing-guide context has been checked — including `logseq/db-test` and the April 2026 product-split announcement;
- it is not a duplicate symptom of another selected root cause — in particular, **do not select both I-A-A and I-A-B without deciding whether they share one root cause**.
