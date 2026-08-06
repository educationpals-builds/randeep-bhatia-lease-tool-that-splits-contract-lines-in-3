# Verify: Lease tool that splits contract lines into separate duties

## What this file is for

A stranger who runs their own lease-splitting setup can use this verification flow to confirm the auditor surfaces the deciding-check finding and demands a numeric measurement for it.

---

## Verification steps

### 1. Prepare your specimen

Gather three real lease lines where your tool currently fails to split duties correctly. Focus on lines containing hinge words like "provided that", "unless", or "provided, however".

Example format (from the builder's audit):
> Tenant shall repair the roof provided that Landlord funds materials within 10 days.
> Fees accrue daily; provided, however, that the cap in §4.2 still applies.
> Notice is deemed given when posted, unless the parties agree otherwise in writing.

### 2. Run through /play

Open the auditor in /play mode. Paste:
- Your tool description (what it's supposed to do)
- Your standard (how you'll know it's fixed)
- Your three failing lines

### 3. Confirm the deciding-check finding surfaces

The auditor must walk all five checks and identify which one decides. In the builder's case, the deciding check was **unowned** — the hinge-word detection gap had no owner assigned to close it.

Your verification passes when:
- [ ] The auditor names a specific check as the decider (not a vague "needs improvement")
- [ ] The finding connects to your actual failing lines, not generic examples

### 4. Confirm a numeric measurement is demanded

The auditor must propose a concrete metric for the deciding check. In the builder's audit:

> Watch: count of hinge words ("provided that", "unless", "provided, however") appearing in source text but absent from any duty-boundary in output. Threshold: 1 miss per 50 leases triggers Priya's review — because right now this number doesn't exist as a metric at all.

Your verification passes when:
- [ ] The auditor proposes a countable metric (not "monitor quality")
- [ ] The metric has a threshold number that triggers action
- [ ] An owner is named for watching that threshold

---

## What failure looks like

If the auditor returns findings without numeric measurements, or names no owner on the tripwire, the verification fails. The tool must demand the same discipline the builder applied: a number, a danger line, and a watcher.

---

## Builder's reference

- **Specimen**: Lease tool that splits contract lines into separate duties
- **Deciding check**: unowned
- **Call**: Hold. An unowned check on hinge words is not a tuning problem, it's a missing-component problem — no amount of retraining fixes a gap nobody is assigned to close. Priya owns adding an explicit hinge-detection step before Friday's ship.
- **Tripwire**: 1 hinge-word miss per 50 leases triggers Priya's review
