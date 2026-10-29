# ADR-001: Compose state syntax and ViewModel state API

Status: Accepted
Date: 2026-10-29
Decision IDs: D-01, D-02
Resolves review findings: F-04, F-07 (and part of F-03) in `reviews/initial-curriculum-consistency-review.md`
Amends: course contract §8.3, §9.1, §19; course map L01-L07, L11, L14

## Context

The course teaches state-driven Compose from L01 (a counter) but formally introduces `remember` and `mutableStateOf` only in L07. The review found two hidden dependencies:

1. The common `var x by remember { mutableStateOf(...) }` form uses a property delegate. The contract excludes delegated properties as a language topic (§9.1).
2. The L11 ViewModel state API was left open (`collect-or-read-viewmodel-state`, "StateFlow unless selected by implementation baseline"). A `StateFlow` choice would silently require coroutines and Flow, which are not required outcomes.

## Decision

### Compose state syntax (D-02)

- The core path uses explicit `.value` reads and writes. The `by` delegate syntax is not used anywhere in the core path.
- L01-L06: state code is provided as **named scaffold** written with `.value`. Learners may modify values and behavior but are not expected to understand `remember` or `mutableStateOf` yet.
- L07: `remember`, `mutableStateOf`, reading `.value` and writing `.value` are explained and practiced formally.
- L14: `by` may be shown as an **optional** idiomatic shortcut. The delegation mechanism is not explained. It must not appear in required exercises or assessment.

### ViewModel state (D-01)

- The core path uses **Compose snapshot state inside the ViewModel**. `StateFlow`, `Flow`, coroutines, `collectAsState` and `collectAsStateWithLifecycle` are not part of the core path.
- Pattern:
  - the ViewModel holds a private mutable state object;
  - it exposes a public read-only `State<T>`;
  - the Screen reads the state value;
  - the ViewModel changes state only by assigning a new immutable value built with `copy` (or the game's immutable transition functions).
- `viewModel()` is presented as a framework-provided acquisition function. The required Gradle dependency is supplied as ready-made configuration, with a short explanation that a dependency makes an API available to the project.
- `StateFlow` is allowed only as an **optional post-course extension**, clearly labeled and never a prerequisite.

## Rationale

- Removes the hidden delegation dependency without changing lesson order.
- Gives a direct conceptual path from local `remember` state (L07) to hoisted state (L10) to ViewModel-owned state (L11), keeping attention on source of truth and unidirectional data flow.
- Honors contract §9.1 (no required coroutines/Flow) and the minimum-infrastructure principle (§6.8).

## Consequences

- Course map: remove the "StateFlow unless selected by implementation baseline" clause from L11 deferrals; replace `collect-or-read-viewmodel-state` with a concept describing reading `State<T>` in a Screen.
- Dependency map: add the missing state concepts (`state-read`, `state-write`) and the L01-L06 scaffold entries; keep `delegation` and `stateflow` deferred (CDM-03, CDM-07 stay valid).
- Contract §19: the ViewModel/state part of the open decisions is closed.
- Any generated material that uses `by` or `StateFlow` in core code is non-conformant.
- `viewmodel-lifecycle-intro` (L11) must be limited to a user-level statement of what a ViewModel is for; Activity lifecycle internals stay out of scope (contract §9.2).

## Open items

- Exact public type exposed by the ViewModel (`State<T>` vs a type alias) is an implementation detail to settle when the reference implementation is written.
