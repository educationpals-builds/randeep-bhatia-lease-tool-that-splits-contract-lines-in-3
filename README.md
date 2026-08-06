# Lease tool that splits contract lines into separate duties

## Verdict: Hold

An unowned check on hinge words is not a tuning problem, it's a missing-component problem — no amount of retraining fixes a gap nobody is assigned to close. Priya owns adding an explicit hinge-detection step before Friday's ship.

---

## The problem

A lease tool is supposed to split contract lines into separate duties. On lines with "provided that", it still merges two duties so the wrong party looks responsible.

**What goes wrong if this never gets fixed:** A partner signs a summary that puts repair duty on the wrong side.

**Standard for fixed:** Each duty lands on its own line with the right party named.

---

## Tripwire

Watch: count of hinge words ("provided that", "unless", "provided, however") appearing in source text but absent from any duty-boundary in output. Threshold: 1 miss per 50 leases triggers Priya's review — because right now this number doesn't exist as a metric at all.

---

## One-paste rebuild

To run the full five-check audit on your own lease-splitting setup:

```
Describe your lease-splitting tool and paste three real contract lines where it fails to separate duties correctly.
```

The auditor walks five checks, scores each, identifies the deciding check, and returns:
- A severity story grounded in your failing lines
- A ship / hold call with an owner on any condition
- A tripwire with a number, a danger line, and who watches it

See [charter.md](charter.md) for the complete audit of this specimen.

---

## What this auditor checks

The tool applies five checks to any lease-splitting setup. Each check gets a score (1–4). The lowest-scoring check with the highest impact decides the call.

This audit found the **unowned** check as the decider — no one owns the hinge-word detection gap.

For the method behind these five checks, see [METHOD.md](METHOD.md).

---

## Verify the tool works

See [VERIFY.md](VERIFY.md) for stranger verification steps.

<!-- educationpals-build-verified -->
