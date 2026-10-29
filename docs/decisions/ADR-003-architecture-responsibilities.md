# ADR-003: Architecture responsibilities and evolution order

Status: Accepted
Date: 2026-10-29
Decision IDs: D-08
Resolves review findings: F-16, F-17 in `reviews/initial-curriculum-consistency-review.md`
Amends: course contract §11.1, §11.5; course map L05-L12

## Context

Contract §11.5 listed the architectural evolution as: local UI behavior, modeled data, lifted state, state-driven screen, extracted game logic, ViewModel, reusable components. The course map extracts pure game rules (L09) before state hoisting (L10). The review found the two orders inconsistent, and found that the roles of game rules, `GameUiState`, screen mode and ViewModel were not defined consistently across L09-L12.

## Decision

### Evolution order (D-08)

Contract §11.5 becomes **normative in content** and is replaced by this pedagogically causal sequence:

1. local UI behavior;
2. reusable visual components;
3. modeled domain data;
4. board as a collection;
5. local observable state;
6. explicit game rules;
7. immutable state transition;
8. state hoisting and a screen-level source of truth;
9. ViewModel-managed state;
10. application screen modes;
11. package-by-feature organization;
12. capstone adaptation and refactoring.

Extracting pure game rules in L09, before the ViewModel in L11, is intentional and stays. Each step must be introduced as a refactoring that solves a problem the learner has just experienced (contract §6.4, §11.5).

### Responsibilities

These definitions restate contract §11.1 and apply to every lesson and to the reference implementation.

| Part | Responsibility | Must not |
|---|---|---|
| Model | Immutable data describing cells, players, board, game status | know about Compose, ViewModel or screen modes |
| Game rules | Pure functions: validate a move, apply it to produce a new board/state, derive status (win, draw, terminal) | read or write UI state; depend on Compose or Android types |
| UI state | Immutable data required to render the current game screen | contain behavior or framework objects |
| Screen mode | Which top-level view the application shows (ADR-002) | own game data that belongs to UI state |
| ViewModel | Owns the state (ADR-001), receives events, calls game rules, publishes the new immutable state | build UI or depend on composables |
| Screen | Reads state from the ViewModel, passes data and callbacks down, forwards events up | hold a duplicate copy of ViewModel-owned state; contain game rules |
| Components | Render parts of the UI from the data and callbacks they receive | own board state or call the ViewModel directly |

### Stage ownership

- L09-L10: rules are pure functions callable independently of any composable. State lives at screen level (hoisted) in the Screen composable.
- L11: state and event handling move into the ViewModel. Rules stay in the pure functions; the ViewModel calls them.
- The ViewModel never re-implements rules that already exist as pure functions.

## Consequences

- Contract §11.5 must be rewritten with the sequence above (minor version bump; ordering change with architectural effect, see contract §18.3).
- Course map L11 challenge "Extract a standalone GameEngine" becomes redundant with the L09 outcome and must be removed or reworded.
- The relation between game-level UI state (`GameUiState`, L10) and screen mode (L12) must be fixed in the reference design; until then lesson specifications must not assume nesting in either direction.

## Open items

- **Naming of the rules layer.** The contract uses `GameRules`, the course map uses "game engine". A single term must be chosen for all artifacts. This ADR does not decide it.
- **`GameUiState` vs screen mode.** Whether screen mode is a field of the UI state or a wrapper around it is not decided here.
- **Contract §11.3 file layout.** The `Player.kt` entry does not fit puzzle-style projects (ADR-004); the layout is "equivalent structure" and may be adapted per project profile.
