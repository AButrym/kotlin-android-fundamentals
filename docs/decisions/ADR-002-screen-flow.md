# ADR-002: Screen flow and screen modes

Status: Accepted
Date: 2026-10-29
Decision IDs: D-03, D-10
Resolves review findings: F-08 in `reviews/initial-curriculum-consistency-review.md`
Amends: course contract §8.3, §19; course map L11, L12, milestone M-D

## Context

The contract limits navigation to what the selected final project requires (§8.3) and leaves open whether a navigation library is used (§19). The course map used the terms `route-vs-screen-intro` and `navigation-boundary-intro`, which suggest Navigation Compose, while deferring only "complex" navigation graphs. Milestone M-D and the L12 lab also required a setup state for every project, which exceeds the contract's minimum capstone (§10.2).

## Decision

### Screen flow (D-03)

- The core path uses **explicit screen state**. Navigation Compose is not used.
- The application has a finite set of screen modes, for example: setup, playing, result, and handover (only where a game needs it).
- Screen mode is modeled as an `enum` or a `sealed` hierarchy and rendered with an exhaustive `when`.
- The term **route** is removed from the core curriculum. Navigation Compose may appear only in a clearly labeled optional deep dive.

### Which modes are required (D-10)

| Mode | Requirement |
|---|---|
| playing | required for every project |
| result (or equivalent terminal-state UI) | required for every project |
| setup | optional; only when the game needs configuration |
| handover | only for hot-seat games with hidden information |

- The guided Tic-Tac-Toe reference shows **playing** and **result**.
- L12 demonstrates the screen-mode **pattern**. It does not require every mode from every project.

## Rationale

- Keeps screen switching an application of already-taught concepts (sealed types, exhaustive `when`, state-down/events-up) rather than a new framework.
- Matches contract §8.3 ("only to the extent required") and §10.2 (minimum capstone).
- Handover has a real purpose only when hidden information must not be shown to the next player.

## Consequences

- Course map: rename/remove `route-vs-screen-intro` and `navigation-boundary-intro`; make `Setup or start state` and `Handover state` in the L12 lab conditional; change M-D evidence from "supports setup, play, and result states" to the playing/result minimum plus the pattern explanation.
- Dependency map: `screen-flow`, `screen-mode`, `application-ui-state` stay, defined as explicit-state concepts; `advanced-navigation` stays deferred.
- The relation between the game-level UI state and the screen mode is defined in ADR-003.
- L12 needs a demonstration that is not Tic-Tac-Toe-specific for handover (the guided baseline has no hidden information); the form of this demonstration is left to the L12 lesson specification.
