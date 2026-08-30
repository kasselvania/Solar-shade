# Contributing

## Before work

Read:

1. `AGENTS.md`
2. `CURRENT_SLICE.md`
3. `ARCHITECTURE.md`

Then read the exact supporting dossier, ADR, research record, and evidence package required by the slice.

## Branches

Use one branch per bounded slice or repair:

```text
slice/s0-reference-characterization
feature/<bounded-claim>
fix/<exact-failure>
docs/<bounded-governance-change>
```

## Commits

Commits should describe the fact or boundary changed.

Avoid mixing:

- governing design and implementation;
- mechanical and fleet work;
- component research and qualification claims;
- raw evidence and unrelated cleanup;
- multiple uncertainty domains.

## Pull requests

A pull request must state:

- basis commit;
- primary claim;
- exact changed paths;
- fixture/revisions;
- acceptance evidence;
- tests;
- safety review;
- energy/resource consequences;
- explicit non-goals;
- known remaining risks.

Use the pull-request template.

## Hardware changes

Include, as applicable:

- schematic/CAD revision;
- exact component and ratings;
- voltage/current limits;
- assembly state;
- photographs;
- test procedure;
- rollback or safe disassembly;
- battery and chain safety review.

## Firmware changes

Include:

- toolchain/SDK;
- reproducible commands;
- state transitions;
- bounds;
- failure behavior;
- hardware-safe defaults;
- unit and hardware-in-loop results;
- effect on sleep and daily energy.

## Documentation changes

Design amendments are explicit. Do not change a dossier merely to match convenient implementation.

Status changes cite evidence.

Source updates include access date and invalidated calculations.

## Review

Review the exact diff and evidence. Agent summaries are not the implementation.

Do not merge because a nearby idea is useful. Keep it for a later slice.
