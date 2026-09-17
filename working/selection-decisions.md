# Selection Decisions

Complete this file after all four submissions have been cross-reviewed.

> **Direction note (17 September 2026).** The five selected issues must be **architecture-level or design findings** with moderate, staged recommendations. The three positive design choices must come from specific decisions, not from observations like "it has tests". See the README's "Report Direction" section.

## Evidence Baseline

- Reviewed repositories:
- Reviewed commits or revisions:
- Date selected:
- Members present:

## Pre-selection Decisions

These are **group decisions**, taken at the selection meeting **after all four member submissions are in**. No single member resolves them in advance, and Member 1 has deliberately submitted candidates as a pool rather than a pre-selected set. Record the outcome here with a short reason for each.

| # | Question | Decision | Reason |
| --- | --- | --- | --- |
| 1 | Are I-A-A (implicit CLI/worker contract) and I-A-B (silent failure) one root cause or two? | Open — group meeting after all submissions | They share a seam. Selecting both without a decision spends two slots on one structural cause; merging frees a slot for another member's finding. |
| 2 | Which slice of I-A-C (two coexisting architectures) is admissible at moderate scope? | Open — group meeting after all submissions | The full theme is project-wide and would read as a rewrite proposal, which the brief excludes. |
| 3 | Which three positive design choices? | Open — group meeting after all submissions | Four candidates exist from Member 1 alone, plus Member 2-4 candidates. The group needs exactly three, preferably from different subsystems. |
| 4 | Which candidates from Members 2-4 survive alongside Member 1's pool? | Open — requires their submissions | Members 2-4 have not submitted. The five slots cannot be allocated before their candidates exist. |

## Five Selected Issues

| Rank | Candidate ID | Final issue title | Repository | Commit | Why selected | Evidence owner | Remaining verification |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | | | | | | | |
| 2 | | | | | | | |
| 3 | | | | | | | |
| 4 | | | | | | | |
| 5 | | | | | | | |

## Three Selected Positive Design Choices

| Candidate ID | Final title | Repository | Why selected | Evidence owner | Remaining verification |
| --- | --- | --- | --- | --- | --- |
| | | | | | |
| | | | | | |
| | | | | | |

## Rejected or Merged Candidates

| Candidate ID | Decision | Reason |
| --- | --- | --- |
| | Rejected / merged into another finding / needs more evidence | |

## Prioritization Rationale

For each selected issue, compare impact, implementation cost, risk, dependencies, maintainer capacity, backward compatibility, and contributor expectations. **Do not repeat the issue descriptions** — this section is marked on judgement, not coverage.

Consider in particular:

- which findings the maintainers have already acted on (the product split) versus which are unrecognised (the silent-failure pattern);
- which recommendations change visible behaviour for users or automation;
- which findings depend on other findings.

## Hardest Recommendation to Land

- Recommendation:
- Technical difficulty:
- Social or process constraint:
- Backward-compatibility or contributor constraint:
- Why a technically correct recommendation may still be impractical:

The strongest candidate is likely a recommendation that asks maintainers to add a contract, a failure mode, or a documented boundary **at a time when they are deliberately reducing surface area** — the April 2026 split exists precisely to stop maintaining two of things. A recommendation that adds a new obligation must explain why it does not work against that direction.

## Group Approval

- [ ] Member 1 approved the selection and ranking.
- [ ] Member 2 approved the selection and ranking.
- [ ] Member 3 approved the selection and ranking.
- [ ] Member 4 approved the selection and ranking.
