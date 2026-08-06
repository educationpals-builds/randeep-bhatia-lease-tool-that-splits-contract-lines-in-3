# The Five Checks: PRISM

When auditing whether a setup's checks actually split the work, apply these five principles in sequence.

---

## P — Partition the Space

Every input type the tool handles must land in exactly one bucket. For lease-splitting, this means: does the tool have distinct handling for simple clauses, compound clauses with hinge words ("provided that", "unless", "provided, however"), and nested conditionals? If two input types share a single code path, the partition is incomplete.

---

## R — Run in Parallel

Each check should operate independently. The hinge-word detector shouldn't wait for party-assignment to finish. When checks run in sequence and one fails silently, downstream checks inherit garbage. Parallel execution surfaces failures at their source.

---

## I — Individuate the Pattern

Each pattern the tool must recognize needs its own explicit detector. "Provided that" is not the same pattern as "unless" — they create different duty structures. A single regex that lumps all hinge words together will merge duties that should stay separate. One pattern, one detector.

---

## S — Stitch the Spectra

After individual detectors fire, their outputs must be stitched into a coherent result. If the hinge-word detector finds "provided that" but the duty-splitter ignores that signal, the final output collapses back to a single merged line. Stitching means each detector's output changes the final structure.

---

## M — Map What Each Head Sees

For every check, you must be able to answer: what did this check see, and what did it do about it? If you can't trace a specific input through a specific check to a specific output change, that check is decorative. Mapping means logging or surfacing the check's view of each input.

---

## The Anti-Pattern: Collapse to Monochrome

When a setup lacks one or more PRISM principles, it collapses to monochrome — all inputs get the same treatment regardless of their structure. 

In lease-splitting, collapse looks like this: "Tenant shall repair the roof provided that Landlord funds materials within 10 days" becomes a single duty assigned to Tenant, because the tool never partitioned hinge-word clauses into their own bucket, never individuated "provided that" as a pattern requiring a split, and never mapped what the hinge-word check saw.

The result: a partner signs a summary that puts repair duty on the wrong side.

---

## Using PRISM in Your Audit

Walk each principle as a check. Rate how well the setup implements it (1 = absent, 5 = solid). The lowest-rated check is your deciding factor — the crack that determines your ship/hold call.

These five letters appear only in this file. Other audit documents reference "the five checks" without spelling out the acronym.
