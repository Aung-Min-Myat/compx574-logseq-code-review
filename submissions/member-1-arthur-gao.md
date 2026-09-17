# Member 1 Review Submission: Arthur Gao

## 1. Reviewer Information

- Member number: Member 1
- Full name: Arthur Gao
- Student ID: `[STUDENT ID]`
- University email: `[UNIVERSITY EMAIL]`
- GitHub username: [hahaArthur17](https://github.com/hahaArthur17)
- Submission status: Evidence collected; candidates are architecture-level and ready for group selection

## 2. Contribution and Familiarity Evidence

Five upstream contributions to the Logseq organisation. Four were merged; the fifth is a verified local fix with no pull request. Together they establish direct familiarity with the CLI/DB-worker seam, the DB query pipeline, editor async state, mobile development tooling, and the DB property/class layer.

| Type | Repository | Reference | Status | Work | Areas learned |
| --- | --- | --- | --- | --- | --- |
| PR | `logseq/logseq` | [#13200](https://github.com/logseq/logseq/pull/13200) (issue [logseq/logseq#13071](https://github.com/logseq/logseq/issues/13071)) | **Merged** 15 Sep 2026 as [`883331e`](https://github.com/logseq/logseq/commit/883331ee0113899df7b4cf93cacc2e0fb9c25bcf) | Fixed `graph export --edn-options` silently nesting all keys under `:graph-options`; corrected the OCaml CLI payload, added payload-level and real CLI E2E regression tests, updated help text and docs | Cross-language data contract between OCaml CLI and ClojureScript DB worker; CLI E2E harness; Transit payload testing |
| PR | `logseq/logseq` | [#13154](https://github.com/logseq/logseq/pull/13154) (issue [logseq/db-test#1087](https://github.com/logseq/db-test/issues/1087), formerly `logseq/logseq#12979`) | **Merged** 1 Sep 2026 as [`4231bf3`](https://github.com/logseq/logseq/commit/4231bf309172cae619d65cec10eae98e8706b704) | Fixed a stale delayed refocus race after code-block conversion; added root-cause state-condition tests | Editor async state, delayed callbacks, focus ownership, race-condition testing |
| PR | `logseq/logseq` | [#13121](https://github.com/logseq/logseq/pull/13121) (issue [logseq/logseq#13010](https://github.com/logseq/logseq/issues/13010)) | **Merged** 31 Aug 2026 as [`de2ecab`](https://github.com/logseq/logseq/commit/de2ecabf6d6891a41c104d7c4041797bcaa54b3e) | Repaired the opt-in HTTPS mobile development server, document-root resolution, and Android development documentation | Shadow CLJS build configuration, Capacitor/WebView dev URLs, cross-layer configuration drift |
| PR | `logseq/logseq` | [#13118](https://github.com/logseq/logseq/pull/13118) (issue [logseq/db-test#1089](https://github.com/logseq/db-test/issues/1089), formerly `logseq/logseq#12951`) | **Merged** 26 Aug 2026 as [`835c031`](https://github.com/logseq/logseq/commit/835c031d69d1771baa2f11bbb303c0c6e952de09) | Added regression coverage for scalar custom-query rendering after upstream had already refactored the production path | Query result shapes, renderer contracts, test-only contribution strategy |
| Local commit | `logseq/logseq` | `c2600c83bf` (issue [logseq/db-test#1179](https://github.com/logseq/db-test/issues/1179)) | **No PR** — fix implemented and verified | Removed `Root Tag` from the `Extends` candidate list and prevented it being re-added via "New option"; updated an E2E helper that depended on the old workaround | DB class/property layer, property picker state, worker-side option generation, E2E helper coupling |

### Maintainer feedback

For PR #13154, maintainer `tiensonqin` submitted an `APPROVED` review and wrote ["Works great, thanks for the fix!"](https://github.com/logseq/logseq/pull/13154#pullrequestreview-5077197335), and the PR was merged shortly after. This is evidence that the contribution was accepted upstream, but it is not by itself proof that the change should become one of the report's positive design choices.

### What the contribution record itself demonstrates

Across five contributions, the same structural pattern kept appearing: **a boundary between two layers where the contract is implicit**, and where a mismatch produces a *successful-looking* result rather than an error. That observation, not the individual fixes, is the basis of the candidates below.

## 3. Review Scope

### Areas examined closely

| Area | Repository | Files/modules/workflows | Evidence | Depth of review |
| --- | --- | --- | --- | --- |
| CLI to DB-worker export contract | `logseq/logseq` | `cli/lib/graph.ml`, `cli/lib/command_registry.ml`, `cli/lib/example.ml`, `deps/db/src/logseq/db/sqlite/export.cljs`, `cli/test/cli_parity_test_cases.ml`, `cli-e2e/spec/non_sync_cases.edn`, `docs/cli/logseq-cli.md` | [PR #13200](https://github.com/logseq/logseq/pull/13200) | Traced the full payload path, reproduced a silent empty export, corrected the adapter, added payload-level and end-to-end tests, updated the public contract documentation |
| DB query result pipeline | `logseq/logseq` | `src/main/frontend/components/query.cljs`, `src/main/frontend/components/query/result.cljs`, `src/main/frontend/db/query_custom.cljs`, `src/main/frontend/db/query_react.cljs`, `deps/db/src/logseq/db/common/view.cljs` | [PR #13118](https://github.com/logseq/logseq/pull/13118) | Reproduced, traced two successive `is not ISeqable` failure paths, then discovered upstream had refactored the whole path |
| Editor async state and focus ownership | `logseq/logseq` | `src/main/frontend/handler/events.cljs`, `src/main/frontend/handler/editor.cljs`, `src/test/frontend/handler/events_test.cljs` | [PR #13154](https://github.com/logseq/logseq/pull/13154) | Reproduced the macOS symptom, identified the stale 100 ms refocus callback, implemented a guarded refocus, added root-cause assertions |
| Mobile development configuration layers | `logseq/logseq` | `shadow-cljs.edn`, `scripts/src/logseq/tasks/dev/mobile.clj`, `docs/develop-logseq-on-mobile.md`, Capacitor-generated dev URL | [PR #13121](https://github.com/logseq/logseq/pull/13121) | Reproduced TLS and path failures, traced generated URLs and runtime module paths, built an APK, cold-started it in a Pixel 9 emulator |
| DB class/property layer and property picker | `logseq/logseq` | `src/main/frontend/worker/handler/property.cljs`, `src/main/frontend/components/property/value.cljs`, `src/main/frontend/components/select.cljs`, `deps/db/src/logseq/db/frontend/class.cljs`, `deps/outliner/...` | Local commit `c2600c83bf` | Reproduced in a real Electron UI twice, captured worker and `:app` runtime evidence, confirmed root cause, implemented a minimal fix and tests |

### Whole-system material examined

- The relationship between the **OCaml CLI** and the **ClojureScript DB worker** as two independently written layers that exchange a payload with no shared schema.
- The repository's `AGENTS.md` design policy (prefer removing compatibility layers, no new fallbacks, a single clear code path) and its effect on an actual implementation decision.
- The **multi-repository structure** of the project: `logseq/logseq` (DB line), `logseq/db-test` (DB bug tracker), `logseq/og` (Markdown line), plus `datascript`, `docs`, `publish-spa`, `mldoc`, `marketplace`, `logseq-plugin-samples`, `logseq_journal`.
- The **April 2026 product-split announcement** and its stated rationale.
- The upstream contribution process: issue claim checks, scope control, focused commits, CI, CLA, review, and merge.

### Areas not examined

| Area | Reason it was not examined |
| --- | --- |
| iOS device or simulator behaviour for the mobile HTTPS change | Verification was limited to Android; the shared configuration suggests iOS benefits but this was not manually tested |
| RTC synchronisation internals | None of the five contributions required reviewing the RTC implementation |
| Plugin API and marketplace behaviour | Outside the implementation paths exercised |
| Release packaging across all operating systems | Android debug packaging was tested; no full macOS, Windows, Linux, iOS production-release audit |
| The Markdown (OG) codebase itself | All five contributions were on the DB line; the OG repository was read only at the level of the split announcement and repository structure |
| The full frontend architecture | Depth was concentrated on queries, editor state transitions, the DB property layer, the CLI seam, and mobile development configuration |

### Evidence baseline

| Contribution | Repository | Reviewed revision |
| --- | --- | --- |
| #13071 / PR #13200 | `logseq/logseq` | `upstream/master` @ [`be800f17`](https://github.com/logseq/logseq/commit/be800f171172c259d4dd942346e4d247a0783738); merged as `883331e` |
| #1087 / PR #13154 | `logseq/logseq` | `upstream/master` @ `3b9c0d0b92`; merged as `4231bf3` |
| #13010 / PR #13121 | `logseq/logseq` | `master` at the time of the fix; merged as `de2ecab` |
| #12951 / PR #13118 | `logseq/logseq` | Old baseline [`ab57092`](https://github.com/logseq/logseq/commit/ab5709218b8ae51acb055d5e6441cdb519c1f575) vs upstream `3b9c0d0b92`; merged as `835c031` |
| #1179 | `logseq/logseq` | `upstream/master` @ [`e963b91d`](https://github.com/logseq/logseq/commit/e963b91ddf55d160371108fe0cb0ad1191fc8b91); local commit `c2600c83bf` |

## 4. Candidate Issues

### Candidate Issue A: The OCaml CLI and the ClojureScript DB worker share an implicit, unvalidated data contract

#### Current status

- Ready for group consideration
- Reviewed repository: `logseq/logseq`
- Reviewed revision: `upstream/master` @ `be800f17`, fixed on `883331e`

#### Location and repository evidence

- Repository and layer: CLI adapter layer (OCaml) and database worker layer (ClojureScript)
- Files: [`cli/lib/graph.ml`](https://github.com/logseq/logseq/blob/be800f171172c259d4dd942346e4d247a0783738/cli/lib/graph.ml) (`export_payload`), [`deps/db/src/logseq/db/sqlite/export.cljs`](https://github.com/logseq/logseq/blob/be800f171172c259d4dd942346e4d247a0783738/deps/db/src/logseq/db/sqlite/export.cljs) (`build-export`)
- Related: [issue #13071](https://github.com/logseq/logseq/issues/13071), [PR #13200](https://github.com/logseq/logseq/pull/13200), [`AGENTS.md`](https://github.com/logseq/logseq/blob/master/AGENTS.md)

#### Current design

The CLI is implemented in **OCaml**; the database worker is implemented in **ClojureScript**. They are separate programs in separate languages, and the payload they exchange is assembled by hand on one side and destructured by hand on the other. There is no shared schema, no generated types, and no validation at the seam. Each side independently "knows" which keys belong at the top level.

The repository's own guidance confirms the separation is deliberate. `cli/AGENTS.md` states:

> "This repository implements only the CLI portion. It does not include the db-worker-node server, and should use the existing cljs version of the db-worker-node server."

The root `AGENTS.md` then assigns responsibility for well-formedness to the caller:

> "Internal code may assume well-formed inputs from controlled callers."

So the worker is *permitted* to assume the payload is well-formed, which makes the CLI adapter solely responsible for correctness — and there is no check on that responsibility anywhere.

The two sides disagreed. `export_payload` lifted `:export-type` out and wrapped every remaining field inside a new `:graph-options` map. `build-export` reads different top-level keys per export type — `:block-id`, `:page-id`, `:rows`, `:node-ids` — and only `:graph-human` reads `:graph-options`. For `:selected-nodes`, the worker therefore received `(:node-ids options) = nil`, ran `keep` over nothing, and produced an empty result set.

#### Evidence

Reproduced on `upstream/master` @ `be800f17`:

```bash
node static/logseq-cli.js graph export --root-dir /tmp/... --graph issue13071-repro \
  --type edn --file selected-nodes.edn \
  --edn-options '{:export-type :selected-nodes :node-ids [193]}'
```

- CLI reported success and exit code `0`.
- The written file was 75 bytes: `{:pages-and-blocks [] :logseq.db.sqlite.export/export-type :selected-nodes}`.
- The target block demonstrably existed: `show --id 193 --output json` returned `{"block/title":"selected node survives export","db/id":193}`.

The actual payload crossing the seam was `{:export-type :selected-nodes :graph-options {:node-ids [193]}}` — the ID in the wrong place. Full record: `Issue-13071-Progress-Record.md`.

#### Concrete developer consequence

A developer adding a new export type, or changing an existing one, must edit two languages in two directories and keep an unwritten agreement in their head. Nothing fails if they get it wrong. In this case the existing test suite covered only `:graph-human`, the one type that *does* use `:graph-options`, so the mismatch was invisible to CI for as long as it existed. The fix also had to update help text, examples, and `docs/cli/logseq-cli.md`, because the seam is simultaneously a **public contract**.

#### Impact on users

Direct and severe for automation: scripts, backup jobs, and integrations received a *valid, well-formed, empty* EDN file with a success status. A caller cannot distinguish "you asked for nothing" from "your parameters were dropped". This is worse than a crash, because it silently propagates into downstream data.

#### Why this is architectural, not an isolated bug

The bug is a symptom. The structural fact is that **two independently written programs in two languages exchange a payload that is defined nowhere**. The same class of mismatch can recur for any key, in either direction, and CI will not catch it. The single defect is one instance of a missing contract.

#### Recommendation: a staged, moderate first step

Do **not** attempt to unify the languages or generate types across OCaml and ClojureScript — that is a migration and out of scope.

Staged first step: make the seam **explicit and self-checking** in the existing test layer. The parity tests already decode the real Transit payload sent to the worker; extend that pattern so every export type is asserted to carry its required keys at the expected level, and add one assertion that the worker's accepted keys match the keys the CLI can emit. This converts an implicit agreement into a checked one without changing either language or the wire format.

#### Expected impact

A future export type or renamed key fails a test instead of silently exporting nothing. Contributors editing one side of the seam get a signal pointing at the other side.

#### Cost and risk

- Implementation effort: low to moderate — test-only, extending an existing harness
- Compatibility or migration risk: none, provided the tests describe the current contract rather than changing it
- Testing requirements: payload-level assertions per export type plus one real CLI E2E per type
- Possible disadvantages: the test encodes today's contract; if maintainers intend to change the payload shape, the test must change with it

#### Maintainer awareness and position

The maintainers accepted PR #13200, which fixed the CLI side and updated the documented contract, so they are aware of the mismatch. The repository's stated policy shaped the fix direction. `AGENTS.md`, section "Error handling and compatibility", says:

> - "When modifying code, first consider removing compatibility layers rather than extending them."
> - "Prefer fail-fast over fallback."
> - "Do not add backward compatibility unless explicitly requested."
> - "Do not introduce default values to mask invalid state."
> - "Do not silently recover from programmer errors."
> - "Keep one clear code path whenever possible."
> - "Internal code may assume well-formed inputs from controlled callers."

This is why the fix removed the wrapping rather than adding a tolerant reader, and the group finding should engage with that position rather than against it. The policy argues **against** making either side more forgiving, and **for** making the contract explicit and checked — which is exactly the staged recommendation above.

---

### Candidate Issue B: Integration boundaries default to silent failure rather than a surfaced error

#### Current status

- Ready for group consideration
- Reviewed repository: `logseq/logseq` (DB line) and its bug tracker `logseq/db-test`
- Reviewed revisions: as listed in the evidence baseline table

#### Location and repository evidence

- Files: [`cli/lib/graph.ml`](https://github.com/logseq/logseq/blob/be800f171172c259d4dd942346e4d247a0783738/cli/lib/graph.ml), [`src/main/frontend/components/query.cljs`](https://github.com/logseq/logseq/blob/ab5709218b8ae51acb055d5e6441cdb519c1f575/src/main/frontend/components/query.cljs), [`src/main/frontend/worker/handler/property.cljs`](https://github.com/logseq/logseq/blob/e963b91ddf55d160371108fe0cb0ad1191fc8b91/src/main/frontend/worker/handler/property.cljs)
- Related: [PR #13200](https://github.com/logseq/logseq/pull/13200), [PR #13118](https://github.com/logseq/logseq/pull/13118), [db-test #1179](https://github.com/logseq/db-test/issues/1179)

#### Current design

Across three independently investigated contributions, the system produced a **successful-looking outcome while the data or state was wrong**.

This is not a case of the project having no policy. `AGENTS.md`, section "Error handling and compatibility", states the opposite policy explicitly:

> - "Prefer fail-fast over fallback."
> - "Do not introduce default values to mask invalid state."
> - "Do not silently recover from programmer errors."
> - "Keep one clear code path whenever possible."

The design problem is therefore a **policy-to-practice gap**: a clear written rule exists, but at these boundaries it is not applied and nothing enforces it. The three cases below each violate the stated policy.

| Experience | What the system reported | What was actually true |
| --- | --- | --- |
| CLI `graph export` (#13071) | `status: ok`, exit code `0`, "wrote file" | File contained an empty result set |
| Custom query after an error (#12951) | No new error, no console output | A React error boundary had latched; correct results were hidden until a page reload |
| Deselecting `Root Tag` in `Extends` (#1179) | Picker showed the value unchecked | The value was written back into the property |

#### Evidence

- #13071: exit code `0` with a 75-byte empty export, while `show --id 193` proved the block existed.
- #12951: reproduced `Error: 194 is not ISeqable` at `grouped-by-page-result?`; the render crash was captured by `ui/catch-error`, and correcting the query did not reset the boundary — only a reload did.
- #1179: two UI reproductions plus worker evidence — `:app` runtime returned `:extends ["Root Tag" "Issue1179 Parent Tag"]` while the picker displayed `Root` as unchecked; the worker's `get-property-node-selector-data` returned `:contains-root? true`.

#### Concrete developer consequence

Developers cannot trust success signals. Every one of these three investigations required *manual* verification against real data to establish that the reported result was wrong — a passing test suite and a `0` exit code were both insufficient. Debugging cost is paid again by every future contributor who touches these paths.

#### Impact on users

The most serious consequence in this set. Silent empty exports can propagate into backups and integrations; a latched error boundary makes a *correct* query appear broken until reload; a silently re-added property value produces data the user did not choose and may not notice.

#### Why this is architectural, not an isolated bug

Three different subsystems, one repeated pattern: **a boundary that does not distinguish "succeeded with no data" from "failed to receive data"**, despite a written policy that requires exactly that distinction.

The structural finding is stronger than "three bugs": the project has a **clear, deliberate failure-mode policy that is not enforced at its integration boundaries**. `AGENTS.md` says "Do not silently recover from programmer errors" and "Prefer fail-fast over fallback", yet three separate boundaries did the opposite without failing a test. The gap is between a stated design rule and its enforcement — a process and design-consistency problem, not a defect list. This is the error-handling and failure-mode-strategy area named in the assessment brief, evidenced three times over from direct experience.

#### Recommendation: a staged, moderate first step

Do not propose a project-wide error-handling rewrite.

Staged first step: define the rule for the **newest and most automation-facing boundary** — the CLI export path — and apply it there first. Specifically, distinguish "valid request that legitimately matched nothing" from "request that could not be interpreted", and return a non-success status in the second case. Then add one regression test per export type asserting that a dropped parameter produces a failure rather than an empty file. The pattern, once it exists in one place, is a concrete precedent for other boundaries.

#### Expected impact

Automation stops consuming empty results as success. The precedent gives later boundaries a template instead of an argument.

#### Cost and risk

- Implementation effort: low to moderate, confined to the CLI layer and its tests
- Compatibility or migration risk: moderate — scripts that currently treat every exit code `0` as success may start seeing failures, which is the intended behaviour but is a visible change
- Testing requirements: one test per export type covering both the empty-but-valid and the uninterpretable cases
- Possible disadvantages: maintainers may prefer a different mechanism; the distinction must be documented before it is enforced

#### Maintainer awareness and position

No single upstream discussion covers "silent failure" as a pattern. The three instances are tracked separately — #13071 in `logseq/logseq`, #1089 and #1179 in `logseq/db-test` — which is itself evidence for the finding: the pattern is not recognised as a pattern.

The maintainers' *position*, however, is written down and unambiguous. `AGENTS.md` states "Prefer fail-fast over fallback" and "Do not silently recover from programmer errors". The group should engage with this directly: the policy already endorses the recommendation, so the finding is not a request for a new maintainer decision. It is a request to **apply an existing stated policy at the boundaries where it is currently not applied**. That framing is much stronger than proposing a new convention, and it is the framing the group should use.

---

### Candidate Issue C: Two coexisting architectures make ordinary changes cost more than expected

#### Current status

- Ready for group consideration, with a scope caveat (see "Why this is architectural")
- Reviewed repository: the Logseq organisation — `logseq/logseq` (DB line), `logseq/og` (Markdown line), `logseq/db-test` (DB bug tracker)
- Reviewed revisions: contributions listed in the evidence baseline table

#### Location and repository evidence

- Product-split announcement: [Logseq is splitting into two versions](https://logseq.io/page/b2ad9ce1-9cb7-4436-8083-54cb4516d324/df4dc09d-0a12-4c87-904e-22a9bf4c350a)
- Repositories: [`logseq/logseq`](https://github.com/logseq/logseq), [`logseq/og`](https://github.com/logseq/og), [`logseq/db-test`](https://github.com/logseq/db-test)
- Code evidence of the DB-side complexity: `deps/db/src/logseq/db/...`, `deps/graph-parser/src/logseq/graph_parser/...`
- Issue-transfer evidence: [logseq/logseq#12951](https://github.com/logseq/logseq/issues/12951) → [logseq/db-test#1089](https://github.com/logseq/db-test/issues/1089); [logseq/logseq#12979](https://github.com/logseq/logseq/issues/12979) → [logseq/db-test#1087](https://github.com/logseq/db-test/issues/1087)

#### Current design

Logseq carries two data architectures: a **file-based Markdown** model and a **database** model backed by **SQLite** (via `sqlite-wasm`). The maintainers' own announcement states the consequence directly:

> "Supporting both file based (Markdown) and database graph — in one app has slowed us down. Every feature, bug fix, and UX change needs to be considered twice — often leading to regressions and confusion."

Their response, announced 24 April 2026, is a **product split**: "Logseq OG" for file-based graphs (moving to `github.com/logseq/og`) and "Logseq" for database graphs (remaining in `github.com/logseq/logseq`). Bug reports for the DB version are directed to a **separate repository**, `logseq/db-test`.

#### Evidence

- The announcement itself, in which maintainers describe the duplicated work and user confusion.
- A concrete instance from direct experience: the custom-query path was **refactored upstream while the fix was in flight**. Between the local baseline `ab57092` (21 July 2026) and upstream `3b9c0d0b92` there were roughly 450 commits; `grouped-by-page-result?` was deleted, the query data flow was rebuilt around `fb1047d1f8`, and the production fix became obsolete — the contribution was reduced to a test-only regression guard.
- Repository fragmentation in practice: issues are **transferred** between `logseq/logseq` and `logseq/db-test`, so the same defect changes number mid-investigation (#12951 became #1089, #12979 became #1087), and a contributor must search several repositories before knowing where a bug belongs.

#### Concrete developer consequence

Three costs, all experienced directly: (1) the same change may need to be made in two architectures; (2) internal paths a contributor depends on can be rewritten under them with no deprecation signal, so completed work can become obsolete; (3) locating the right repository and issue number for a defect is itself a step in onboarding. The `AGENTS.md` instruction to avoid compatibility layers is in direct tension with maintaining two architectures, which is the structural pressure the split is intended to relieve.

#### Impact on users

Users obtain different products through different download paths while the products are not clearly distinguished, which the maintainers state has made support harder. Bug reports land in a tracker that does not match the app the user downloaded, and the same symptom may not reproduce in the other architecture.

#### Why this is architectural, not an isolated bug

This is a deliberate, documented architectural decision with a stated maintenance cost, acknowledged by maintainers, and it changes how contributors work. It is the clearest possible architecture-level finding.

**Scope caveat the group must respect.** This theme is project-wide, and the brief requires each issue to have **moderate scope**. It must therefore be sliced into a *specific, evidence-backed* issue with a *staged* recommendation — for example, the cost of locating and transferring issues across repositories, or the absence of a deprecation signal for internal paths contributors depend on — not submitted as "two architectures should be merged", which is a rewrite proposal and explicitly excluded by the brief.

#### Recommendation: a staged, moderate first step

Do not propose merging the architectures or reversing the split — the maintainers have chosen the opposite direction, and the brief excludes rewrite proposals.

Staged first step, aimed at the contributor-facing cost rather than the architecture itself: make the **cross-repository and cross-architecture boundary legible**. Concretely, require the main repository's bug-reporting guidance to state which repository owns which class of defect, and add a short compatibility note listing the internal paths that are known to be under active refactor. This reduces onboarding and duplicate-work cost without touching the architecture.

#### Expected impact

Contributors spend less time locating the correct tracker and less time building on paths that are about to change.

#### Cost and risk

- Implementation effort: low — documentation and repository guidance
- Compatibility or migration risk: none; no code changes
- Testing requirements: none beyond link checking
- Possible disadvantages: a list of refactor-prone paths goes stale and needs ownership; maintainers may consider it unnecessary given the split is already announced

#### Maintainer awareness and position

Maximally aware: the split is the maintainers' own decision, publicly announced with the duplication cost stated as the reason. The group must **engage with their stated position** — agree that the split addresses the root problem, and argue only for the narrower contributor-facing improvement. Attempting to argue against the split would contradict the brief's exclusion of rewrite proposals and the maintainers' documented reasoning.

## 5. Candidate Positive Design Choices

### Positive Choice A: Contract-level tests that decode the real cross-language payload

- Specific design decision: The CLI parity test suite decodes the **actual Transit payload** the CLI sends to the DB worker, and asserts on its structure — rather than only asserting that a flag parses.
- Repository and files: `logseq/logseq`, [`cli/test/cli_parity_test_cases.ml`](https://github.com/logseq/logseq/blob/883331ee0113899df7b4cf93cacc2e0fb9c25bcf/cli/test/cli_parity_test_cases.ml), extended in [PR #13200](https://github.com/logseq/logseq/pull/13200)
- Permanent link: [`cli_parity_test_cases.ml` @ `883331e`](https://github.com/logseq/logseq/blob/883331ee0113899df7b4cf93cacc2e0fb9c25bcf/cli/test/cli_parity_test_cases.ml)
- Developer-level benefit: An otherwise invisible seam between two languages becomes directly testable. The regression test written for #13071 fails before the fix because the top-level `:node-ids` is absent — it catches the real defect rather than a proxy for it.
- What would otherwise be difficult: Verifying a cross-language contract by hand. Without payload-level assertions, the only way to detect a mismatch is to run a real export and inspect the output file, which is exactly why the defect survived in CI.
- Evidence from experience: The pre-fix test failed on the missing top-level `:node-ids` and passed after the fix; 91/91 CLI non-sync E2E and 237/237 CLI tests passed.
- Limitations: The test encodes the current contract. It does not prove the contract is correct, only that both sides agree today. Coverage is per export type and must be extended when a type is added.

### Positive Choice B: A written design policy that actually constrains implementation

- Specific design decision: The repository's `AGENTS.md`, section "Error handling and compatibility", states a deliberate failure-mode and compatibility policy. Verbatim, from the reviewed revision:
  > - "When modifying code, first consider removing compatibility layers rather than extending them."
  > - "Prefer fail-fast over fallback."
  > - "Do not add backward compatibility unless explicitly requested."
  > - "Do not introduce default values to mask invalid state."
  > - "Do not silently recover from programmer errors."
  > - "Keep one clear code path whenever possible."
  > - "Internal code may assume well-formed inputs from controlled callers."
- Repository and files: `logseq/logseq`, [`AGENTS.md`](https://github.com/logseq/logseq/blob/master/AGENTS.md), section "Error handling and compatibility"
- Permanent link: [`AGENTS.md`](https://github.com/logseq/logseq/blob/master/AGENTS.md)
- Developer-level benefit: The policy changed a real decision. The initial #13071 plan kept an implicit wrapping step for `:graph-human` while using top-level parameters for the other export types; re-reading this section caused that compatibility layer to be dropped in favour of the worker's native contract. A contributor-facing document that alters implementation choices is doing real work, not describing intent.
- What would otherwise be difficult: Deciding, without a stated policy, whether to preserve backwards compatibility or adopt the cleaner contract. The ambiguity is precisely what produces duplicated compatibility layers over time.
- Evidence from experience: The final #13200 implementation passes `--edn-options` directly to the worker, adds no fallback, and updates help text, examples, and docs to match the single path. It is a smaller diff than the compatibility-preserving alternative.
- Limitations: This is a contributor-guidance document, not an enforced check. Nothing fails if a contributor ignores it — which is the same enforcement gap that Candidate B identifies. The group should present the policy as a strong positive **and** note that its strength depends on enforcement.

### Alternate candidates for the group

Two further positive choices from the same body of work, offered in case the group prefers them:

- **Opt-in, environment-controlled development configuration.** `LOGSEQ_SHADOW_HTTPS` enables HTTPS for mobile development while leaving default HTTP workflows unchanged — a switch rather than a fork. Evidence: [PR #13121](https://github.com/logseq/logseq/pull/13121), [`shadow-cljs.edn`](https://github.com/logseq/logseq/blob/de2ecabf6d6891a41c104d7c4041797bcaa54b3e/shadow-cljs.edn). Limitation: Android verified, iOS not.
- **State guards testable without the timing-sensitive symptom.** The editor refocus policy after type-block conversion is expressed as explicit state conditions with focused tests, so a race is verified deterministically rather than by reproducing typing speed. Evidence: [PR #13154](https://github.com/logseq/logseq/pull/13154), [`events_test.cljs`](https://github.com/logseq/logseq/blob/4231bf309172cae619d65cec10eae98e8706b704/src/test/frontend/handler/events_test.cljs). Limitation: concerns the refocus path only; it does not establish that all editor races are handled consistently.

## 6. Suggested Priority

| Candidate | Suggested priority | Impact | Cost | Risk | Dependencies or community constraints |
| --- | --- | --- | --- | --- | --- |
| A — implicit CLI/worker contract | High | High for automation and for anyone extending export | Low-moderate (test-only first step) | Low if the test describes the current contract | None; `AGENTS.md` policy supports making the contract explicit |
| B — silent failure at boundaries | High | Highest user-visible risk of the three | Moderate (CLI layer, then precedent) | Moderate; changes visible success semantics | Needs a documented rule before enforcement; maintainers may prefer another mechanism |
| C — two coexisting architectures | Medium (as sliced) | High structurally, but the full theme is out of scope | Low as sliced (documentation) | Low as sliced | Must be sliced to moderate scope; must engage with the maintainers' split rationale rather than opposing it |

Arthur recommends ranking **B above A** despite A being the more fully evidenced: silent wrong-success has the highest user-visible cost, and B's staged first step is a subset of A's seam. The two should not both occupy a full slot if the group judges them one root cause — see Questions for the Group.

This priority table is **input to the group's ranking discussion, not a decision**. It applies only if these candidates are selected at all, and selection happens after all four submissions are in.

## 7. Questions for the Group

> **These are decisions for the group, not for this submission.** Arthur is submitting the candidates above as a **pool for group selection**, not as a pre-selected set, and is not narrowing them now. Nothing here should be treated as resolved until all four member submissions are in and the group meets to freeze the five issues and three positive design choices. The decision record belongs in `working/selection-decisions.md`.

- **Duplicate risk between A and B.** Candidate A is the *contract* problem; Candidate B is the *failure mode* of that boundary and others. If the group judges them one root cause, they merge into one slot and a slot is freed for another member's finding. Deferred to the group meeting after all four submissions are received.
- **Which three positive choices?** Arthur offers P-A and P-B as primary, with two alternates. The group needs exactly three and should prefer ones drawn from different subsystems. Deferred to the same meeting.
- **How much of Candidate C to include.** The full dual-architecture theme is project-wide; only a sliced, moderate version is admissible. Which slice the group prefers determines whether it survives selection.
- **Evidence baseline for the final report.** Which commit should the integrated report name as its reviewed revision, given the contributions span `ab57092` to `e963b91d`?
- **Exact wording check.** The `AGENTS.md` policy text must be quoted verbatim from the repository before it appears in the report.
- **Members 2-4 themes.** Do any of their reviewed areas overlap with the CLI/worker seam or the DB property layer? If so, the group should merge rather than duplicate.

## 8. Member Verification

- [x] GitHub issue, pull-request, commit, file, and maintainer-review links were checked when this submission was prepared, and each is attributed to the correct repository.
- [x] Every candidate names the revision it was verified against.
- [x] Completed upstream fixes are distinguished from current candidate issues; all five contributions are framed as evidence of a pattern, not as current defects.
- [x] Unverified areas and the Android-only platform boundary are stated.
- [x] Maintainer praise is treated as contribution evidence rather than automatic proof of a positive design choice.
- [x] Each candidate is argued as architectural rather than an isolated bug, and each recommendation is a staged, moderate first step.
- [x] The `AGENTS.md` wording has been quoted verbatim from the repository (root `AGENTS.md`, section "Error handling and compatibility", and `cli/AGENTS.md`).
- [x] The three candidates and four positive choices are submitted as a pool for group selection; Arthur has not pre-selected.
- [ ] The group has decided whether Candidates A and B are one finding or two — deferred to the group meeting after all four submissions are received.
- [ ] Arthur has completed a final review after the integrated report is drafted.
