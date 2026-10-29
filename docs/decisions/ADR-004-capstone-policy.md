# ADR-004: Capstone policy

Status: Accepted
Date: 2026-10-29
Decision IDs: D-04
Resolves review findings: F-14, F-15 in `reviews/initial-curriculum-consistency-review.md`
Amends: course contract §10.3, §10.4, §19; course map `final_project`

## Context

The course map used Tic-Tac-Toe both as the shared baseline and as a basic capstone variant, and its variant tiers (basic / intermediate / advanced) disagreed with the contract's standard / advanced lists. A learner could submit the shared guided example as their own capstone, which conflicts with contract §13.5 (no copied solution).

## Decision

The shared guided example and the learner's capstone are **separate artifacts**.

### Shared guided project

- Tic-Tac-Toe.
- Built progressively across L07-L12.
- Serves as the architectural **reference implementation**.

### Own capstone

Tic-Tac-Toe is **not accepted** without substantial extension. Acceptable extensions include:

- configurable board size;
- configurable win length;
- score across rounds;
- an alternative rule set;
- undo or move history;
- another agreed extension that requires the learner's own modeling.

### Tiers

**Core**

- Fifteen Puzzle, with a ready-made valid shuffle or a simpler shuffling scheme;
- Connect Four;
- simplified Memory without any coroutine delay;
- another agreed game of comparable complexity.

**Advanced**

- Reversi;
- simplified Checkers;
- hot-seat Battleship;
- extended Tic-Tac-Toe.

### Memory constraint

Core Memory must not require a delay. A mismatched pair closes after an explicit user action, for example a **Continue** button.

## Rationale

- Keeps the shared example a teaching reference and the capstone a test of transfer.
- Fixes the tier lists in one place and removes the contract/course-map disagreement.
- Avoids a hidden dependency on coroutines (`delay`), which are outside the course (contract §9.1).

## Consequences

- Course map: remove Tic-Tac-Toe from `suggested_variants.basic`; replace the three-tier list with core / advanced; remove the "Simplified Reversi" intermediate entry.
- Contract §10.3-§10.4 are rewritten to the core / advanced lists above.
- Different project types (puzzle without players or turns, open turn-based game, hot-seat game with hidden information) need their own minimum feature list; the form is defined in the lesson specifications for L12-L13. Notably, a puzzle has no `Player` and no turns, so the `Player.kt` entry in contract §11.3 does not apply to it.
- The baseline timeline in the course map (`introduced_in` L06, completed L09, architecture completed L12) must be reconciled with "built in L07-L12" above; this ADR does not choose the exact lesson boundaries.
- Project selection and scope approval happen before L13 (see ADR-006, checkpoint "Final Project Readiness").
- Contract §19 item "whether the core capstone is chosen by each learner or standardized for the cohort" is resolved as: each learner chooses within the tiers, with instructor approval of scope.

## Open items

- Minimum feature lists per project profile.
- Criteria that make an extension of Tic-Tac-Toe "substantial" enough.
