# ADR-005: Board representation, layout and update scaffold

Status: Accepted
Date: 2026-10-29
Decision IDs: D-05
Resolves review findings: F-06, F-24 in `reviews/initial-curriculum-consistency-review.md`
Amends: course map L03, L06, L07, L08

## Context

L06 declared `List<Cell>` as the single board representation, while its challenge compared flat and nested lists. The board layout technique (nested `Row`/`Column` from L03 vs `LazyVerticalGrid` from L06) was not fixed. L07 requires an interactive board, but the tools to update an immutable list (`map`, `copy`) arrive only in L08, so the L07 update mechanism was undefined.

## Decision

### Representation

- Canonical board representation: a **flat immutable `List<Cell>`**.
- Each cell carries `row` and `column`, or a stable `position`/index.
- Board updates use **immutable replacement**: a new list is produced; the old one is not mutated.

### Layout

- The base board is rendered as **nested `Row` inside `Column`**.
- `LazyVerticalGrid` is demonstrated in L06 as a general layout for larger collections. It is **not required** for Tic-Tac-Toe or any other core project.

### Update scaffold for L07

- L07 provides a **named scaffold**: `updateCellForMove(...)`.
- Learners **use** this function in L07 and are not expected to implement it.
- In L08 the implementation is opened, explained, and rewritten by learners with `map` and `copy`.

## Rationale

- One representation across L06-L14 keeps rules, UI state and tests simple.
- Nested `Row`/`Column` reuses L03 knowledge and avoids lazy-layout concerns for a fixed small grid.
- The scaffold gives L07 an interactive board without teaching L08 content early (contract §15.2, "provide the construct as fixed boilerplate").

## Consequences

- Course map: L06 homework challenge comparing flat vs nested lists stays optional and does not change the canonical choice; L07 gets an explicit scaffold entry; L08 gets an outcome "reimplement the scaffold with `map` and `copy`".
- Dependency map: add the scaffold as a named entry (not a taught concept) so that GV-05 and CDM-05 are satisfied.
- Variants whose natural model is not a flat grid (for example Memory card collections) may adapt the structure, but must keep immutable replacement.
- The interim representation of cell contents in L05-L06 (before nullable types and enums in L07) is **not** decided here; see the open items.

## Open items

- Interim representation of cell contents in L05-L06 and the refactoring to `Player?` in L07 (review finding F-05).
- The L06 sandbox task "find a cell by position using a loop" must avoid a "not found" result before nullable types are taught, or define how it is handled.
