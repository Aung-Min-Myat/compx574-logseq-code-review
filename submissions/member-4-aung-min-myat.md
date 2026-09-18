# Member Review Submission: Aung Min Myat

> **Note on drafting.** This file was drafted with AI assistance from my own weekly logs, then checked against GitHub directly. All nine links I supplied (PR #13011, PR #12989, PR #12965, Issue #12893, PR #13153, db-test#1126, Issue #12987, Issue #13112, db-test#914) were fetched and read during this drafting pass, and two repository corrections came out of that check: my Week 6 "#1126" and "#914" references are actually **`logseq/db-test`** issues, not `logseq/logseq` — fixed throughout this file. I still need to personally re-read everything below against the group's "How AI May Be Used" policy before opening a pull request against `hahaArthur17/compx574-logseq-code-review` — see Section 8 for exactly what's still open.

## 1. Reviewer Information

- Member number: Member 4
- Full name: Aung Min Myat
- Student ID: 1697158
- University email: am2823@students.waikato.ac.nz
- GitHub username: [Aung-Min-Myat](https://github.com/Aung-Min-Myat)
- Submission status: Draft — candidates ready for group review

## 2. Contribution and Familiarity Evidence

All contributions are on `logseq/logseq` (the DB line) or its paired bug tracker, `logseq/db-test`.

| Type | Repository | Reference | Status | Your work | Verification |
| --- | --- | --- | --- | --- | --- |
| PR | `logseq/logseq` | [#13201](https://github.com/logseq/logseq/pull/13201) (fixes [db-test#1173](https://github.com/logseq/db-test/issues/1173)) | Merged 15 Sep 2026 by tiensonqin | Property option icons missing from tag-table columns for any property with closed values | **Confirmed on GitHub** — merge commit `381fab6`, fix commit `77b44f3` |
| PR | `logseq/logseq` | [#13153](https://github.com/logseq/logseq/pull/13153) (intended to fix [db-test#1135](https://github.com/logseq/db-test/issues/1135)) | Merged 1 Sep 2026 by tiensonqin | Deleting a page you're viewing redirected to home instead of the previous page; fixed with `(.back js/window.history)` in `pipeline.cljs`, with a `(not (util/mobile?))` guard | **Confirmed on GitHub** — merge commit `211ccfa`. Its own "Fixes #1135" link resolves to the *wrong* issue — see Candidate Issue B |
| PR | `logseq/logseq` | [#13011](https://github.com/logseq/logseq/pull/13011) (fixes [db-test#1054](https://github.com/logseq/db-test/issues/1054)) | Confirmed to exist; merge status not shown in the page extraction I could read | Custom text-property filters (e.g. `author`) missed results on the first click, and re-filtering created duplicate filter chips that could only be removed together | **Confirmed description on GitHub.** My Week 4 log also describes a maintainer follow-up adding an "index-based slot remover" for compound filters — not visible in the description text I could read, so still needs checking on the PR's Commits/Files tabs |
| PR | `logseq/logseq` | [#12965](https://github.com/logseq/logseq/pull/12965) and [#12989](https://github.com/logseq/logseq/pull/12989) (fixes [db-test#1075](https://github.com/logseq/db-test/issues/1075)) | Both confirmed to exist, identical title and description | Renaming a page to a name still held by a soft-deleted (recycled) page threw a false "Duplicate page" error; fixed `validate-unique-for-page` in `deps/outliner/src/logseq/outliner/validate.cljs` to skip recycled entities | **Confirmed on GitHub**, but as *two* PRs with identical content — I don't yet know if one superseded the other or if this is a duplicate submission. See Section 7 |
| Issue comment | `logseq/logseq` | [#12893](https://github.com/logseq/logseq/issues/12893) | Open; I left a comment, no linked fix found | Confirmed the reported bug (side menu language not updating) also affects the settings menu's theme/colour section labels until the panel is closed and reopened | **Confirmed the issue and my comment exist.** I could not find a merged PR by me resolving this — my Week 4 log says I "fixed this and submitted a pull request," but I have not located that PR. See Section 7 |
| Issue comment | `logseq/logseq` | [#13112](https://github.com/logseq/logseq/issues/13112) | Open; commented | Tested on latest `master` (Web + Desktop): adding to Favorites via the 3-dot menu already works; dragging from Recent to Favorites is unimplemented — traced to `left_sidebar.cljs`, where the `dnd-component` wrappers only handle reordering *existing* favorites | **Confirmed on GitHub**, including the file reference |
| Issue comment | `logseq/db-test` | [#1126](https://github.com/logseq/db-test/issues/1126) | Open | Reproduced on the live app, then on a local build of latest `master`; the linked-references count showed correctly on `master` but not on the released build, so I recommended closing as "fixed, pending release" | **Confirmed the issue exists** in `logseq/db-test` (corrected from my earlier draft, which had it under `logseq/logseq`); comment content taken from my own saved session notes |
| Issue comment | `logseq/db-test` | [#914](https://github.com/logseq/db-test/issues/914) | Open, labelled `bug` | Confirmed that editing a page title from table view now updates in place on the latest build | **Confirmed the issue exists** in `logseq/db-test` (corrected from `logseq/logseq`); still open and still labelled `bug` as of this check, which is consistent with my not having closed it myself |
| Issue comment | `logseq/logseq` | [#12987](https://github.com/logseq/logseq/issues/12987) | Open | Traced `order-list-index`; numbering is scoped to the parent block by design, so I reclassified the report as a feature request rather than a bug | **Confirmed the issue exists**, opened by Carlchenmw on 7 Aug 2026, still open; comment content taken from my own saved session notes |

**What the contribution record shows.** The two fully re-verified fixes (#13201, #13153) both involve data or navigation state that is correct in one place and silently wrong in another — a closed-value property that renders correctly on its own page but not in a table, a delete action that correctly returns home but not to where the user actually was. My PR #13153 also turned up a second pattern I hadn't noticed until checking these links just now: it is itself an example of a cross-repository issue-reference problem, which is the basis of Candidate Issue B below.

## 3. Review Scope

### Areas examined closely

| Area | Repository | Files/modules | Evidence | Depth |
| --- | --- | --- | --- | --- |
| Property rendering across the db-worker boundary | `logseq/logseq` | `src/main/frontend/components/property/value.cljs`, `entity_plus.cljc`, the `:thread-api/get-class-properties` worker handler, `src/main/frontend/components/views.cljs` | PR #13201 (merged) | Traced `:property/closed-values` from a live-entity reverse reference through to the flattened worker response; implemented and unit-tested the fix |
| Page-delete navigation | `logseq/logseq` | `src/main/frontend/modules/outliner/pipeline.cljs` | PR #13153 (merged); PR #13156 (tiensonqin's follow-up, merged 6 days later) | Implemented the initial `history.back()` fix with a mobile guard; the maintainer's follow-up (`src/main/frontend/handler/route.cljs`, `redirect-to-previous!`) added an empty-history fallback and stopped a mobile double-pop, explicitly building on top of my PR rather than replacing it |
| Compound/text property filters | `logseq/logseq` | filter selection handling in `views.cljs`; ID-based vs content-based matching in `view.cljs` | PR #13011 | Diagnosed a two-part bug: an ID-matching fast path that fails for text properties, and a filter-selection handler that stacked duplicate clauses instead of replacing them |
| Page rename validation against recycled pages | `logseq/logseq` | `deps/outliner/src/logseq/outliner/validate.cljs` (`validate-unique-for-page`) | PR #12965 / PR #12989 | Replaced a `first`-based collision check with a `some` + `recycled?` predicate so a soft-deleted page no longer blocks a rename to its old name |
| Issue triage: Favorites/Recent drag, table-view title edit, linked-references counter, ordered-list numbering | `logseq/logseq`, `logseq/db-test` | `left_sidebar.cljs` (Favorites); `order-list-index` (ordered lists) | Issues #13112, db-test#914, db-test#1126, #12987 | Reproduction, environment comparison (released build vs. local `master`), written explanations to the community |
| Settings/sidebar locale reactivity | `logseq/logseq` | not yet located | Issue #12893 | Left a confirming/extending comment on someone else's report; I have not located a fix of my own for this one yet |

### Whole-system material examined

- **The group's own collaboration workspace and its 17 September 2026 direction-change note** (`README.md`, `working/teacher-direction-vs-brief-comparison.md`): read these to align this submission with the group's shared architecture-level framing, the multi-repository map, and the corrected facts about `db-test` (a bug tracker, not a database technology) and the Logseq OG/DB product split.
- **Local development environment and REPL workflow**: setup, compilation, and iteration cycle, covered in my own Week 1–2 logs.
- **Contribution governance in practice**: observed the founder-centred review and merge pattern directly through my own PRs — including a case (#13153 → #13156) where the founder built a follow-up on top of my merged fix rather than replacing it, which is a gentler variant of the "founder re-implements independently" pattern the group's Governance Report documents elsewhere.

### Areas not examined

| Area | Reason it was not examined |
| --- | --- |
| CLI (OCaml) and the `db-worker-node` seam | Not touched by any of my contributions; covered by Member 1's evidence (A2, A3) |
| `logseq/og` (the file-based Markdown line) | All my contributions are against the DB line, `logseq/logseq` |
| Mobile (Android/iOS) native build tooling | I did not do native-build work, though PR #13201's worker fix and PR #13153/#13156's navigation fix both explicitly account for mobile behaviour |
| RTC/sync internals | Not exercised by any issue I worked on |
| The exact files behind Issue #12893 | Not yet located — see Section 7 |

### Evidence baseline

Reviewed baseline: `upstream/master` @ `3103543b66` (18 September 2026), matching the group's shared evidence baseline used elsewhere in this repository. PR #13201 (`381fab6`, 15 Sep 2026), PR #13153 (`211ccfa`, 1 Sep 2026), and PR #13156 (`7eb7911`, 7 Sep 2026) are all more recent than that baseline and should be checked against current `master` before the report is finalised.

## 4. Candidate Issues

### Candidate Issue A: Derived/virtual attributes can silently disappear when data crosses the ClojureScript worker boundary

**Possible duplicate — see note below.** This looks like a second, independently-discovered instance of Member 1's Candidate A3 ("Silent failure instead of surfaced errors at boundaries"). I'd suggest the group treat it as corroborating evidence for A3 rather than a separate issue slot, per the "do not spend two slots on A3-style silent failures that share one missing-contract root cause" guidance already recorded in `working/teacher-direction-vs-brief-comparison.md`.

#### Current status

- Ready for group consideration, pending the merge-with-A3 decision above
- Reviewed repository: `logseq/logseq`
- Reviewed commit: fix commit [`77b44f3`](https://github.com/logseq/logseq/pull/13201/commits/77b44f32d9481983fae8e5f1699364e3ee3d82f0), merged as [`381fab6`](https://github.com/logseq/logseq/commit/381fab6eea3178b26670c6ac6a20a6dcbd3ba02a) on 15 September 2026

#### Location and repository evidence

- Repository and layer: shared `db-worker` layer (ClojureScript), compiled into the build used by Desktop, Web, and Mobile
- Files: `src/main/frontend/components/property/value.cljs` (`select-item`, around line 1491); `entity_plus.cljc` (around line 180, the `:block/_closed-value-property` reverse reference); the `:thread-api/get-class-properties` worker handler; `src/main/frontend/components/views.cljs` (around line 80, the `built-in-property` fallback)
- Permanent code links: [PR #13201](https://github.com/logseq/logseq/pull/13201) (merged), [db-test#1173](https://github.com/logseq/db-test/issues/1173)

#### Current design

`:property/closed-values` is a *virtual* attribute — it only resolves on a live DataScript entity, backed by the reverse reference `:block/_closed-value-property`. Code that reaches a property through a live entity (a property's own page, via `property-related-objects`) sees it correctly. A tag/class page, however, loads its table columns through `:thread-api/get-class-properties`, which returns a flattened, forward-datoms-only map (`entity-forward-map`) across the worker boundary. The reverse reference — and the closed values and icons it carries — does not survive that trip.

#### Evidence

Property option icons were missing from every `#Task`-style table column, while the exact same values displayed correctly with icons on the property's own page. The bug affected every property that has closed values, not just one. A `built-in-property` fallback already existed in `views.cljs`, but it only triggers when `:db/ident` is absent — and the worker's flattened map always has one, so the fallback never engaged either; it looked like a safety net but wasn't one for this case. The fix attaches the closed values inside `get-class-properties` itself, reusing the same `select-keys` shape already used by `:thread-api/get-property-closed-values`, and extracts the logic into a small function specifically so it could be unit tested (`get-class-properties-keeps-closed-values-for-icons`, in `src/test/frontend/worker/handler/property_test.cljs`).

#### Concrete developer consequence

A developer adding a new property type, or a new UI surface that reads properties through the worker API rather than a live entity, has no signal that a derived attribute needs to be explicitly re-attached before it crosses the boundary. Worse, the existing `built-in-property` fallback looks like it should catch this and silently doesn't — a contributor could reasonably assume they were already covered.

#### Impact on users

The icons clearly exist — they render correctly one click away, on the property's own page — but do not appear where users spend the most time, a shared tag table. Nothing errors or warns; the table simply looks slightly wrong in a way that reads as a styling quirk rather than a data-loss bug, which is a plausible reason it went unreported for a while.

#### Why this is architectural, not an isolated bug

This is the same underlying failure class Member 1 documents for the CLI export boundary in A3: a value that exists only on a live, in-memory representation, crossing a serialization boundary that carries forward data only, with no declared contract for which derived fields must be preserved. It surfaced independently in a different subsystem (property rendering, not CLI export), which is stronger evidence of a shared structural cause than two symptoms found in the same code path would be.

#### Realistic improvement / migration direction (staged, moderate)

Not a redesign of the worker protocol. A staged first step: give worker handlers like `get-class-properties` a short, explicit, tested list of "virtual attributes that must be re-attached" — closed-values/icon is one instance; there may be others — each backed by a unit test in the style this PR already added. That gives the next contributor a concrete place to look and a test that fails loudly, without touching how the worker boundary serializes data in general.

#### Expected impact

The next derived/virtual attribute that needs to survive the worker boundary is caught by an explicit test at PR time, instead of shipping as a quiet visual gap a user has to notice and report.

#### Cost and risk

- Implementation effort: low — this is testing and documentation discipline, not new architecture.
- Compatibility or migration risk: low; no production behaviour changes beyond the properties already identified.
- Testing requirements: one unit test per virtual attribute added to the "must re-attach" list.
- Possible disadvantages: the list only helps if it is kept current as new virtual attributes are introduced; without an owner, it degrades back to the status quo Candidate A describes.

#### Maintainer awareness and position

`tiensonqin` reviewed and merged this fix within two days of the PR opening ("QAed and works great, thanks for the fix!") — a fast, engaged response, consistent with the founder-centred review pattern already documented in the group's Governance & Policies Report. I found no tracker discussion of the underlying pattern (virtual attributes not surviving the worker boundary in general), only of this one symptom.

---

### Candidate Issue B: The same bug is tracked under different issue numbers in different repositories, and even a correctly-merged PR can silently link to the wrong one

This directly reinforces Member 1's **A4 ("Repository and issue-tracking fragmentation")** with a concrete example drawn from my own merged work, discovered while checking these links for this submission.

#### Current status

- Ready for group consideration
- Reviewed repository: `logseq/logseq` and `logseq/db-test`
- Reviewed commits: PR #13153, merged as [`211ccfa`](https://github.com/logseq/logseq/commit/211ccfae44ad4d527273e9caa11abae40809a3a3) (1 Sep 2026); PR #13156, merged as [`7eb7911`](https://github.com/logseq/logseq/commit/7eb791177763ca36b3dfad037bc52a2ffdf7c592) (7 Sep 2026)

#### Location and repository evidence

- Repositories: `logseq/logseq` (code and same-repo issues), `logseq/db-test` (the DB-line bug tracker)
- My PR: [#13153](https://github.com/logseq/logseq/pull/13153), "Fix UI #1135: redirect to previous page instead of home on page delete"
- Follow-up PR: [#13156](https://github.com/logseq/logseq/pull/13156), "fix: go to previous page after current page delete," by tiensonqin
- The two colliding issue numbers: [`logseq/logseq#1135`](https://github.com/logseq/logseq/issues/1135) ("Cannot embed images without extension" — unrelated) vs. [`logseq/db-test#1135`](https://github.com/logseq/db-test/issues/1135) ("[UI] After deleting a page, dont return the user to home, instead take them to the previous visited page" — the actual bug both PRs fix)

#### Current design

Bug reports for the DB line of Logseq are filed in a separate repository, `logseq/db-test`, while the fix code lives in `logseq/logseq`. GitHub's closing-keyword auto-linking ("Fixes #N" / "Closes #N") resolves `N` against the *same* repository the PR is opened in, unless the reference is explicitly repository-qualified (`owner/repo#N`).

#### Evidence

My own PR #13153 is titled "Fix UI #1135…" and its description says "Fixes #1135" with no repository qualifier. Read literally, that links to `logseq/logseq#1135` — "Cannot embed images without extension," a completely unrelated bug. The bug I was actually fixing is `logseq/db-test#1135`. Six days later, `tiensonqin`'s own follow-up PR #13156, fixing the same underlying behaviour, correctly wrote "Fixes db-test#1135" with the repository prefix. So on the very same bug, six days apart, one PR used the ambiguous form that resolves to the wrong issue and the other used the correct one.

#### Concrete developer consequence

Anyone using GitHub's "Development" sidebar on the issue, an issue search, or a changelog tool that trusts closing-keyword links, would see my PR as having closed an unrelated image-embedding bug in `logseq/logseq` rather than the DB-line delete-navigation bug it actually fixes. A contributor later searching "#1135" for prior work on delete-navigation would not reliably land on the right thread unless they already knew to add the `db-test` prefix.

#### Impact on users

Low direct impact — the fix itself behaves correctly regardless of which issue it's linked to — but it degrades the project's own record of what fixed what, which matters to users tracking whether a bug they reported is resolved, especially since `db-test` is where the project's own README tells users to report DB-line bugs in the first place.

#### Why this is architectural, not an isolated bug

This is not a mistake in my PR description alone. It is a structural consequence of splitting bug reports (`logseq/db-test`) from code (`logseq/logseq`) with no project-wide, enforced convention for cross-repository issue references — exactly the pattern Member 1's A4 describes. That it happened to me, and that the founder used the opposite (correct) convention for the very same bug days later, shows the inconsistency isn't about any one contributor being careless; nothing enforces the repository-qualified form.

#### Realistic improvement / migration direction (staged, moderate)

Not a repository merge. A staged first step: a CONTRIBUTING.md note plus a lightweight, advisory (non-blocking) CI check that flags a PR opened against `logseq/logseq` whose description contains a bare "Fixes #N" / "Closes #N" with no repository qualifier. A warning comment rather than a blocking check keeps this from slowing down the fast merge cadence the group's Governance Report already documents.

#### Expected impact

Future PRs get a visible nudge toward the `db-test#N` form where relevant, without changing how anyone reviews or merges code.

#### Cost and risk

- Implementation effort: low — a single CI Action, advisory rather than blocking.
- Compatibility or migration risk: none to production code.
- Testing requirements: check the Action doesn't fire on PRs that intentionally close a `logseq/logseq`-only issue.
- Possible disadvantages: some false positives are likely on genuinely same-repo closes; the check needs to stay advisory, not blocking, or it will annoy contributors for no benefit.

#### Maintainer awareness and position

I found no CONTRIBUTING.md guidance on cross-repository issue references. The inconsistency between my own PR and the founder's own follow-up PR six days later suggests this isn't a documented convention even internally — it's evidence of the gap, not a sign anyone is ignoring a known rule.

## 5. Candidate Positive Design Choices

### Positive Choice A: The worker-boundary fix extracts its logic into a small, independently unit-tested function

- Specific design decision: rather than patching the bug inline inside the `get-class-properties` handler, the fix pulls the closed-value-flattening logic into its own function purely so it can be unit tested in isolation.
- Repository and files: `logseq/logseq`, [PR #13201](https://github.com/logseq/logseq/pull/13201) (merged commit `381fab6`); test added at `src/test/frontend/worker/handler/property_test.cljs`
- Developer-level benefit: I could assert the exact shape of the data crossing the worker boundary — closed values and icons present, and the map shape unchanged for properties that have none — without rendering any UI or running the whole app. That made it possible to be confident the fix was correct beyond having clicked through it by hand.
- What would otherwise be difficult: testing this kind of worker-boundary behaviour any other way would mean an end-to-end test exercising the full tag-table render path — slower to write, slower to run, and harder to pin down to the actual data-shape problem rather than a rendering symptom of it.
- Limitations: nothing in the codebase requires worker-boundary logic to be written as a separately-tested pure function — it happened here because it was the natural way to fix this bug, not because of an enforced rule. The same silent-loss bug (Candidate A) still shipped once before anyone noticed, which shows the pattern is not applied consistently enough to prevent the class of bug it would help catch.

### Positive Choice B: A maintainer follow-up can build on a contributor's merged fix instead of replacing it

- Specific design decision: PR #13156 explicitly describes itself as unifying and extending PR #13153 — "`#13153` already replaced the hardcoded home redirect with `history.back`. This follow-up unifies that path, adds the empty-history fallback, stops mobile double-back on recycle, and adds tests" — rather than reverting or duplicating it.
- Repository and files: `logseq/logseq`, [PR #13153](https://github.com/logseq/logseq/pull/13153) (mine, merged `211ccfa`) and [PR #13156](https://github.com/logseq/logseq/pull/13156) (tiensonqin's follow-up, merged `7eb7911`), both touching `src/main/frontend/modules/outliner/pipeline.cljs`
- Developer-level benefit: my merged code stayed the foundation rather than being thrown away; the follow-up specifically called out that it "does not also go back" on mobile "(avoids a double pop after #13153)," meaning it was reviewed *against* my change rather than written blind.
- What would otherwise be difficult: without this kind of layered follow-up, edge cases found after a merge (here: no-history fallback, mobile double-pop) tend to either wait for the original contributor to come back, or get redone from scratch by whoever picks it up — both slower than a maintainer extending the existing code in place.
- Limitations: this is one instance, not a documented policy, and it sits alongside a different pattern already in the group's Governance Report where the founder closed a contributor's PR (#13008) after independently re-implementing the same fix (#13014). The two cases together suggest the outcome (build-on vs. replace) is currently a judgement call made case by case, not a consistent practice.

## 6. Suggested Priority (my view, for the group's Prioritization section)

| Candidate | Suggested priority | Impact | Cost | Risk | Dependencies or community constraints |
| --- | --- | --- | --- | --- | --- |
| A — worker-boundary virtual attributes | High, if merged into A3 | Medium: a second, independently-discovered occurrence of the same failure class strengthens the case that it is structural rather than incidental | Low (one test pattern + a short tracked list) | Low | Recommend merging into Member 1's A3 rather than opening a separate slot; group decision needed |
| B — cross-repository issue-reference collisions | Medium–High, if merged into A4 | Low-to-medium direct impact (the fixes themselves are correct) but real damage to traceability, which compounds every time it recurs | Low (one advisory CI check) | Low, provided the check stays advisory rather than blocking | Recommend as supporting evidence for Member 1's A4 |


- [x] I stated areas I did not examine.
- [x] Both candidates name the commits they were verified against.
- [x] I am comfortable being named as the verifier of claims about my reviewed areas, once the three items in Section 7 are resolved.
