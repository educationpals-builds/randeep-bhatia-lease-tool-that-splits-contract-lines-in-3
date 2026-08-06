# Lease Duty-Splitting Tool — Five-Check Audit Prompts

Use these five standalone prompts to audit any lease duty-splitting tool. Each check ends with the measurement it demands. Paste one prompt at a time into any chat model, along with your tool's output on a sample lease.

---

## Check 1: Unowned

**Prompt:**

I have a lease tool that splits contract lines into separate duties. I need to audit whether every parsing rule has an owner who can fix it when it fails.

Here is a failing input from my tool:

> Tenant shall repair the roof provided that Landlord funds materials within 10 days.

The tool merged both duties onto one line, making the wrong party look responsible.

**Your task:**
1. List every parsing rule the tool must apply to split duties correctly on hinge words like "provided that", "unless", and "provided, however".
2. For each rule, name who owns it — or mark it "unowned" if no one is assigned.
3. Count the unowned rules.

**Measurement demanded:** Number of parsing rules with no assigned owner.

---

## Check 2: Copies

**Prompt:**

I have a lease tool that splits contract lines into separate duties. I need to audit whether the tool duplicates logic in multiple places, making fixes fragile.

Here is a failing input from my tool:

> Fees accrue daily; provided, however, that the cap in §4.2 still applies.

The tool failed to split at "provided, however" and merged two duties.

**Your task:**
1. Identify every place in the tool's logic where hinge-word detection could occur.
2. Flag any duplicated detection logic (same pattern matched in more than one module or step).
3. Count the duplicated copies.

**Measurement demanded:** Number of duplicated hinge-word detection patterns across the codebase.

---

## Check 3: Room

**Prompt:**

I have a lease tool that splits contract lines into separate duties. I need to audit whether the tool has room to handle edge cases — nested clauses, unusual punctuation, variant phrasings.

Here is a failing input from my tool:

> Notice is deemed given when posted, unless the parties agree otherwise in writing.

The tool did not split at "unless" and left both conditions merged.

**Your task:**
1. List the edge cases the tool must handle: nested "provided that" clauses, semicolon-separated conditions, variant hinge phrases ("provided, however", "unless", "except that").
2. For each edge case, state whether the current logic has room to handle it or is hardcoded to fail.
3. Count the edge cases with no room for handling.

**Measurement demanded:** Number of edge-case patterns the tool cannot currently accommodate.

---

## Check 4: Stitch

**Prompt:**

I have a lease tool that splits contract lines into separate duties. I need to audit whether the tool's components stitch together correctly — does the output of one step feed cleanly into the next?

Here are three failing inputs from my tool:

> Tenant shall repair the roof provided that Landlord funds materials within 10 days.
> Fees accrue daily; provided, however, that the cap in §4.2 still applies.
> Notice is deemed given when posted, unless the parties agree otherwise in writing.

In each case, the tool merged duties that should have been split.

**Your task:**
1. Map the pipeline: what step detects hinge words, what step splits duties, what step assigns parties.
2. Identify any stitch gaps — places where one step's output does not match the next step's expected input.
3. Count the stitch gaps.

**Measurement demanded:** Number of pipeline handoff points where output format does not match input expectation.

---

## Check 5: Ablation

**Prompt:**

I have a lease tool that splits contract lines into separate duties. I need to audit whether removing any single component breaks the tool entirely — a sign of missing redundancy or unclear ownership.

Here is a failing input from my tool:

> Tenant shall repair the roof provided that Landlord funds materials within 10 days.

The tool merged both duties onto one line.

**Your task:**
1. List the components involved in duty-splitting: hinge-word detector, clause boundary parser, party assigner.
2. For each component, state what happens if it is removed or disabled.
3. Count the components whose removal causes total failure with no fallback.

**Measurement demanded:** Number of single-point-of-failure components with no fallback behavior.

---

## Worked Example: Builder's Audit

**Specimen:** Lease tool that splits contract lines into separate duties

**Failing inputs (from Harbor Lease sample contracts):**
- Tenant shall repair the roof provided that Landlord funds materials within 10 days.
- Fees accrue daily; provided, however, that the cap in §4.2 still applies.
- Notice is deemed given when posted, unless the parties agree otherwise in writing.

**Check ratings:**
| Check | Score (1–5) |
|-------|-------------|
| Unowned | 4 |
| Copies | 2 |
| Room | 2 |
| Stitch | 2 |
| Ablation | 1 |

**Top crack:** Unowned

**Ship call:** Hold. An unowned check on hinge words is not a tuning problem, it's a missing-component problem — no amount of retraining fixes a gap nobody is assigned to close. Priya owns adding an explicit hinge-detection step before Friday's ship.

**Tripwire:** Watch: count of hinge words ("provided that", "unless", "provided, however") appearing in source text but absent from any duty-boundary in output. Threshold: 1 miss per 50 leases triggers Priya's review — because right now this number doesn't exist as a metric at all.

---

## Sample Asks

A stranger auditing their own lease duty-splitting tool can paste inputs like these:

**Stranger input 1:**
> Lessee shall maintain insurance provided that Lessor provides proof of property value within 30 days.

**Stranger input 2:**
> Rent increases annually; provided, however, that increases shall not exceed 3% per year.

**Stranger input 3:**
> Subletting is permitted unless Landlord objects in writing within 14 days.

Run each check prompt with your tool's output on these inputs. Record the measurement each check demands. The check with the worst score is your top crack.
