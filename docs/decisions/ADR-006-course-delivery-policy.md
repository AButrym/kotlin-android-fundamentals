# ADR-006: Course delivery policy

Status: Accepted
Date: 2026-10-29
Decision IDs: D-06, D-07, D-09, D-11, D-12
Resolves review findings: F-09, F-10 (partly), F-11, F-12, F-18, F-19, F-21 in `reviews/initial-curriculum-consistency-review.md`
Amends: course contract §4.2, §8.4, §12, §13.3, §14, §18, §19

This ADR groups delivery decisions that are not tied to a single technical topic: homework, checkpoints, tooling progression, testing policy, and canonical locations.

## 1. Tooling progression (D-06)

Git, Gradle, IDE use and stack-trace reading are integrated as a cross-cutting **tooling progression**, not as a separate lesson. These items are recorded as `tooling_skills`, **not** as programming concepts, in the curriculum models.

| Lesson | Tooling skills |
|---|---|
| L01 | build/run cycle, Project view, editor, Run, Logcat, first compiler message |
| L02 | code completion, parameter hints, formatting |
| L03 | breakpoints, step over, variable inspection |
| L04 | rename refactoring, go to declaration |
| L06 | runtime exception; reading the main parts of a stack trace |
| L08 | dependency concept at user level |
| L11 | ready-made addition of the ViewModel dependency |
| L12 | Git status, add, commit |
| L13 | branch/checkpoint workflow |
| L14 | regression check; final commit/tag if needed |

Git and Gradle remain integrated tools, not theory-heavy goals (contract §8.4).

## 2. Homework policy (D-07)

Global rule: **homework must never be the only path to the prerequisites of the next lesson.**

- Core completion happens during the lesson.
- Homework provides reinforcement or extension.
- If a later lesson uses a homework result, a **starter checkpoint** is provided.
- A learner can continue the course without having completed homework.
- The project proposal is prepared before L13, and the **template and time for a basic choice** are provided at the end of L12.

This rule is a Core/Practice/Challenge classification constraint (contract §12.2) for every lesson specification. The review items where homework carried next-lesson prerequisites (L05, L07, L08, L12, L13) must be reworked under this rule.

## 3. Testing policy (D-09)

- Unit tests are **not** a mandatory learner outcome.
- The main validation path is deterministic sandbox checks and manual test scenarios.
- One **instructor-led** unit test for a pure rule function may be shown as a preview in L09 or L11.
- Writing a full set of unit tests is a **Challenge**.
- Assessment does not depend on learners setting up test infrastructure.
- `unit-test-intro` must not be a prerequisite for core progression.

This closes the contract §19 item on automated test coverage expectations. Course map references to tests in L09 homework and the L11 challenge must follow this policy.

## 4. Checkpoints (D-11)

A fifth checkpoint, **Final Project Readiness**, is placed at the **end of L12**, completing the five stages required by contract §13.3.

It checks:

- approved project profile;
- bounded feature list;
- board/state model draft;
- screen modes;
- rule list;
- component breakdown;
- scope risks;
- starter repository checkpoint;
- the learner's ability to explain how the guided architecture transfers to their own game.

L13 does **not** begin with project selection; it begins with implementing the approved plan. Milestone M-D (after L12) and this checkpoint are both located at the end of L12, so the assessment map must define how they relate (one combined event or two ordered parts); this ADR does not decide that.

## 5. Canonical locations and identifiers (D-12)

### Locations

- `docs/` holds human-readable governance and decisions.
- `curriculum/` holds machine-readable curriculum models.
- The numbered initial files in `docs/` were bootstrap artifacts and are replaced by canonical paths after reconciliation.

| Canonical path | Replaces |
|---|---|
| `docs/course-contract.md` | `docs/01-course-contract.md` |
| `curriculum/course-map.yaml` | `docs/02-course-map.yaml` |
| `curriculum/dependency-map.yaml` | `docs/03-concept-dependency-map.yaml` |
| `curriculum/assessment-map.yaml` | (new) |
| `docs/decisions/ADR-001` to `ADR-006` | (this decision set) |

After the move, the numbered files are either deleted or reduced to short migration notices. **No two active copies may remain.**

### Identifiers and language

- Course ID everywhere: `kotlin-android-fundamentals` (replaces `kaf`).
- Canonical artifacts are written in **English**.
- The default learner-facing language is **Ukrainian**.
- Delivery language is not part of curriculum semantics. `language_of_instruction: uk` moves to a separate delivery profile.
- Stable outcome IDs must be assigned to the contract's learning outcomes (contract §18.1) so that coverage can be traced from outcome to lesson without reviewer-assigned labels.

## Naming note

The author's decision list names this file `ADR-006-course-delivery-policy.md`, while the canonical path list in D-12 names it `ADR-006-homework-and-checkpoints.md`. The first name is used; if the second is preferred, the file must be renamed and references updated.

## Consequences

- Contract §4.2 and §8.4 must reference the tooling progression; contract §13.3 gains the fifth checkpoint; contract §19 loses the items on homework volume and test coverage.
- The dependency map needs a `tooling_skills` structure separate from `concepts`.
- Moving and renaming canonical files is a separate task and must update every `depends_on` reference.
- The grading weights and hosting items in contract §19 remain open.
