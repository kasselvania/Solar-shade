# Architecture Decision Records

Use an ADR for a durable technical choice that should survive individual slices, such as:

- motor family after fixture proof;
- battery chemistry;
- power-path topology;
- position-reference strategy;
- local transport;
- firmware framework;
- repository split;
- licensing boundary;
- update and recovery strategy.

Do not use an ADR to authorize implementation. `CURRENT_SLICE.md` remains the live authority.

## States

```text
PROPOSED
ACCEPTED
SUPERSEDED
REJECTED
```

## Naming

```text
ADR-0001-short-title.md
```

Use [`ADR_TEMPLATE.md`](ADR_TEMPLATE.md).

## Required discipline

An ADR must distinguish:

- operator/product decision;
- published component facts;
- measured fixture facts;
- derived calculations;
- remaining unknowns.

An accepted ADR does not turn a hypothesis into evidence.

When superseding an ADR, link both directions and identify which implementation/evidence is invalidated.
