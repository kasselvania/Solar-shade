# Evidence

This directory retains sanitized, reproducible proof packages.

Evidence is organized by slice and fixture, for example:

```text
evidence/
  s0-reference-shade/
  s1-guarded-bench-motion/
  s2-local-controller/
  s3-power-path/
  s4-window-solar/
  s5-local-network/
  s6-installed-field-trial/
```

Every package must include a `README.md` that identifies:

- repository commit;
- slice;
- fixture;
- mechanical/electrical/firmware revisions;
- date;
- instruments;
- environment;
- procedure;
- raw files;
- derived files;
- result;
- nonclaims.

## Public sanitation

Do not commit:

- full room/exterior photographs when a crop is enough;
- addresses or precise location;
- Wi-Fi names, IP addresses, tokens, certificates, or passwords;
- serial numbers unrelated to the claim;
- family or bystander images;
- proprietary binary assets without permission;
- undocumented lithium packs or unsafe test instructions normalized as installed design.

Use an external private archive for raw sensitive material where needed. Public evidence should preserve the technical claim without exposing the home.

## Data integrity

- Keep raw observations separate from derived summaries.
- Preserve units.
- Record missing samples.
- Do not overwrite failed runs.
- Corrections retain the original value and reason.
- Large binary logs may be compressed or placed in release artifacts later, with checksums and indexes.
