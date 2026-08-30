# Governance

## Purpose

This repository contains three different kinds of material that must not be confused:

1. long-form product and system design intent;
2. active implementation authority;
3. evidence of what one exact fixture and implementation actually did.

The project remains trustworthy only when those layers stay distinct.

## Governing surfaces

### Long-form dossiers

Files under `docs/dossiers/` preserve product intent, failure pressure, user experience, safety goals, and long-range system shape.

They are used by the operator and technical lead to make bounded decisions. They are not wholesale coding prompts and they do not make every described future capability current scope.

### Active architecture

`ARCHITECTURE.md` is the short, current ownership and dependency law for implementation.

A dossier may be broader than the active architecture. That is expected. Future intent does not automatically become a current interface, module, daemon, PCB block, or protocol.

### Current slice

`CURRENT_SLICE.md` is the sole live implementation or experimental authority.

No issue, roadmap sentence, component wish list, or implementation convenience may silently widen it.

### Evidence and status

`evidence/` records what happened. `PROJECT_STATUS.md` summarizes only current, supportable facts, hypotheses, gaps, and blockers.

Passing tests prove only the tested behavior. A successful bench run does not prove safe installation, seasonal autonomy, or broad compatibility.

## Fact classes

Every retained claim should be classifiable as follows.

| Class | Meaning | Example |
|---|---|---|
| `OBSERVED` | Directly measured or photographed on an identified fixture | Peak chain pull was 7.4 N on fixture SS-FX-001 |
| `PUBLISHED` | Stated by an identified authoritative source | A motor vendor publishes a 2.0 A extrapolated stall current |
| `DERIVED` | Calculated from explicit inputs | Required sprocket torque is force × pitch radius |
| `DECIDED` | Operator-approved product or architecture choice | Remote commands are desired-state intents, not PWM commands |
| `HYPOTHESIZED` | Plausible but unproved | A compact micro gearmotor may fit the final enclosure |
| `UNKNOWN` | Not yet known | Winter harvested energy at the reference window |

A statement may have more than one supporting class, but the classes do not collapse into each other.

## Precedence for physical and product truth

For questions about what is physically safe or true:

1. applicable law, mandatory safety constraints, and verified component absolute limits;
2. direct evidence from the exact identified fixture and revision;
3. official manufacturer datasheets, manuals, and errata;
4. independently repeatable calculations using explicit inputs;
5. commercial-product benchmarks;
6. informal reports;
7. assumption.

For questions about intended product behavior:

1. explicit operator decision;
2. accepted architecture decision record;
3. `ARCHITECTURE.md`;
4. the relevant governing dossier;
5. current status and planning material.

Product intent cannot override a component limit. A datasheet cannot decide the desired user experience.

## Source discipline

Research records must identify:

- source title;
- publisher;
- direct URL;
- access date;
- source class;
- exact fact used;
- limitations or interpretation.

Prefer primary sources:

- manufacturer datasheets and official product pages;
- official framework documentation;
- standards bodies;
- government safety authorities;
- direct measurements.

Retail availability and prices are volatile. Record them as dated snapshots.

Do not copy substantial third-party text. Retain concise facts, calculations, and links.

## Measurement discipline

A measurement record must identify the exact fixture and method. At minimum include:

```text
fixture_id
date
repository_commit
instrument
instrument_resolution_or_limit
environment
procedure
raw_observations
derived_values
result
nonclaims
```

Do not silently discard failed trials or inconvenient measurements. Repair an obvious transcription error by preserving the original value, the correction, and the reason.

Photographs are evidence only for what is visible. They do not establish hidden dimensions, material strength, battery ratings, wire polarity, chain pitch, or safe clearances.

## Design amendments

A proposed change to governing design must be explicit.

The proposal must identify:

- exact document and section affected;
- current language or decision;
- proposed language or decision;
- reason;
- evidence or pressure causing the change;
- implementation consequences;
- migration or compatibility consequences;
- operator disposition.

Cleanup, status editing, issue prose, implementation comments, and pull-request summaries cannot amend governing design.

Use an ADR when the change is a durable implementation boundary or tradeoff. Use a dossier amendment when it changes product intent.

## Disagreement handling

When design, implementation, and evidence disagree, record them separately:

```text
Design:
  what the governing document requires

Implementation:
  what the exact code or hardware currently does

Evidence:
  what was directly observed

Difference:
  the unresolved mismatch

Decision:
  repair, amend, defer, or reject
```

Do not write a blended sentence that makes the disagreement disappear.

## Component selection law

A component becomes a candidate because published specifications appear compatible.

A component becomes a prototype selection only after:

- the current slice authorizes selection;
- its exact published limits are recorded;
- fixture-derived requirements fit inside those limits with stated margin;
- known thermal, current, voltage, mechanical, and availability risks are recorded;
- substitutions and nonclaims are explicit.

A component becomes qualified only through the exact acceptance evidence required for its role.

Nameplate power, theoretical stall torque, nominal battery capacity, and wireless feature lists are not qualification.

## Safety-governance rules

- No unattended motor test without a guarded chain path, independent current limit, bounded movement timeout, and reachable physical stop.
- No lithium charging test near a window without a known protected cell, qualified charger, observed temperature, and nonflammable test posture.
- No child-safe or product-safe claim from a prototype.
- No removal of an anchored continuous-loop tension device unless the prototype simultaneously provides an equal or safer anchored constraint.
- No ordinary endpoint operation by repeated hard stall.
- No bypass of a fault latch by a remote retry loop.
- No install instruction may normalize exposed pinch points or unsupported adhesive-only mounting.
- Any damaged, swollen, hot, punctured, or undocumented battery is rejected from the project.

## Public-repository privacy

The repository is public. Retained evidence must be sanitized before commit.

Crop or redact:

- street or building identifiers;
- family photographs;
- addresses and precise location;
- Wi-Fi names and IP addresses;
- device serial numbers;
- access tokens and certificates;
- account or purchase identifiers.

A full reference-room photograph is not committed merely because it is technically relevant. Use a cropped chain, tensioner, mounting surface, and window-light view sufficient for the claim.

## Repository independence

The embedded device, homelab service, CAD, electronics, and evidence tooling may eventually live in one repository or several repositories. Repository topology is a working boundary, not automatically a product boundary.

A future split must preserve ownership and versioned contracts. It must not duplicate motion authority or cause the homelab to become required for local safety.

## Work selection

The technical lead selects no slice from a summary alone.

Before authorizing work:

1. read the exact relevant dossier sections;
2. inspect the current architecture and status;
3. inspect the exact fixture evidence and implementation;
4. record match, gap, risk, drift, and unknown;
5. identify the smallest next proof;
6. state alternatives and nonclaims;
7. obtain operator approval when a product decision is required.

## Review and merge

Review the exact pull-request head, not an agent's summary.

A merge requires:

- the authorized claim to be satisfied;
- changed paths to remain inside scope;
- acceptance evidence to be present or linked;
- failure behavior to be tested where applicable;
- governing status to be updated without inventing future completion.

After merge, close the current slice or authorize the next slice explicitly. Do not leave stale implementation authority that appears active after its work is complete.

## Status language

Use only supportable status:

- `UNEXPLORED`
- `RESEARCHED`
- `CHARACTERIZED`
- `AUTHORIZED`
- `IMPLEMENTED`
- `BENCH_PROVED`
- `FIXTURE_PROVED`
- `FIELD_TRIAL`
- `QUALIFIED_FOR_DEFINED_USE`
- `BLOCKED`
- `REJECTED`

Avoid “done,” “production-ready,” “safe,” “universal,” or “self-sustaining” without a defined acceptance envelope.
