# Lease tool that splits contract lines into separate duties

## One-paste spec for the conversational auditor

This auditor walks five checks against any lease duty-splitting tool to determine whether it correctly separates contract lines into distinct duties with the right party named.

---

## The problem this auditor targets

A lease tool is supposed to split contract lines into separate duties. On lines with "provided that", it still merges two duties so the wrong party looks responsible.

**What goes wrong if this never gets fixed:** A partner signs a summary that puts repair duty on the wrong side

**Standard for success:** Each duty lands on its own line with the right party named

**Real inputs look like:** Old scanned leases with nested "provided that" lines

---

## Failing inputs (from Harbor Lease sample contracts)

```
Tenant shall repair the roof provided that Landlord funds materials within 10 days.
Fees accrue daily; provided, however, that the cap in §4.2 still applies.
Notice is deemed given when posted, unless the parties agree otherwise in writing.
```

---

## Five-check audit framework

The auditor walks these five checks against the lease duty-splitting tool:

| Check | What it measures | Rating (1–5) |
|-------|------------------|--------------|
| **Unowned** | Is there a gap nobody is assigned to close? | 4 |
| **Copies** | Does the tool duplicate or lose duties? | 2 |
| **Room** | Is there space for the tool to improve? | 2 |
| **Stitch** | Does the tool connect duties to the right parties? | 2 |
| **Ablation** | What breaks if you remove a component? | 1 |

**Top crack:** unowned

---

## Worked example: the builder's own audit

### Ship call

Hold. An unowned check on hinge words is not a tuning problem, it's a missing-component problem — no amount of retraining fixes a gap nobody is assigned to close. Priya owns adding an explicit hinge-detection step before Friday's ship.

### Tripwire

Watch: count of hinge words ("provided that", "unless", "provided, however") appearing in source text but absent from any duty-boundary in output. Threshold: 1 miss per 50 leases triggers Priya's review — because right now this number doesn't exist as a metric at all.

---

## How a stranger uses this auditor

A stranger describes their own lease duty-splitting tool (or similar contract-parsing setup) that is failing. They provide:

1. What the tool is supposed to do
2. Who gets hurt when it fails
3. A few real failing inputs (lease lines where duties merge incorrectly)

The auditor then:

1. Walks all five checks conversationally against the stranger's tool
2. Proposes findings with the measurement that would confirm each
3. Returns a scored audit with:
   - A severity story naming the top crack
   - A call (ship / ship-with-conditions / hold) with an owner on any condition
   - An alarm with a number, a danger line, and a watcher

---

## Sample asks

A stranger might paste:

> "Our lease parser handles most clauses but when we hit 'notwithstanding the foregoing' or 'subject to' it lumps the condition with the main duty. Last week a tenant got a summary saying they owed HVAC maintenance when the lease clearly put that on the landlord after the 'subject to' clause."

> "We're splitting commercial lease obligations but 'except as otherwise provided' keeps getting swallowed into the prior sentence. Three clients have complained about wrong party assignments this month."

> "Duty extraction tool for residential leases. Works fine on simple sentences but 'provided, however' and 'unless otherwise agreed' cause the tool to assign both duties to whoever is named first."

---

## Acceptance criteria

- Auditor walks all five checks for a stranger's lease duty-splitting tool
- Every finding names the measurement that would confirm it (e.g., count of hinge words missed)
- Each result includes a call with an owner on any condition, and an alarm with a number, a danger line, and a watcher
- The builder's own audit (hinge-word detection gap, Priya's ownership, 1-miss-per-50-leases threshold) is visible as the worked example
