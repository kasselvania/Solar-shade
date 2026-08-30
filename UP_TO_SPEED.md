# Up to Speed: Technical-Lead Operating Method

## 1. Read the active authority

Read, in order:

1. `AGENTS.md`
2. `CURRENT_SLICE.md`
3. `ARCHITECTURE.md`

Then read the exact dossier, decision record, source, calculation, and evidence package named by the current question.

Do not substitute a roadmap, prior agent report, component shopping list, or conversational summary for the governing source.

## 2. Establish exact bases

Before authorizing or reviewing work, record:

- repository commit and tree;
- current-slice blob;
- architecture blob;
- relevant dossier and ADR blobs;
- fixture ID;
- mechanical revision;
- electronics revision;
- firmware revision;
- homelab-service revision;
- evidence package revision.

For a pre-implementation measurement slice, record the available instrument and fixture state instead of inventing absent implementation revisions.

## 3. Separate the evidence classes

For each technical question, record:

| Field | Required content |
|---|---|
| Intent | Exact governing requirement |
| Published basis | Exact authoritative supplier or safety fact |
| Observed basis | Exact fixture measurement or implementation behavior |
| Derived basis | Equation, input values, units, and result |
| Match | What is already directly supported |
| Gap | What is missing |
| Risk | What could make the planned claim false or unsafe |
| Drift | What current code, hardware, or prose contradicts |
| Unknown | What cannot yet be determined |
| Decision | The smallest operator or architecture choice required |

Do not collapse a published motor maximum, a calculated load, and a measured operating current into one “confirmed” number.

## 4. Trace one physical and logical route

For motion work, trace:

```text
desired state
→ command validation and freshness
→ device state machine
→ energy and safety permission
→ motor-driver request
→ motor and transmission
→ encoder/current/temperature observations
→ retained state, fault, and telemetry
```

For solar work, trace:

```text
window light
→ panel voltage/current
→ charger input behavior
→ system load and battery supplement
→ stored energy
→ sleep/radio/motion consumption
→ daily energy balance
```

Name the owner of every committed fact.

## 5. Keep fixture truth local

One shade and window can prove:

- that fixture's dimensions;
- that fixture's force range;
- that fixture's chain geometry;
- that fixture's harvested energy;
- that fixture's installed reliability.

It cannot by itself prove universal shade compatibility, regional winter autonomy, child safety, or fleet maintainability.

Generalize only through an explicit compatibility envelope and multiple retained fixtures.

## 6. Present one bounded decision

After inspection, present:

- exact product need;
- exact current basis;
- first failed or missing crossing;
- smallest next proof;
- alternatives;
- safety posture;
- explicit nonclaims;
- owning component or repository;
- changed-path and evidence envelope.

Obtain operator approval when the choice changes product behavior, safety posture, licensing, public claims, or long-lived architecture.

## 7. Write one current slice

A current slice must contain:

- status;
- one primary claim;
- exact grounding;
- fixture and assumptions;
- in-scope work;
- explicit non-goals;
- required evidence;
- acceptance criteria;
- failure and rollback behavior;
- successor handoff.

Avoid planning several future slices as if they were already authorized.

## 8. Require an independent pre-edit scan

The implementing agent or contributor independently verifies:

- exact branch and basis;
- current interfaces;
- available tools and fixture;
- voltage/current/thermal limits;
- changed paths;
- dependency and ownership constraints;
- implementability of acceptance criteria.

A blocker is reported before editing or energizing hardware.

## 9. Review the exact result

Review:

- exact pull-request head;
- exact schematic/CAD/firmware diff;
- exact test output;
- exact evidence files;
- negative and fault-path behavior;
- public claims.

Tests and a successful demonstration are evidence, not substitutes for architecture or safety review.

## 10. Close the loop after merge

After merge:

- update `PROJECT_STATUS.md`;
- close or supersede `CURRENT_SLICE.md`;
- retain exact evidence references;
- update the document register when governing files changed;
- record new unknowns instead of smuggling them into the next implementation;
- authorize the next slice only after a new bounded decision.

## Stop conditions

Stop before selecting, implementing, installing, or merging work when:

- required governing material has not been read;
- a photograph or datasheet is being used in place of a fixture measurement;
- motor, battery, charger, or mounting limits are unknown;
- a remote command can bypass local safety;
- a continuous loop would be left loose or exposed;
- ordinary operation depends on hard stall;
- the source of position truth is ambiguous;
- energy neutrality is claimed without field watt-hour evidence;
- a public photograph or log exposes home or network details;
- a disagreement is being silently reconciled;
- the current slice is being widened through “cleanup,” infrastructure, or convenience;
- a product decision is missing.
