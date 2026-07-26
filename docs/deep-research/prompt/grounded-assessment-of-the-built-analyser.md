# Deep Research Prompt: Grounded Assessment of the Analyser As BUILT

**Second pass, 2026-07-26.** The first report (`docs/deep-research/assessment-invariants.md`) was explicit that
it inferred the concept from public marketing pages rather than assessing an implementation. It researched the
surrounding category well and the project's own record not at all. This brief supplies the record.

**Read before publishing:** this brief names the products the rules were learned from and two of their real
defects. Both are the owner's own products, one defect is already described publicly on a corrections page, and
no credentials, customer data or business rules appear here. Publishing it is still an editorial decision, not a
mechanical one — decide consciously.

---

## What has changed since the first report

The first report's verdict — *continue but narrow substantially* — is accepted. Its prior-art work is accepted
and should not be repeated. Two of its conclusions are contested, and the contest is the reason for this pass:

1. It proposed the project's centre as **ownership drift and duplicated business-rule locations**. Those
   correspond to declaration forms that exist in the vocabulary but have **no extractor and no specimen** — the
   least evidenced half. The forms that carry every measurement, every specimen and every mutation-check are the
   **quantity axes**, for which its defect-class table has no row.
2. It treated "Roslyn already supports custom analysers" as territory already occupied. Roslyn supplies symbol
   binding — the substrate. It supplies no semantic model of what a quantity IS. The contested claim is whether
   that model is the contribution.

---

## The central question for this pass

> Given the concrete artefact and experimental record below, is **semantic-kind checking of calculation-heavy
> .NET code** a defensible open-source contribution — and if so, what is the smallest honest claim the evidence
> supports, and what would falsify it?

Specifically, and each of these is answerable against the record rather than in the abstract:

- Does a **four-member closed set with ~30 declarations** occupy real whitespace between refinement/liquid types
  (expressive, nine documented adoption barriers) and dependency-rule tools (cheap, cannot express semantic
  identity)? Or is it a thin slice of the former with the same adoption ceiling?
- Is the **axis-parametric design** (propagation rules as code, closed sets and conversions as data) a genuine
  generalisation, or an abstraction over n=2 axes that will not survive a third?
- The record contains **one real defect caught and one real defect missed.** Which of those is the more
  informative result for a reviewer, and what does the miss imply about the technique's ceiling?
- Would this be **better delivered as a Roslyn analyser package** (editor squiggles, standard suppression,
  existing distribution) than as a standalone sweep? What is lost either way?

---

## The record. Verify it; do not take it as established.

Everything below is measured and reproducible from the test suite. Where a number is an estimate or a
limitation, it is labelled. **A report that repeats the wins and omits the limitations has failed this brief.**

### What the thing is

A Roslyn-based sweep (~1,100 lines) that resolves the declared **semantic kind** of every operand at every
arithmetic site in a compiled corpus, and reports operand pairs that cannot honestly be set against each other.
Fifteen propagation rules, stated in writing before the first sweep ran. Two axes:

- **basis** — `PerShare · Total · Dimensionless · ShareCount`, with one conversion (`Total / ShareCount →
  PerShare`) and two compositions.
- **currency** — `Native · Usd · None · UsdPerNative`, with the rate derivation, both conversions, and one
  pair marked *sanctioned but unnameable* (`Native / Usd` is a real rate the set has no member for → silent, not
  flagged).

The rules are axis-agnostic code; the closed sets and conversions are data. A synthetic third axis (a time base:
`PerYear × Years → Cumulative`) works with zero analyser changes, and that is a test, not a claim.

Declared kinds are matched by **enum member NAME**, so three repositories each hold their own copy of the
vocabulary with no shared package and no cross-references.

### What it caught

**One real historical defect, caught in two independent places.** A ratio divided an undivided cash flow by a
per-share dividend — mathematically valid, semantically invalid, and it scored several hundred companies for
months. Spliced back verbatim from git:

- at the site where it was **written** (the scoring engine): 1 collision, 0 false positives;
- at the site where it was **checked** (an independent recompute verifier in a different repository, different
  assembly, different tests — which reproduced the same error and certified 715 firings as verified): 1
  collision, 0 false positives.

**The control is the load-bearing part.** In the same file as the defect, four other ratios cross the same record
types, use the same input object, the same `decimal?`, the same LINQ shape and the same guarded ternary. One is
wrong. **No type, name, dataflow, dependency or test-shape signature separates them** — that separation is the
whole result, and the declared kind is the only thing that achieves it. The structurally identical correct
sibling is never flagged.

Sensitivity was measured, not assumed: disabling propagation rules one at a time showed **three are individually
necessary** to the catch (`Math.Abs` preserves kind; a conditional merges its branches; `ToDictionary`'s value
selector gives element kind). No redundancy.

### What it MISSES — and this is not a caveat, it is half the result

**A second real defect, written by a person in a live product, is not caught.** A portfolio series valued
holdings in the local currency while the total it was displayed against was normalized (a EUR holding inflated a
chart by ~8%). Spliced back verbatim: **0 collisions, 0 return-contract mismatches, both axes.** Two independent
structural reasons, both measured:

1. Every operation inside the method is internally **consistent** in one currency. The error is the function's
   **output kind** — a whole-function contract mismatch, not an operand pair. The lattice judges operand pairs.
2. The value chain is severed at a `Dictionary<int, (decimal, decimal)>`, and a **ValueTuple's members cannot
   carry an attribute**, so the operands are Unknown regardless of what is declared elsewhere.

The miss is preserved as an executable specimen that asserts today's silence, so it fails the day the gap closes.
A candidate rule (assignment contracts) is registered with predictions **and deliberately unbuilt**, because a
capability added the moment its test case appears is a capability its test case can no longer test.

### Coverage, and the collapse that matters

| | Origin product | Sibling subsystem | Unrelated product |
|---|---|---|---|
| basis sites judged / where a collision is expressible | **191 / 356 (53.7%)** | **127 / 493 (25.6%)** | **5 / 190 (2.6%)** |
| collisions at HEAD | 0 | 0 | 0 |
| historical specimen | caught | caught | n/a — controlled mutation caught |
| false positives | 0 | 0 | 0 |

**The 2.6% is the finding, not a failure to report around.** The analyser transferred byte-identical to an
unrelated product (a classroom trading game) and bound a 504-site corpus with zero errors — but that domain's
quantities are house prices, mortgage balances, venture valuations and experience points, which the closed set
cannot name. **The machinery transferred; the vocabulary did not.** Assess the significance of that split
directly: it is the difference between a reusable framework and a good rule about one domain.

### The two refusals, which are design positions and should be judged as such

1. **The currency axis is deliberately UNSEEDED in the origin product's own corpora** — so the axis has never run
   on the codebase it was built for. Reason: `Native` is not one kind there. A statement figure is in the
   *reporting* currency and a price in the *listing* currency; the corpus divides one by the other constantly. One
   label certifies all of those ratios; two labels flag all of them, and most are fine. Both make the checker
   confidently wrong, so it reports nothing. Tests assert the silence with the reason attached.
2. **Declaring quantities the closed set cannot distinguish was refused even though it would have raised
   coverage.** In the unrelated product, a native price and a normalized price sit side by side — the 2nd and 3rd
   largest unreached chain ends, 45 occurrences. Both are per-unit, so the basis axis would have labelled them
   identically, certified their difference as sound, and shown better coverage. **Declaring them would have made
   the real hazard invisible.**

Judge whether these refusals are rigour or an admission that the technique's applicable surface is smaller than
it appears. Both readings are available and the report should pick one and defend it.

### Method, because the method is part of what is being assessed

- Rules were written down **before** the first sweep; a rule invented after seeing output is fitted to it.
- Predictions are **registered in a committed document before implementation**. The most recent rule shipped with
  five predictions, all held — including the load-bearing prediction that it would **not** catch the real defect.
- Checks are **mutation-tested**: restoring an earlier ordinal-matching scheme causes the historical defect's own
  shape to resolve to a sanctioned conversion and report nothing; removing a single declaration kills both the
  positive and negative controls. A notable result: **all 21 corpus tests pass identically under that mutation**,
  because the real vocabularies happened to be in the analysed order — so proving the fix required synthetic
  vocabularies. *A latent coupling is invisible to every test that uses the convention it depends on.*
- **Not wired into CI, deliberately.** A check judging 53.7% of one corpus on one axis is evidence, not a gate.

### Honest limits, stated so the report does not have to discover them

- **n on real defects caught is 1**, and it is the same defect across every phase. One further real defect is a
  miss. Nothing has caught an unknown defect.
- The original basis labels were assigned by the author **knowing which case had to be separated**. Zero false
  positives across 152–191 judged sites is real; one true positive is one.
- Unjudged sites are **unjudged, not clean**. 165 of 356, 366 of 493, 185 of 190.
- Three further vocabulary forms (single derivation home, sanctioned recompute, assertion ceiling, one-way
  transition) have **live declarations but no extractor** — they are checked only for internal consistency.
- **Density: 139 declaration sites across 85,208 production LOC = 16.3 per 10k**, inside the 6–20 band the first
  report proposed, having moved from ~11.7 in a single day when the second axis landed. A third axis needs a
  density budget.
- Known blind spot, pinned rather than fixed: `Math.Max(localPrice, normalizedPrice)` sets two quantities against
  each other exactly as `a - b` does, and the Math-family rule resolves it through a merge that answers "I cannot
  tell which one comes back" — sound about the result, silent about the comparison.
- Inexpressible classes found by transfer: currency in a domain with a game currency, non-share asset money,
  equity percentages, tuple and anonymous-type members, and runtime-keyed field readers (`GetDecimal("name")`),
  which are core to how one subsystem reads its parameters.

---

## What the report must do

1. **Place the technique against refinement/liquid types specifically**, not against static analysis generally.
   That is the only comparison where the claim is interesting, and the adoption-barrier literature is the crux.
2. **Rule on the axis-parametric generalisation** using the n=2-plus-synthetic evidence. Say plainly if that is
   insufficient.
3. **Say which claim the evidence supports** and write the single sentence it would license — then write the
   sentence it would NOT license, so the difference is on the record.
4. **Name the cheapest experiment that could falsify it.** Preference: one that could be run in days.
5. **Rule on packaging**: standalone sweep vs. Roslyn analyser package vs. both.
6. **Do not repeat the first report's prior-art survey.** Extend it only where the record demands.

## What would make this report useless

- Treating "Roslyn exists" as dispositive without addressing the substrate-versus-semantic-model distinction.
- Reporting the caught defect without the missed one, or the 53.7% without the 2.6%.
- Recommending the ownership/duplicated-rules centre again without engaging the fact that those forms have no
  extractor and no specimen.
- Any recommendation whose success condition cannot be measured.
