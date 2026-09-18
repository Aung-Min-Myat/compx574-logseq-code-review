# Member 3 Review Submission: Danny Pham

## 1. Reviewer Information

- Member number: Member 3
- Full name: Danny Pham
- Student ID: 1688322
- University email: dp456@students.waikato.ac.nz
- GitHub username: [dannyinit](https://github.com/dannyinit)
- Submission status: Draft — candidates ready for group review

## 2. Contribution and Familiarity Evidence

All contributions are on `logseq/logseq` (the DB line).

| Type | Repository | Reference | Status | Your work | Areas learned |
| --- | --- | --- | --- | --- | --- |
| PR | `logseq/logseq` | [#13008](https://github.com/logseq/logseq/pull/13008) | Closed — duplicate of maintainer [#13014](https://github.com/logseq/logseq/pull/13014) | Removed a deleted page from the Recent list | Recent-list state, page-deletion flow |
| PR | `logseq/logseq` | [#13038](https://github.com/logseq/logseq/pull/13038) (issue [#12972](https://github.com/logseq/logseq/issues/12972)) | Open | Fixed query-builder "show built-in properties" checkbox not refreshing the list | Query builder, atom subscription patterns |
| PR | `logseq/logseq` | [#13142](https://github.com/logseq/logseq/pull/13142) | Merged | Fixed table view not showing a newly added property until reload | Class/property reactivity |
| PR | `logseq/logseq` | [#13174](https://github.com/logseq/logseq/pull/13174) (issue [#8663](https://github.com/logseq/logseq/issues/8663)) | Merged | Fixed block highlight colour being dropped on Text/OPML/HTML export | Export pipeline, inline markup syntax |
| PR | `logseq/logseq` | [#13202](https://github.com/logseq/logseq/pull/13202) (issue [db-test#1174](https://github.com/logseq/db-test/issues/1174)) | Closed — duplicate of maintainer [#13217](https://github.com/logseq/logseq/pull/13217) | Fixed property/tag picker ignoring arrow-key highlight on Enter | Autocomplete component state ownership |
| PR | `logseq/logseq` | [#13203](https://github.com/logseq/logseq/pull/13203) (issue [db-test#1177](https://github.com/logseq/db-test/issues/1177)) | Merged | Fixed Ctrl+Enter TODO-cycling getting stuck after the first press | Editor state, stale snapshots vs re-fetch |
| Issue comments | `logseq/logseq` | multiple (block-embed reference bug, icon checksum bug, 4-backtick invisible-text bug, tag-styling issue #6060) | Commented / some closed | Reproduction and triage; recommended closing where I couldn't reproduce | Investigative workflow, DB-line behaviour |

**What the contribution record shows.** Two of my six code contributions (#13008, #13202) were closed because the maintainer independently shipped a different fix for the exact same bug within days of mine, touching the same files. That repetition led me to look for a common cause instead of treating each as separate — see Candidate Issue A.

## 3. Review Scope

### Areas examined closely

| Area | Repository | Files/modules | Evidence | Depth |
| --- | --- | --- | --- | --- |
| Query builder property filter | `logseq/logseq` | `src/main/frontend/components/query/builder.cljs` | PR #13038 | Traced checkbox state to a plain `deref` instead of `hooks/use-atom`; implemented and tested the fix |
| Table view / class properties | `logseq/logseq` | `src/main/frontend/components/objects.cljs` | PR #13142 (merged) | Traced a fetch-once-on-mount pattern with an incomplete dependency array; fixed and verified merge |
| Recent-pages lifecycle | `logseq/logseq` | `src/main/frontend/components/page_menu.cljs`, `src/main/frontend/state.cljs` | PR #13008 (closed, duplicate) | Implemented a fix; compared it against the maintainer's alternative (#13014) |
| Property/tag picker autocomplete | `logseq/logseq` | `src/main/frontend/components/select.cljs`, `src/main/frontend/ui.cljs` | PR #13202 (closed, duplicate) | Implemented a fix; compared it line-for-line against the maintainer's independent fix (#13217), which touches the same two files |
| Editor TODO-cycling state | `logseq/logseq` | `src/main/frontend/handler/editor.cljs` | PR #13203 (merged) | Diagnosed a stale-snapshot bug; fixed it; updated an existing test that assumed synchronous behaviour |
| Export pipeline (Text/OPML/HTML) | `logseq/logseq` | `src/main/frontend/handler/export/common.cljs`, `src/main/logseq/common/export/file.cljs` | PR #13174 (merged) | Traced a property-to-export gap; reused an existing inline syntax to fix it; added test coverage |

### Whole-system material examined

- **Rendering-runtime migration history.** Read the Rum→HSX migration commit ([`d185ea58ee`](https://github.com/logseq/logseq/commit/d185ea58ee749c78b226b4f5aeed126e944b6397), 177 files, ~15,000 lines) and its architecture decision record ([ADR 0019](https://github.com/logseq/logseq/blob/3103543b660d572cb4c62dd194b37a1126dc2504/docs/adr/0019-replace-rum-with-hsx.md)) to root-cause the pattern behind my own bug fixes — see Candidate Issue A.
- **CI / test-workflow configuration.** Read `.github/workflows/clj-e2e.yml`, `.github/workflows/clj-rtc-e2e.yml`, and `clj-e2e/bb.edn` to understand what automated testing runs on ordinary commits versus what is conditional — see Candidate Issue C.
- **Architecture decision record practice.** Read all 22 files under `docs/adr/` to understand how (and how consistently) the project documents major design decisions — see Positive Choice B.
- **Contribution governance and review process:** researched for the group's Project Governance & Policies Report, and observed directly — two of my own PRs (#13008, #13202) were closed after the maintainer independently re-implemented the same fix.
- Onboarding and contributing documentation, and initial environment setup (Clojure/ClojureScript toolchain, shadow-cljs, REPL workflow).

### Areas not examined

| Area | Reason it was not examined |
| --- | --- |
| OCaml CLI / DB-worker seam | None of my contributions touched it; covered by another member's evidence |
| Mobile (Android/iOS) build and dev tooling | No mobile-related work done |
| `logseq/og` (Markdown-only line) | All my contributions are against the DB line, `logseq/logseq` |
| RTC/sync implementation internals (`deps/db-sync`, sync protocol code) | I reviewed the CI wiring around RTC tests (Candidate Issue C), not the sync algorithm itself |
| Plugin API / marketplace | Not exercised by any issue I worked on; spot-checked `libs/src/LSPlugin.ts` for API-stability concerns and found only 2 `@deprecated` markers in 1,267 lines — not enough evidence for a candidate |

### Evidence baseline

Reviewed baseline: `upstream/master` @ [`3103543b66`](https://github.com/logseq/logseq/commit/3103543b660d572cb4c62dd194b37a1126dc2504) (2026-09-18). Each candidate below is also pinned to the specific commit(s) it is evidenced against.

## 4. Candidate Issues

### Candidate Issue A: A one-shot rendering-runtime migration removed each component's automatic reactivity, and nothing systematically verifies it was replaced

#### Current status

- Ready for group consideration
- Reviewed repository: `logseq/logseq`
- Reviewed commit: migration commit [`d185ea58ee`](https://github.com/logseq/logseq/commit/d185ea58ee749c78b226b4f5aeed126e944b6397) (2026-06-02); baseline `3103543b66` (2026-09-18)

#### Location and repository evidence

- Repository and layer: frontend UI components (ClojureScript, React), rendering-runtime layer (Rum → HSX)
- Files: `src/main/frontend/components/query/builder.cljs`; `src/main/frontend/components/objects.cljs`; `src/main/frontend/components/select.cljs` and `src/main/frontend/ui.cljs`; `src/main/frontend/handler/editor.cljs`; `src/main/frontend/components/page_menu.cljs` and `src/main/frontend/state.cljs` — **all six rewritten by the same migration commit**
- Permanent code links (pinned to commit):
  - [migration commit](https://github.com/logseq/logseq/commit/d185ea58ee749c78b226b4f5aeed126e944b6397) — touches all six files above in one shot
  - [ADR 0019: Replace Rum With HSX](https://github.com/logseq/logseq/blob/3103543b660d572cb4c62dd194b37a1126dc2504/docs/adr/0019-replace-rum-with-hsx.md)
  - [query builder fix](https://github.com/logseq/logseq/commit/e43ade2b9c17c74226540bfa0c628342ef269da9) (PR #13038, open)
  - [table view fix](https://github.com/logseq/logseq/commit/acfd4121208a0f6c66dcc2d1f76183809f1138da) (PR #13142, merged)
  - [my picker fix](https://github.com/logseq/logseq/commit/ff5d91d34d08950919f8c647ab682f6717329d9d) (PR #13202, closed as duplicate) / [maintainer's independent picker fix](https://github.com/logseq/logseq/commit/edc63a4160) (PR #13217, merged — same two files)
  - [TODO-cycling fix](https://github.com/logseq/logseq/commit/7562db4814b127982562547958801ef8578b0124) (PR #13203, merged)
  - [Recent-list fix](https://github.com/logseq/logseq/commit/b9271ca2fac5fb71011e628676cb84d0b4a7c7bb) (PR #13008, closed as duplicate of #13014)
- Related: [db-test#1174](https://github.com/logseq/db-test/issues/1174), [db-test#1177](https://github.com/logseq/db-test/issues/1177), [issue #12972](https://github.com/logseq/logseq/issues/12972)

#### Current design

Per ADR 0019, Rum gave components *implicit* reactivity: `rum/reactive` tracked which atoms a component read during render and re-rendered it automatically when they changed. HSX (the project's own fork, adopted in the same migration) requires *explicit* subscription: a component must call `hooks/use-atom` or `db.hooks/use-query` to be notified of a change; a bare `deref` renders once and never updates. The migration commit rewrote 177 files in one pass to make this switch.

#### Evidence

I compared the buggy functions before and after the migration commit directly:

- `src/main/frontend/components/query/builder.cljs`: before migration, `property-select` was a plain `rum/defc` (no `rum/reactive`) reading `@*private-property?` — already latent, but shielded because a parent component's `rum/reactive` re-rendered the whole subtree on unrelated changes. After migration, the same bare `@*private-property?` deref remained, but nothing re-renders this component implicitly anymore. My fix (PR #13038, still open) replaces it with `hooks/use-atom`.
- `src/main/frontend/components/objects.cljs`: before migration, `class-objects` was `rum/defcs class-objects < rum/reactive db-mixins/query mixins/container-id` — reactive by construction. After migration, it became a plain `hsx/defc class-objects` with **no reactivity mixin at all**. My fix (PR #13142, merged) added the missing dependency tracking.
- The same commit also rewrote `select.cljs`, `ui.cljs`, `editor.cljs`, `page_menu.cljs`, and `state.cljs` — the exact files behind my other three fixes (#13202, #13203, #13008).

All five of my state-related bug fixes sit in files this one commit rewrote.

#### Concrete developer consequence

Before this investigation I assumed these were five unrelated bugs and treated each as its own fix. They are one migration's blind spot recurring five times. A contributor fixing any one of them in isolation (as I initially did, and as the maintainer did independently for two of them: #13217, #13014) has no way to know four siblings exist elsewhere in the same rewritten surface, because nothing marks "this component used to be implicitly reactive under Rum and may not be anymore."

#### Impact on users

A checkbox or a new property appears to do nothing. Arrow keys pick the wrong item. A keyboard shortcut seems to freeze after one use. A deleted page lingers in a list. None of these lose data outright, but they surfaced only after this migration, in code that previously worked.

#### Why this is architectural, not an isolated bug

This is not five bugs; it is one migration decision — rewriting the entire component tree's reactivity model in a single commit — with a systematic blind spot for components that relied on Rum's implicit tracking without an explicit equivalent. ADR 0019's own "Verification" section requires checking hook-ordering, subscription cleanup, and behavioural parity for named flows, but has no checklist item for "every component that lost `rum/reactive` gained an explicit equivalent." That specific gap is what all five bugs share.

#### Recommendation: a staged, moderate first step

Not a re-migration or a revert — Rum is fully removed (zero references remain in the codebase) and reverting would be a larger change than the problem justifies. Staged step: add a project-local `clj-kondo` lint rule (the project already uses clj-kondo) flagging a bare `deref` of a shared atom inside an `hsx/defc` render body that isn't wrapped in `hooks/use-atom` or `db.hooks/use-query`, matching the shape of the three confirmed cases. Add one line to ADR 0019's own Verification checklist naming this exact check, so future rendering-runtime changes inherit the lesson.

#### Expected impact

The next instance of this bug class is caught by lint before merge, instead of shipping, being reported by a user, and being independently rediscovered by two people (as already happened twice).

#### Cost and risk

- Implementation effort: low to moderate — one lint rule, one ADR edit, no production code changes.
- Compatibility or migration risk: low; existing violations elsewhere in the codebase would need triaging once the rule is enabled.
- Testing requirements: verify the rule fires on the three confirmed `deref`-shaped cases and does not flag correct `hooks/use-atom` usage.
- Possible disadvantages: catches the "bare deref" shape specifically; the picker and Recent-list bugs (state kept local to the wrong component, or no central lifecycle hook) are related but structurally different and wouldn't be caught by this rule alone.

#### Maintainer awareness and position

ADR 0019 shows real care — a detailed Context/Decision/Consequences/Verification structure, not a rushed change. The gap is narrow and specific, not evidence of carelessness. In practice, the maintainer has independently fixed two of the five instances (#13217, #13014) after the fact, treating each as a one-off bug rather than revisiting the migration's verification checklist — which is exactly the case for adding the missing checklist item rather than re-litigating the migration itself.

---

### Candidate Issue B: The property model has no declared contract for how a property should appear on export, so DB-only data can vanish silently

#### Current status

- Ready for group consideration, with a duplicate-risk caveat — see Questions for the Group
- Reviewed repository: `logseq/logseq`
- Reviewed commit: [`d1cefadbf5`](https://github.com/logseq/logseq/commit/d1cefadbf5083714a764fb79ac16215fbf9610b3) (PR #13174, merged 2026-09-07), baseline `3103543b66`

#### Location and repository evidence

- Files: `src/main/frontend/handler/export/common.cljs`, `src/main/logseq/common/export/file.cljs`
- Related: [issue #8663](https://github.com/logseq/logseq/issues/8663) (opened February 2023, closed by #13174 in September 2026)

#### Current design

A block's highlight colour is stored as a property, not as text in the block's content. The Text/OPML/HTML exporters walk block content to build their output. A property that is never projected into that content is invisible to them by construction — the exporter doesn't fail, it simply never sees the data.

#### Evidence

Issue #8663 stayed open for about three and a half years: highlighting a block and exporting to Text, OPML, or HTML silently dropped the colour, with no error, in all three formats at once, because none of the three exporters had ever been told the property existed. The fix (#13174) does not add a general mechanism. It adds one narrowly scoped `:encode-highlight-as-mark?` flag for this single property, explicitly kept separate from the markdown-mirror path that shares the same function "to avoid affecting other paths." It is a one-off patch for one property, not a fix to the underlying gap.

#### Concrete developer consequence

Nothing tells a developer adding a new DB property that they also need to decide how it appears in each export format. The only feedback loop is a user filing a bug for that specific property in that specific export path. The three-and-a-half-year gap between the issue being filed and fixed suggests this loop is slow.

#### Impact on users

Users exporting notes for backup, sharing, or migration lose formatting or metadata with no warning. They only find out by comparing the export to the original.

#### Why this is architectural, not an isolated bug

One missing property is a bug. The pattern is that the export pipeline has no declared, checked mapping from "properties a DB block or page can have" to "how each exporter represents them." Any future DB-only property has the same silent gap by default, unless someone happens to test that export path.

#### Recommendation: a staged, moderate first step

Not a general bidirectional serialization framework for all properties and all formats. Staged step: add one regression test per export format asserting that every current built-in DB property is either represented in the output or is on an explicit, documented "intentionally not exported" list. This doesn't catch unknown future gaps automatically, but it turns "property silently missing from export" into a failing test the next time a built-in property is added without an export-list entry.

#### Expected impact

New built-in properties get caught by the test the first time they're added, instead of waiting for a user to notice.

#### Cost and risk

- Implementation effort: low — enumerate current built-in properties, write one assertion per format.
- Compatibility or migration risk: none; test-only.
- Testing requirements: needs a maintained list of current built-in properties, which needs an owner or it goes stale.
- Possible disadvantages: covers built-in properties only, not arbitrary user-defined ones; doesn't stop the same gap on a genuinely new property until the list is updated.

#### Maintainer awareness and position

The issue sat for over three years with no comment treating it as anything other than one missing feature. I found no tracker discussion of "export completeness for DB properties" as a general problem.

---

### Candidate Issue C: The deeper RTC/sync regression suite runs only when a commit message happens to contain the word "rtc," not based on what the change touches

#### Current status

- Ready for group consideration
- Reviewed repository: `logseq/logseq`
- Reviewed commit/revision: baseline `3103543b66` (2026-09-18); workflow history checked back to commit `23417ad2d8`

#### Location and repository evidence

- Repository and layer: CI / test infrastructure
- Files: `.github/workflows/clj-rtc-e2e.yml`; for comparison, `.github/workflows/clj-e2e.yml`; `clj-e2e/bb.edn`
- Permanent links:
  - [`clj-rtc-e2e.yml`](https://github.com/logseq/logseq/blob/3103543b660d572cb4c62dd194b37a1126dc2504/.github/workflows/clj-rtc-e2e.yml)
  - [`clj-e2e.yml`](https://github.com/logseq/logseq/blob/3103543b660d572cb4c62dd194b37a1126dc2504/.github/workflows/clj-e2e.yml)
  - [`clj-e2e/bb.edn`](https://github.com/logseq/logseq/blob/3103543b660d572cb4c62dd194b37a1126dc2504/clj-e2e/bb.edn)

#### Current design

Real-time collaboration/sync is the most actively revised subsystem in the codebase by architecture-decision count: 9 of the 22 files under `docs/adr/` concern sync (encryption, checksums, rebase, snapshot download, and more). Basic RTC correctness (`logseq.e2e.rtc-basic-test`) runs on every push and pull request touching `src/**`, `deps/**`, or `packages/**`, as part of the always-on `clj-e2e.yml` workflow's `outliner-rtc` shard.

A second, deeper suite — `rtc-extra-test` and `rtc-extra-part2-test`, run via `clj-rtc-e2e.yml` with a 40-minute timeout (five times the 8-minute budget of the basic shards) — exists specifically to go beyond basic coverage. Both of its jobs declare `on.push.paths` and `on.pull_request.paths` matching `src/**`, `deps/**`, `packages/**` — the same paths as the always-on suite. But each job also carries:

```yaml
if: "contains(github.event.head_commit.message, 'rtc')"
```

The workflow's own path filters say it should run whenever core code changes. The job-level condition instead requires the commit author to type "rtc" somewhere in their message, regardless of what the diff touches.

#### Evidence

Of the last 500 commits on `upstream/master`, only 10 (2%) contain "rtc" in the commit message. No scheduled or nightly workflow exists to compensate — the only `schedule:`-triggered workflows in `.github/workflows/` are `stale-issues.yml` and a commented-out (disabled) nightly desktop build. Git history shows the general E2E workflow was previously enabled to "run for every commit" (`23417ad2d8`) before the RTC-specific keyword gate was added in later commits.

#### Concrete developer consequence

A contributor changing shared code under `src/**`, `deps/**`, or `packages/**` — most of the application — gets a fully green CI even if their change breaks the deeper RTC regression tests, unless they happen to type "rtc" in a commit message for a change that may have nothing else to do with sync. The trigger is decoupled from the code paths it is meant to protect.

#### Impact on users

A regression in the most actively revised, most architecturally complex subsystem (multi-device sync) can reach a release without the suite built specifically to catch it ever running. Sync bugs risk data conflicts or loss across devices, not just a visible UI glitch.

#### Why this is architectural, not an isolated bug

This is not a missing test — the tests exist, are well organised, and already have a working path-based trigger declared in the same file. It is a wiring decision that silently overrides that trigger with an unrelated manual keyword check. Any future change to sync-adjacent code inherits this gap by default, regardless of how carefully it's written.

#### Recommendation: a staged, moderate first step

Not "run the full 40-minute suite on every commit" — the split into `rtc-extra-test` / `rtc-extra-part2-test` and the 40-minute timeout suggest the team already tried to manage cost, likely the actual reason for the keyword gate. Staged step: replace the commit-message `if:` condition with a narrower `paths:` filter scoped to sync-relevant directories the workflow already lists broadly (for example `deps/db-sync/**`, `deps/outliner/**`, and the frontend RTC handler namespace), instead of gating on an unrelated keyword. This reuses a mechanism the workflow already has and doesn't require running the suite on every commit if the paths are scoped tightly.

#### Expected impact

The suite runs when sync-relevant code actually changes, not when a commit message happens to contain a keyword — closing the gap without necessarily increasing CI cost much, if paths are scoped narrowly.

#### Cost and risk

- Implementation effort: low — edit the path list, remove or narrow the `if:` condition.
- Compatibility or migration risk: none to production code; CI minutes increase versus the current ~2% trigger rate, proportional to how broadly the paths are scoped.
- Testing requirements: verify the narrowed path list still covers the files behind past RTC regressions.
- Possible disadvantages: scoping paths too broadly (all of `src/**`, as currently declared) brings back the cost problem the keyword gate was likely trying to solve; the paths need to be chosen deliberately, not just widened back to what's already there.

#### Maintainer awareness and position

I found no tracker or ADR discussion of this specific gate. Git history suggests it was a deliberate, cost-driven narrowing rather than an oversight (the workflow was previously "every commit," then split and gated). The recommendation should acknowledge that likely rationale and propose a path-based fix that respects the same cost concern, rather than simply asking to remove the gate.

## 5. Candidate Positive Design Choices

### Positive Choice A: Reusing an existing inline text syntax as an export fallback, instead of building new per-format serialization

- Specific design decision: Logseq already has a lightweight inline markup, `^^...^^`, for representing highlighted text directly in block content, used for editing and import. PR #13174 fixed issue #8663 by having the exporter wrap highlighted text in this existing markup instead of inventing new per-format handling for a highlight property.
- Repository and files: `logseq/logseq`, [`src/main/logseq/common/export/file.cljs` @ `d1cefadbf5`](https://github.com/logseq/logseq/commit/d1cefadbf5083714a764fb79ac16215fbf9610b3)
- Developer-level benefit: because the text markup already existed and every exporter already passes block content through largely unchanged, the fix for all three export formats was a single small change, not three separate serializers.
- What would otherwise be difficult: any DB-only property with no plain-text representation needs its own bespoke per-format export code, one format at a time — the exact gap described in Candidate Issue B. The inline-syntax fallback is what kept this particular fix small.
- Evidence from experience: the merged fix touched two production files (about 22 lines total) plus one test file, and fixed Text, OPML, and HTML export at the same time.
- Limitations: this only works for properties that can be meaningfully written as inline text — colour-as-markup works, but a numeric priority or a scheduled date might not translate as naturally. It reduces the cost of Candidate Issue B's problem after the fact; it does not prevent the gap from occurring in the first place.

### Positive Choice B: A written, versioned decision record for every major architecture change, with an explicit verification checklist

- Specific design decision: the project keeps `docs/adr/`, 22 sequentially numbered architecture decision records as of the reviewed commit. Each follows a Context / Decision / Consequences structure, and several — including ADR 0019, used directly in Candidate Issue A — also carry an explicit pre-merge Verification checklist.
- Repository and files: `logseq/logseq`, [`docs/adr/`](https://github.com/logseq/logseq/tree/3103543b660d572cb4c62dd194b37a1126dc2504/docs/adr)
- Developer-level benefit: investigating Candidate Issue A, I could read exactly why the Rum→HSX migration happened, what tradeoff was accepted (a self-maintained fork), and what behaviour the team explicitly intended to preserve — all dated, in one place, instead of reconstructed from commit messages.
- What would otherwise be difficult: without the ADR, understanding why a 177-file rewrite happened and what its author considered "must not break" would mean reading through dozens of terse conventional-commit messages (`fix: parens`, `fix: sidebar item jitter`) and guessing at intent.
- Evidence from experience: I used ADR 0019 directly to root-cause Candidate Issue A; its Verification section is also where I found the specific gap (no check for "components that lost implicit reactivity gained an explicit equivalent") that the recommendation in Candidate A proposes to close.
- Limitations: coverage is uneven. All 22 ADRs concern the sync/RTC and rendering-runtime subsystems; the CLI/DB-worker contract and the export-property gap (Candidate Issue B) have no corresponding ADR, so the practice isn't applied project-wide. An ADR also documents the *decision*, not whether it was completely and correctly implemented — Candidate Issue A is evidence that a well-written ADR can still leave a gap.

## 6. Suggested Priority

| Candidate | Suggested priority | Impact | Cost | Risk | Dependencies or community constraints |
| --- | --- | --- | --- | --- | --- |
| A — Rum→HSX reactivity blind spot | High | Medium-high: root cause of 5 confirmed bugs across different features, 2 independently re-fixed by the maintainer | Low (lint rule + ADR edit) | Low | None |
| C — RTC test gated by commit-message keyword | High | High if it fires: the one suite built for the most complex, most-revised subsystem currently runs on ~2% of relevant commits | Low to moderate (depends on path scope chosen) | Low; must be scoped to avoid reintroducing the CI-cost problem the gate likely solved | Needs maintainer input on which paths are actually sync-relevant |
| B — export property contract | Medium | Medium: user-facing, but no data is destroyed, only the export copy | Low (test-only) | Low | May share a root cause with another member's boundary/silent-failure theme |

## 7. Questions for the Group

- **Possible duplicate.** Candidate Issue B (no export contract for properties) may share a root cause with a "silent failure at integration boundaries" theme from another member's pool, in a different subsystem (the CLI/DB-worker seam rather than export). We should decide together whether these merge into one slot or stay separate because the subsystems differ.
- **Evidence that still needs validation.** For Candidate A, I've confirmed the before/after reactivity change for two of five files (`builder.cljs`, `objects.cljs`) by diffing against the pre-migration commit; the other three (`select.cljs`/`ui.cljs`, `editor.cljs`, `page_menu.cljs`/`state.cljs`) are confirmed as touched by the same migration commit but not individually diffed line-by-line the same way. For Candidate C, I haven't confirmed with a maintainer whether the commit-message gate was actually added for CI cost reasons — that's my inference from the timeout/sharding pattern, not a stated fact.
- **Decision needed.** Whether Candidate A's recommended fix (a lint rule) and Candidate C's (narrower CI paths) count as "moderate scope," and which of the three candidates here (A, B, C) the group wants to carry forward if only some can be selected.

## 8. Member Verification

- [x] I personally checked every link in this submission, including which repository each belongs to.
- [x] Every claim names the commit or revision it was verified against.
- [x] I distinguished current findings from bugs that have already been fixed (PRs #13142, #13174, #13203 are merged; #13008 and #13202 are closed as duplicates of maintainer fixes, kept as pattern evidence, not as open bugs).
- [x] I stated areas I did not examine.
- [x] Each candidate is an architectural or design finding, not an isolated bug, style preference, feature request, or rewrite proposal.
- [x] Each recommendation is a moderate, staged first step with cost and risk.
- [x] I identified uncertainty (see Questions for the Group) and platform limitations (see Areas not examined).
- [x] I am comfortable being named as the verifier of claims about my reviewed areas.
