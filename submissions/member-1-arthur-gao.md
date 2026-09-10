# Member 1 Review Submission: Arthur Gao

## 1. Reviewer Information

- Member number: Member 1
- Full name: Arthur Gao
- Student ID: `[STUDENT ID]`
- University email: `[UNIVERSITY EMAIL]`
- GitHub username: [hahaArthur17](https://github.com/hahaArthur17)
- Submission status: Evidence collected; candidate report interpretations require group selection

## 2. Contribution and Familiarity Evidence

Arthur completed three upstream contributions to `logseq/logseq`. All three pull requests were merged into `master`. These records establish direct familiarity with query rendering tests, mobile development configuration, editor state transitions, regression testing, and maintainer review.

| Original/current issue | Pull request | Final status | Contribution |
| --- | --- | --- | --- |
| Original [logseq/logseq#12951](https://github.com/logseq/logseq/issues/12951), transferred to [logseq/db-test#1089](https://github.com/logseq/db-test/issues/1089) | [logseq/logseq#13118](https://github.com/logseq/logseq/pull/13118) | Merged on 26 August 2026 as [`835c031`](https://github.com/logseq/logseq/commit/835c031d69d1771baa2f11bbb303c0c6e952de09) | Added regression coverage for scalar custom-query rendering without changing production behavior |
| [logseq/logseq#13010](https://github.com/logseq/logseq/issues/13010) | [logseq/logseq#13121](https://github.com/logseq/logseq/pull/13121) | Merged on 31 August 2026 as [`de2ecab`](https://github.com/logseq/logseq/commit/de2ecabf6d6891a41c104d7c4041797bcaa54b3e) | Repaired the opt-in HTTPS mobile development server, document-root resolution, and Android development documentation |
| Original `logseq/logseq#12979`, transferred to [logseq/db-test#1087](https://github.com/logseq/db-test/issues/1087) | [logseq/logseq#13154](https://github.com/logseq/logseq/pull/13154) | Merged on 1 September 2026 as [`4231bf3`](https://github.com/logseq/logseq/commit/4231bf309172cae619d65cec10eae98e8706b704) | Fixed a stale delayed refocus race after code-block conversion and added root-cause regression tests |

### Maintainer feedback

For PR #13154, Logseq maintainer `tiensonqin` submitted an `APPROVED` review and wrote: ["Works great, thanks for the fix! \ud83d\udea2 \ud83d\udc4d"](https://github.com/logseq/logseq/pull/13154#pullrequestreview-5077197335). The pull request was merged 45 seconds after that review. This is direct evidence that the contribution was accepted by the upstream project, but it is not by itself proof that the underlying change should become one of the report's three positive design choices.

## 3. Review Scope

### Areas examined closely

| Area | Files/modules/workflows | Evidence | Depth of review |
| --- | --- | --- | --- |
| Custom-query rendering contract | `src/main/frontend/components/query.cljs`, query result handling, `src/test/frontend/components/query_test.cljs` | [PR #13118](https://github.com/logseq/logseq/pull/13118) | Traced scalar-result behavior, verified the current renderer contract, added a focused regression test, and ran targeted plus full lint/test commands |
| Mobile development server and Android workflow | `shadow-cljs.edn`, `scripts/src/logseq/tasks/dev/mobile.clj`, `docs/develop-logseq-on-mobile.md`, Capacitor-generated development URL | [Issue #13010](https://github.com/logseq/logseq/issues/13010), [PR #13121](https://github.com/logseq/logseq/pull/13121) | Reproduced TLS and path failures, traced generated URLs and runtime module paths, updated configuration/documentation, built an APK, and cold-started it in a Pixel 9 emulator |
| Editor conversion and focus state | `src/main/frontend/handler/events.cljs`, `src/test/frontend/handler/events_test.cljs`, relevant editor state and pending-new-block behavior | [db-test issue #1087](https://github.com/logseq/db-test/issues/1087), [PR #13154](https://github.com/logseq/logseq/pull/13154) | Reproduced the macOS symptom, identified the stale 100 ms refocus callback, implemented a guarded refocus, added root-cause assertions, and manually verified persistence and visibility |

### Whole-system material examined

- The interaction between frontend components, editor events, transient UI state, DB-backed block creation, and rendered output.
- Shadow CLJS development server configuration and the Babashka mobile-development task.
- The relationship between development documentation, environment variables, generated Capacitor configuration, runtime asset paths, Android build tooling, and emulator behavior.
- The upstream contribution process: issue research, scope control, focused commits, pull-request descriptions, CI/CLA checks, review, and merge.

### Areas not examined deeply

| Area | Reason it was not examined |
| --- | --- |
| iOS device or simulator behavior for the mobile HTTPS change | The shared configuration suggests the change also benefits iOS, but manual verification was limited to Android |
| RTC synchronization internals | None of the three contributions required a detailed review of the RTC implementation |
| Plugin API compatibility and marketplace behavior | Outside the implementation paths exercised by these contributions |
| Release packaging across all operating systems | Android debug packaging was tested; a full macOS, Windows, Linux, iOS, and production-release audit was not performed |
| The entire frontend architecture | Review depth was concentrated on queries, editor state transitions, and mobile development configuration |

## 4. Completed Contribution Analysis

The following items document Arthur's work and possible report relevance. Because the upstream defects are already resolved, they must not be copied into the final report as if they were current unresolved issues.

### Contribution A: Scalar custom-query rendering regression coverage

#### Context

The original issue #12951, now `logseq/db-test#1089`, described a query error that could remain visible until page reload. Arthur traced the earlier scalar-result failure path. On the then-current `master`, an upstream query refactor already preserved scalar tuples and routed non-UUID values through a generic list renderer, so the old production patch was no longer appropriate.

#### Accepted change

PR #13118 added an 11-line regression test in [`src/test/frontend/components/query_test.cljs`](https://github.com/logseq/logseq/blob/835c031d69d1771baa2f11bbb303c0c6e952de09/src/test/frontend/components/query_test.cljs). The test records the scalar-rendering contract without making a production change.

#### Developer significance

The contribution shows a useful maintenance practice: when the production behavior has already changed upstream, a contributor can avoid reintroducing an obsolete fix and instead preserve the recovered behavior with a focused regression test.

#### Possible report use

- Strong scope evidence for query rendering and regression-test review.
- Possible positive-design candidate if the group can show that the query test structure makes renderer contracts easy to isolate and protect.
- Not a current issue unless the group finds current evidence of a broader error-recovery or result-shape weakness.

### Contribution B: Mobile HTTPS development server alignment

#### Context

Issue #13010 showed that `bb dev:android-app` generated certificates and an HTTPS Capacitor URL while the Shadow CLJS server still used HTTP. The configured `/mobile/` URL also conflicted with the document root. A simple change from `/mobile/` to `/` was insufficient because generated runtime modules remained under `/static/mobile/js`.

#### Accepted change

PR #13121 changed:

- [`shadow-cljs.edn`](https://github.com/logseq/logseq/blob/de2ecabf6d6891a41c104d7c4041797bcaa54b3e/shadow-cljs.edn) to restore environment-controlled SSL and serve both the required entry and runtime paths;
- [`docs/develop-logseq-on-mobile.md`](https://github.com/logseq/logseq/blob/de2ecabf6d6891a41c104d7c4041797bcaa54b3e/docs/develop-logseq-on-mobile.md) so the manual Android workflow uses the same HTTPS behavior.

The change was verified through configuration checks, the mobile build, HTTPS path requests, Android APK assembly, and a Pixel 9 emulator cold start. iOS was not manually verified.

#### Developer significance

This contribution spans configuration, generated development URLs, asset routing, documentation, build tooling, and a real emulator. It provides direct evidence of how inconsistency across development-tooling layers can prevent contributors from starting the application even when each individual setting appears plausible.

#### Possible report use

- Strong scope evidence for build/development tooling and configuration handling.
- Possible positive-design candidate for the current opt-in HTTPS workflow if the group verifies that it remains clear and reliable on the reviewed revision.
- Possible starting evidence for a broader current issue about duplicated mobile-development configuration, but only if an audit finds remaining drift risks or inconsistencies. The already-fixed TLS/path bug cannot be presented as a current issue.

### Contribution C: Guarded editor refocus after code-block conversion

#### Context

After a user typed four backticks and pressed Enter, later block text could become invisible. The underlying text could remain present and reappear after reload. Arthur identified a delayed `edit-block!` callback scheduled by `:editor/upsert-type-block`: the callback could reclaim focus after the user had already created and started editing a new block.

#### Accepted change

PR #13154 changed:

- [`src/main/frontend/handler/events.cljs`](https://github.com/logseq/logseq/blob/4231bf309172cae619d65cec10eae98e8706b704/src/main/frontend/handler/events.cljs) to allow automatic refocus only when no new block is pending and the original source block remains active;
- [`src/test/frontend/handler/events_test.cljs`](https://github.com/logseq/logseq/blob/4231bf309172cae619d65cec10eae98e8706b704/src/test/frontend/handler/events_test.cljs) to cover active conversion, a pending new block, a user who has moved to another block, and explicit type insertion.

The targeted tests, related editor tests, lint, manual macOS reproduction, post-fix entry of three subsequent blocks, reload, and persistence checks all passed. The dead-key/composition behavior reported for international keyboard layouts remained outside this fix and is tracked separately.

#### Developer significance

The change demonstrates the importance of validating delayed UI work against current state. It also shows the value of testing the state-transition conditions behind a race instead of testing only the visible symptom.

#### Possible report use

- Strong scope evidence for editor event flow, asynchronous state handoff, focused regression testing, and macOS browser verification.
- Strong candidate positive design choice if framed around the current state guard and root-cause-level testability rather than around the praise received.
- Possible starting evidence for a broader current issue involving delayed callbacks and editor state ownership, but only after searching the current codebase for comparable patterns and demonstrating a remaining consequence.

## 5. Candidate Issues Requiring Current-Master Validation

These are research directions, not ready-made report claims.

### Candidate Issue A: Mobile development configuration may still have multiple sources of truth

#### Current status

- Requires more evidence.
- The specific TLS and `/mobile/` mismatch from issue #13010 has been fixed.

#### Research question

Do Shadow CLJS configuration, Babashka tasks, generated Capacitor settings, documentation, and platform-specific workflows still repeat values or assumptions in ways that can drift?

#### Required evidence before selection

- Map every current source of protocol, host, port, path, certificate, and environment-variable behavior.
- Identify at least one remaining maintenance consequence or credible change path that requires synchronized edits.
- Check the issue tracker and contributor documentation for the maintainers' current position.
- Propose a moderate change, such as generating documentation/configuration from a shared source or adding cross-layer validation, and assess its cost.

### Candidate Issue B: Delayed editor callbacks may lack a consistent stale-work policy

#### Current status

- Requires more evidence.
- The specific stale refocus callback behind db-test #1087 has been fixed.

#### Research question

Are other delayed editor callbacks able to act on stale block identity, pending-new-block state, selection, or editing context?

#### Required evidence before selection

- Search the current editor/event code for timers, queued callbacks, and asynchronous continuations that mutate focus or edit state.
- Compare their guards and cancellation behavior.
- Demonstrate a concrete testing or maintenance consequence beyond the already-fixed #1087 symptom.
- Avoid claiming a systemic pattern from one historical bug alone.

### Candidate Issue C: Query failure recovery contracts may be distributed across renderer and error-boundary layers

#### Current status

- Requires more evidence.
- Scalar result rendering currently has regression coverage from PR #13118.

#### Research question

Can developers reliably understand and test how parse errors, execution errors, result-shape variation, component error boundaries, and later successful reruns clear failure state?

#### Required evidence before selection

- Trace the current query pipeline and error ownership on a fixed commit.
- Inventory existing tests for fail-then-recover transitions, not only successful scalar rendering.
- Identify a concrete current gap and its developer consequence.
- Check related upstream issues before describing the gap as an oversight.

## 6. Candidate Positive Design Choices

### Positive Choice A: Focused renderer-contract regression tests

- Specific design decision: The frontend query tests can exercise a custom-query result shape directly and confirm the renderer's generic-list contract.
- Evidence: [PR #13118](https://github.com/logseq/logseq/pull/13118) and the merged [`query_test.cljs`](https://github.com/logseq/logseq/blob/835c031d69d1771baa2f11bbb303c0c6e952de09/src/test/frontend/components/query_test.cljs).
- Developer-level benefit: A contributor can protect a previously failing result shape with a small test instead of modifying production code after upstream behavior has already changed.
- What would otherwise be difficult: Reproducing the complete advanced-query UI path and diagnosing whether a regression belongs to query execution, tuple preservation, or rendering.
- Limitation: The group must inspect the wider query test design before generalizing from one test.

### Positive Choice B: Opt-in development HTTPS preserves normal HTTP workflows

- Specific design decision: The mobile development server enables SSL through `LOGSEQ_SHADOW_HTTPS`, leaving default HTTP workflows unchanged while supporting Capacitor's HTTPS development URL.
- Evidence: [PR #13121](https://github.com/logseq/logseq/pull/13121), [`shadow-cljs.edn`](https://github.com/logseq/logseq/blob/de2ecabf6d6891a41c104d7c4041797bcaa54b3e/shadow-cljs.edn), and the aligned [mobile development documentation](https://github.com/logseq/logseq/blob/de2ecabf6d6891a41c104d7c4041797bcaa54b3e/docs/develop-logseq-on-mobile.md).
- Developer-level benefit: Mobile contributors can use a secure WebView-compatible server without forcing SSL and certificate setup onto unrelated development workflows.
- What would otherwise be difficult: Maintaining both browser-friendly HTTP development and device/WebView-compatible HTTPS development without separate server implementations.
- Limitation: Manual verification covered Android, not iOS.

### Positive Choice C: Root-cause guards are testable independently of a timing-sensitive UI symptom

- Specific design decision: The refocus rule after type-block conversion is expressed as explicit state conditions and protected by focused event-handler tests.
- Evidence: [PR #13154](https://github.com/logseq/logseq/pull/13154), [`events.cljs`](https://github.com/logseq/logseq/blob/4231bf309172cae619d65cec10eae98e8706b704/src/main/frontend/handler/events.cljs), and [`events_test.cljs`](https://github.com/logseq/logseq/blob/4231bf309172cae619d65cec10eae98e8706b704/src/test/frontend/handler/events_test.cljs).
- Developer-level benefit: Maintainers can verify the intended state-transition policy without relying only on a nondeterministic end-to-end reproduction.
- What would otherwise be difficult: Reliably reproducing a race whose visible failure depends on typing speed and delayed focus timing.
- Evidence from experience: The implementation passed targeted event tests and related editor tests, then behaved correctly in the original macOS workflow. The maintainer approved and merged it without a change request.
- Limitation: This claim concerns the refocus path; it does not establish that all editor races are handled consistently.

## 7. Suggested Priority

No priority is assigned yet because the three possible current-issue directions require a current-master audit. If they produce valid findings, Arthur recommends considering developer-onboarding blockers and data-entry/editor reliability ahead of narrow regression-test completeness, subject to impact, scope, and maintainer constraints.

## 8. Questions for the Group

- Which reviewed commit or date will the final report use as its evidence baseline?
- Does another member have evidence about mobile development configuration, editor async state, or query failure recovery that can confirm or reject Arthur's research directions?
- Which of the three positive candidates remains significant when compared with findings from the other members?
- Should the final report mention upstream contribution history only in the scope section, or also use one contribution as an experiential example inside a selected finding?

## 9. Member Verification

- [x] GitHub issue, pull-request, commit, file, and maintainer-review links were checked when this submission was prepared.
- [x] Completed upstream fixes are distinguished from current candidate issues.
- [x] Unverified areas and the Android-only platform boundary are stated.
- [x] Maintainer praise is treated as contribution evidence rather than automatic proof of a positive design choice.
- [ ] Current-master research questions have been audited and converted into report-ready findings or rejected.
- [ ] Arthur has completed a final review after the integrated report is drafted.
