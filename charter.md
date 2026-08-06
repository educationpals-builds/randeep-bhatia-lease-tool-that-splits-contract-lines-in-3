# Audit: Lease tool that splits contract lines into separate duties

## Specimen under review

**Tool:** Lease tool that splits contract lines into separate duties

**What breaks if this stays broken:** A partner signs a summary that puts repair duty on the wrong side

**Standard for "fixed":** Each duty lands on its own line with the right party named

---

## Real inputs tested

**Source:** Harbor Lease sample contracts

**Usage reality:** Old scanned leases with nested "provided that" lines

### Pasted failing examples

```
Tenant shall repair the roof provided that Landlord funds materials within 10 days.
Fees accrue daily; provided, however, that the cap in §4.2 still applies.
Notice is deemed given when posted, unless the parties agree otherwise in writing.
```

---

## Five-check findings

| Check | Score (1–5) | Notes |
|-------|-------------|-------|
| Unowned | 4 | Hinge words like "provided that" have no assigned handler — the tool doesn't know to look for them |
| Copies | 2 | Duplicate duty fragments appear when clauses nest |
| Room | 2 | No margin for variation in phrasing |
| Stitch | 2 | Conditional clauses get stitched to the wrong duty |
| Ablation | 1 | Removing hinge detection (which doesn't exist) changes nothing — confirms the gap |

---

## Deciding check

**Top crack:** Unowned

The unowned check scores highest because hinge words ("provided that", "unless", "provided, however") are not assigned to any detection component. The tool has no mechanism to recognize these phrases as duty-boundary markers.

---

## Severity story

*[Not provided — severity_note field unanswered]*

---

## Ship call

Hold. An unowned check on hinge words is not a tuning problem, it's a missing-component problem — no amount of retraining fixes a gap nobody is assigned to close. Priya owns adding an explicit hinge-detection step before Friday's ship.

---

## Tripwire

Watch: count of hinge words ("provided that", "unless", "provided, however") appearing in source text but absent from any duty-boundary in output. Threshold: 1 miss per 50 leases triggers Priya's review — because right now this number doesn't exist as a metric at all.

---

## Summary

This audit examined a lease-splitting tool using three real contract lines from Harbor Lease sample contracts. The tool fails to recognize hinge words that signal conditional duties, causing it to merge separate obligations and misattribute responsibility. The deciding gap is unowned — no component handles hinge detection. The call is Hold until Priya adds an explicit hinge-detection step. After release, watch for hinge words in source that don't appear at duty boundaries; 1 miss per 50 leases triggers review.
