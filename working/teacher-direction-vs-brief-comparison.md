# Teacher Direction vs. Assignment Brief — Comparison and Implications

> Purpose: compare the supervisor's weekly-meeting direction against the authoritative assignment brief, decide whether it goes **beyond** the PDF, **inside** it, or was **misread**, and work out what the group should change.
>
> Status: analysis document for the group. Not a report section.

## 0. What is being compared

| Tag | Source | Authority |
| --- | --- | --- |
| **A** | `assignment/COMPX574-Code-Review-Brief.pdf` (4 pages) | Authoritative assessment brief. Marking notes and weights live here. |
| **B** | Supervisor's "Project Report Requirements / Direction" (weekly meeting) | Guidance, framed as "useful directions include…" |
| **C** | Group's current interpretation: `README.md`, `templates/member-review-template.md`, `working/*`, `submissions/*` | The group's working plan |

Deadline check: "next Friday" from the meeting = **Friday 25 September 2026, 5:00 PM** (matches README). ~8 days out.

---

## 1. Bottom line

**B does not contradict A. B is a specialisation of one part of A — not a replacement for it, and not a superset of it.**

Three findings drive everything else:

1. **B is scoped to only ONE of A's four sections.** B talks exclusively about what the *Issues and Recommendations* section should contain (the 13-mark section). B says **nothing** about *Scope and Orientation*, *Positive Design Choices*, or *Prioritization and Synthesis*, and nothing about format/logistics. **A still fully governs those.** The single most dangerous misreading would be to restructure the whole report around B and drop the other three sections.

2. **On content, B does not exceed A — it narrows and concretises A.** Every target B names (integration layers, glue code, dual Markdown/DB versions, hard-to-extend models) maps onto an area A already lists (dependency management, module boundaries and coupling, public API design, configuration handling). B's contribution is **specificity**, not new scope.

3. **On ambition, B may exceed A in two places.** B adds a **user-impact** dimension and a **"migration"** framing, both of which sit awkwardly against A's explicit **"moderate scope"** instruction. This is the only place where following B literally could cost marks.

**Was the group's reading wrong?** Not wrong — *incomplete and mis-weighted*. The README's structure is faithful to A. But the group's evidence base is drifting toward exactly what both A and B warn against: per-member, per-PR, module-local findings.

---

## 2. Side-by-side mapping

| # | Theme | A — PDF brief | B — teacher direction | Verdict |
| --- | --- | --- | --- | --- |
| 1 | Anti-pattern | "This is not a pull-request review… Individual bugs are out of scope unless you can show that the bug is symptomatic of a structural pattern." | "It should not primarily list individual bug fixes, issues, or pull requests." | **Aligned** — B restates A's warning almost verbatim |
| 2 | Target of review | "critical evaluation of its **structure, design, and development practices**" | "**architectural or design decisions** … that make development, extension, maintenance, or onboarding more difficult" | **Narrowed** — B drops "development practices" |
| 3 | Section coverage | Four sections: Scope (2), Issues (13), Positive (5), Prioritization (5) | Speaks only to the Issues section | **B silent on 3 of 4 sections** — A governs |
| 4 | Issue anatomy | what it is + files/modules + concrete consequence for "developing, testing, extending, or maintaining" + recommendation + impact + cost/risk | "current design, evidence from the codebase or contribution experience, its impact on **developers and users**, and a realistic improvement or migration direction" | **Aligned + extended** (adds *users*) |
| 5 | Recommendation ambition | "**moderate scope**"; "this should be rewritten in X" is not an issue | "realistic improvement or **migration** direction" | **Tension** — migration reads larger than moderate |
| 6 | Integration / glue code | Generic: "dependency management and upgrade posture"; "module boundaries and coupling between components" | Specific: "repeated adapters, conversion code, or 'glue code'… brittle, difficult to maintain, or risk losing data during conversions" | **Aligned, concretised** |
| 7 | Markdown vs DB coexistence | Not named; reachable via coupling / public API / configuration | Named explicitly as a target, with version-confusion and bug-report impact | **Aligned, concretised** (highest scope risk) |
| 8 | Evidence type | "an experience you actually encountered"; "grounded in the areas in which you have been working" | "Use concrete developer experience as evidence" | **Aligned** |
| 9 | Maintainer awareness | Required: "check the project's issue tracker, roadmap, design documents, and contributing guidelines… engage with their stated position" | Not mentioned | **B silent — keep from A** |
| 10 | Positive Design Choices | Required: exactly **three**, "avoid the obvious" | Not mentioned | **B silent — keep from A** |
| 11 | Prioritization & Synthesis | Required: rank the five issues; "identify which… would be hardest to actually land in this particular community" | Not mentioned | **B silent — keep from A** |
| 12 | Format & logistics | 3–6 pages of text; first page = each member's full name, student ID, email; PDF via Moodle; one submitter; peer-weighted individual grades | Not mentioned | **B silent — keep from A** |
| 13 | Whole-system obligation | "must also engage with the project at a whole-system level" | Implied by "architectural" framing | **Aligned** |

Score: **8 aligned, 2 extensions, 1 narrowing, 1 tension, 4 areas where B is simply silent.**

---

## 3. The three genuine extensions (and why they matter)

### E1 — "impact on developers **and users**"
A frames the consequence test around developers: "concrete consequence for someone developing, testing, extending, or maintaining the project." B adds end users. A does not forbid this, but the 13 marks are weighted toward developer consequence.
**Implication:** keep developer consequence as the *primary* justification for every issue; use user impact as a *supporting* paragraph. Do not let a finding stand on user confusion alone.

### E2 — "migration direction" instead of "moderate recommendation"
A explicitly caps each issue at "moderate scope" and rules out rewrite proposals. "Migration" implies moving the project between architectures — potentially a multi-release, breaking change.
**Implication:** this is the sharpest conflict. The group must express migrations as *staged, moderate first steps* (e.g. "add cross-layer validation now; defer full unification"), and be explicit about cost and risk, or it will read as a rewrite proposal and lose marks.

### E3 — two concrete target areas
- **Integration / glue layers:** "repeated adapters, conversion code, or 'glue code'… brittle… or risk losing data during conversions."
- **Markdown vs DB coexistence:** two architectures, different download paths, unclear version distinction → user confusion, harder bug reports, higher maintenance.

A's suggested-areas list already covers the *space*; B tells the group which targets the marker cares about. This is the most actionable part of B.

**Fact check on E3 (useful, and it sharpens the report):** the dual-version split is live, not hypothetical. Logseq publicly announced on 24 April 2026 that it is splitting into two products — **Logseq OG** (file-based Markdown, moving to `github.com/logseq/og`) and **Logseq** (database graphs, staying in `github.com/logseq/logseq`, "the main version going forward"). The stated driver is exactly B's point: *"Every feature, bug fix, and UX change needs to be considered twice — often leading to regressions and confusion"* and *"difficult to support users, who are frequently confused by the differences between the two different architectures."* Resolution is by **product naming**, not version numbers, and there is no forced migration.
**Implication:** B's "version numbering does not clearly distinguish them" is directionally right but should be stated precisely as *product identity / download-path confusion* rather than *version-number confusion*, or the claim is easy to rebut. This also means the **reviewed-revision baseline now matters more** — `logseq/logseq` is the DB line, `logseq/og` is the Markdown line. The report must state which repo and revision it reviews.

### The one narrowing — N1
A says "structure, design, and development practices" and explicitly permits process/tooling issues: *"Issues and positive design choices can draw from the inclusion of required developer processes and tooling, or the lack thereof."* B says "architectural or design decisions," which reads narrower.
**Implication:** B's list is introduced with "**useful directions include**" — it is illustrative, not exclusive. A process/tooling finding (e.g. CI, release tooling, onboarding docs) is still fully admissible under A. Do not let B's phrasing push the group away from process findings it can actually evidence.

---

## 4. Where the group's current plan needs adjustment (C vs A+B)

| Observation from `C` | Risk against A/B | Fix |
| --- | --- | --- |
| Evidence base is three merged PRs by one member (mobile HTTPS, query test, editor refocus) | Both A and B warn against a PR/bug-centric report | Keep PRs in *Scope* only; convert at most one into a structural pattern |
| Three candidate issues are module-local (mobile config, delayed callbacks, query error recovery) | Weak coverage of B's architecture/integration/dual-version targets | Re-centre ≥2 of the five issues on integration layers / dual-version |
| `candidate-register.md` still empty for Members 2–4 | Cannot select five issues yet | Chase submissions; the 14 Sep milestone has slipped |
| README DoD: "Exactly five **moderate-scope** issues" | Collides with the dual-version theme, which is project-wide | Slice the dual-version theme into one or two moderate, evidence-backed issues |
| Member template has no field for user impact or migration direction | Misses E1/E2 | Add optional fields |
| No stated reviewed commit/revision | A requires claims to be "locatable"; the OG/DB split makes this critical | Fix and publish the baseline commit per repo |

---

## 5. Recommended actions

1. **Keep A as the contract.** Treat B as guidance for the *Issues and Recommendations* section only. Do **not** remove Scope, Positive Design Choices, Prioritization, the page limit, or the first-page ID block.
2. **Re-centre the issue set.** Aim for at least 2–3 of the five issues on B's architecture-level targets (integration/glue layers; Markdown/DB coexistence; hard-to-extend data models), each still moderate in scope.
3. **Slice, don't sprawl.** Express the dual-version theme as a moderate issue with a staged, non-breaking first step — not as "two architectures should be merged."
4. **Add two fields** to the member template: *impact on users* (supporting) and *realistic improvement / migration direction* (staged).
5. **Keep the maintainer-awareness check.** A requires it; B is silent on it. Cite the April 2026 split announcement and the relevant tracker threads.
6. **Pin the baseline.** State the reviewed repo(s) and commit(s), acknowledging `logseq/logseq` = DB line and `logseq/og` = Markdown line.
7. **Don't drop the judgement section.** Ranking the five issues and naming the hardest-to-land recommendation is 5 marks and is untouched by B.
8. **Convert the PRs into evidence, not content.** Use the three merged PRs to prove familiarity in *Scope*, and at most one as the experiential seed of a structural finding.

---

## 6. Questions worth putting back to the supervisor

1. Does the direction replace the whole brief, or only steer the *Issues and Recommendations* section? (Our reading: the latter.)
2. For the Markdown/DB theme — is one consolidated issue acceptable, or must it be split to respect the brief's "moderate scope"?
3. Should user impact *supplement* developer consequence, or can a finding rest on user confusion alone?
4. Which revision/branch is the intended review baseline now that the OG/DB split has landed?

---

## 7. One-line answer to the original question

> The teacher's direction is **not beyond the PDF** in content — it is a **concretised, narrowed specialisation of the PDF's "Issues and Recommendations" section**. It **extends** the PDF in two ways (user impact; migration framing) and **narrows** it in one (architectural/design vs. the PDF's broader "structure, design, and development practices"). The group's understanding was not wrong, but it is **under-weighting architecture-level findings and over-weighting per-PR evidence**. The real risk is not "teacher vs PDF" — it is forgetting that **three of the four report sections are still governed entirely by the PDF**.

---

## 8. Update — actions taken, 17 September 2026

This comparison was acted on the same day:

| File | Change |
| --- | --- |
| `README.md` | Rewrote the report direction around architecture-level findings; added the situation note, the multi-repository map, the five issue themes (A1-A5), the four positive-choice themes (P1-P4), a corrected internal schedule, and a stricter Definition of Done. |
| `templates/member-review-template.md` | Added a user-impact field, a staged "migration / improvement direction" field, an explicit "why this is architectural, not an isolated bug" field, a reviewed-commit field, and a repository column. |
| `submissions/member-1-arthur-gao.md` | Rewritten. Now records all **five** contributions (the earlier version listed three and omitted the merged #13071 and the #1179 contribution, whose [PR #13296](https://github.com/logseq/logseq/pull/13296) was later closed as a duplicate of maintainer [#13287](https://github.com/logseq/logseq/pull/13287)), and replaces the three module-local candidates with three architecture-level candidates. |
| `working/candidate-register.md` | Rebuilt around architecture-level candidates; old candidates marked superseded, merged, or rejected, with reasons. |
| `working/selection-decisions.md` | Added a pre-selection decisions block (the A/B duplicate question, the scope slice for the dual-architecture theme, and the three positive choices). |
| `report/final-report.md` | Skeleton updated to require the seven-part issue structure and to name the maintainers' surface-area-reduction tension. |

Two factual corrections made during this work, both of which the report must respect:

1. **`db-test` is a repository for bug reports, not a database technology.** The DB version stores data in SQLite via `sqlite-wasm`. The original version is file-based Markdown. Do not write "SQLite to DBTest".
2. **The Markdown/DB distinction is product identity and download path, not version number.** The 24 April 2026 announcement names the products "Logseq OG" and "Logseq" and gives no version numbering scheme.

A third finding came from reading the local Logseq checkout rather than the web. The root `AGENTS.md` contains a section "Error handling and compatibility" that states the project's failure-mode policy in writing — "Prefer fail-fast over fallback", "Do not silently recover from programmer errors", "Do not introduce default values to mask invalid state", "Keep one clear code path whenever possible", "Internal code may assume well-formed inputs from controlled callers" — and `cli/AGENTS.md` states that the CLI repository "does not include the db-worker-node server, and should use the existing cljs version". This changes the framing of two candidates: the silent-failure finding is a **policy-to-practice gap** (the rule exists and is not applied), not a missing policy; and the CLI/worker contract finding has direct written evidence that the seam is deliberately split across two languages with well-formedness delegated to the caller. Both are quoted verbatim in `submissions/member-1-arthur-gao.md`.

A fourth point, on process: Member 1 is submitting candidates as a **pool**, not a pre-selected set. Whether the CLI/worker-contract candidate and the silent-failure candidate merge, and which candidates survive at all, are group decisions taken after all four member submissions arrive. Nothing in this repository should pre-empt that.
