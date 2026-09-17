# Member Review Submission

> Copy this template into your assigned file in `submissions/`. Replace every placeholder. Do not treat another member's completed submission as the template.

> **Direction note (17 September 2026).** The report is about **architectural and design decisions that make Logseq harder to develop, extend, maintain, or onboard into** — not a list of bugs, issues, or pull requests. A candidate that is only an isolated bug is not a valid finding. See the README's "Report Direction" section and `working/teacher-direction-vs-brief-comparison.md`.

## 1. Reviewer Information

- Member number:
- Full name:
- Student ID:
- University email:
- GitHub username:
- Submission status: Draft

## 2. Contribution and Familiarity Evidence

List relevant contributions that establish which parts of Logseq you know well. Contributions are evidence of familiarity; they are not automatically report issues. **Name the repository for every entry** — Logseq work spans several repositories and issue numbers change when a report is transferred.

| Type | Repository | Reference | Status | Your work | Areas learned |
| --- | --- | --- | --- | --- | --- |
| Issue/PR/commit | `logseq/logseq` or `logseq/db-test` or `logseq/og` | Link | Open/closed/merged | Brief description | Modules, tools, or workflows |

## 3. Review Scope

### Areas examined closely

For each area, state what you read, changed, tested, or traced.

| Area | Repository | Files/modules/workflows | Evidence | Depth of review |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

### Whole-system material examined

Examples include high-level architecture, the relationship between the Markdown and DB lines, public APIs, build configuration, CI, release tooling, dependency management, documentation, and contribution workflow.

- Add at least one whole-system artifact or state that none was examined.

### Areas not examined

| Area | Reason it was not examined |
| --- | --- |
|  |  |

## 4. Candidate Issues

Submit **two or three candidates**. A candidate must describe an **architectural or design decision** that makes development, extension, maintenance, or onboarding harder, and that still matters on the reviewed revision. It must not be an isolated bug, a style preference, a feature request, or a proposal to rewrite the project.

### Candidate Issue A: Title

#### Current status

- Ready for group consideration / Requires more evidence / Historical issue only
- Reviewed repository:
- Reviewed commit or revision:

#### Location and repository evidence

- Repository and layer (e.g. frontend, DB worker, CLI, build tooling, docs):
- Files, modules, packages, or subsystems:
- Permanent code links (pinned to a commit):
- Related issue, roadmap, design-document, or contributing-guide links:

#### Current design

Describe the architectural or design decision as it exists now. State the decision, not just the symptom.

#### Evidence

Give the code or contribution evidence that the decision is real and current. Include at least one link pinned to a commit.

#### Concrete developer consequence

Explain how it affects someone developing, testing, extending, or maintaining Logseq. Include an experience you actually encountered where possible.

#### Impact on users (where relevant)

Explain any user-visible consequence: confusion, lost or invisible data, silent wrong results, or extra support burden. If there is no meaningful user impact, say so explicitly.

#### Why this is architectural, not an isolated bug

Explain why this is more than a single defect: what repeated pattern, missing contract, or structural coupling it reveals. If you cannot argue this convincingly, the candidate is not ready.

#### Recommendation: a staged, moderate first step

Describe an actionable change of **moderate scope** — the first step, not the end state. If the full solution is a migration, say what the staged first step is and what it deliberately defers.

#### Expected impact

Explain what should become easier, safer, or more reliable.

#### Cost and risk

- Implementation effort:
- Compatibility or migration risk:
- Testing requirements:
- Possible disadvantages:

#### Maintainer awareness and position

State whether maintainers already know about this issue. Link the relevant discussion and engage with their stated position. For anything touching the Markdown/DB split, engage with the April 2026 product-split announcement.

### Candidate Issue B: Title

Repeat all fields from Candidate Issue A.

### Candidate Issue C: Title

Optional. Repeat all fields from Candidate Issue A.

## 5. Candidate Positive Design Choices

Submit one or two candidates. Identify a **specific design decision** and explain what it makes easier that would otherwise be difficult. Avoid the obvious: "it has tests" or "it uses a widely adopted framework" is an observation, not an analysis.

### Positive Choice A: Title

- Specific design decision:
- Repository and files/modules/workflow:
- Permanent repository links (pinned to a commit):
- Developer-level benefit:
- What would otherwise be difficult:
- Evidence from your development experience:
- Limitations or boundaries of the claim:

### Positive Choice B: Title

Optional. Repeat all fields from Positive Choice A.

## 6. Suggested Priority

If any of your candidate issues are selected, explain their likely priority within one release cycle.

| Candidate | Suggested priority | Impact | Cost | Risk | Dependencies or community constraints |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## 7. Questions for the Group

- Possible duplicates with another member's finding:
- Evidence that still needs validation:
- Decisions requiring group discussion:

## 8. Member Verification

- [ ] I personally checked every link in this submission, including which repository each belongs to.
- [ ] Every claim names the commit or revision it was verified against.
- [ ] I distinguished current findings from bugs that have already been fixed.
- [ ] I stated areas I did not examine.
- [ ] Each candidate is an architectural or design finding, not an isolated bug, style preference, feature request, or rewrite proposal.
- [ ] Each recommendation is a moderate, staged first step with cost and risk.
- [ ] I identified uncertainty and platform limitations.
- [ ] I am comfortable being named as the verifier of claims about my reviewed areas.
