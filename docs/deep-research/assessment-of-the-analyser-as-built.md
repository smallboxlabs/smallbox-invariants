# Grounded assessment of the analyser as built

## Judgement

On the record you supplied, this is a defensible open-source contribution **if it is framed narrowly and honestly**: not as “verification for finance”, not as a lightweight liquid-type system for .NET in general, and not as a CI gate, but as a **semantic-kind checker for arithmetic sites in calculation-heavy .NET code when the relevant quantities can be named by a small closed vocabulary**. That is a real contribution because Roslyn gives you the compiler substrate — syntax, symbols, binding, diagnostics, packaging — but it does **not** give you a domain semantic model of what a quantity *is* in the sense your analyser needs. Roslyn’s own documentation is explicit that its semantic APIs answer questions about symbols, bindings and program meaning in compiler terms; custom analyzers are then responsible for whatever project-specific semantics they enforce. citeturn2view2turn2view3turn2view4

The strongest single conclusion from your record is not “one bug was caught”; it is the **split between transferable machinery and non-transferable vocabulary**. The byte-identical analyser transfers. The propagation rules transfer. The corpus binding transfers. But usefulness collapses when the domain’s money concepts are not the ones the closed set can name. That is not a footnote. It is the main result, because it tells a reviewer where the tool lives: **reusable engine, domain-contingent semantics**.

I would therefore rule **for** publication or open-sourcing, but only behind a constrained claim. The refusals you describe read to me primarily as **rigour**, not embarrassment: the system declines to speak where its vocabulary cannot justify a sound distinction. That is the correct design stance. But the price of that rigour is a materially smaller applicable surface, and the report should say so explicitly rather than treating it as an implementation detail.

One procedural caveat matters. I have not independently rerun the suite, because no repository or artefacts were attached in this conversation. So this report assesses the **evidential force of the supplied record** — its internal coherence, its methodological discipline, and the claim it would license if those measurements reproduce — rather than certifying the measurements firsthand.

## Relation to refinement and liquid types

The only comparison that is actually interesting here is the one you asked for: **against refinement and liquid types, not against static analysis in the abstract**.

Refinement types extend ordinary types with logical predicates and contracts. Liquid-style systems deliberately buy more expressiveness by making specifications solver-facing: predicates refine base types, pre- and post-conditions can relate values, and the checking story runs through SMT-backed reasoning. The classic LiquidHaskell papers are very clear on that bargain. They define refinement types as base types plus SMT-decidable predicates and present them as a way to encode contracts and richer invariants than ordinary type systems can express; they also show the upside, with substantial verification over large Haskell codebases. citeturn19view0turn19view1

That extra power comes with an adoption cost that is now better documented than it was when many earlier surveys were written. The 2025 study on liquid types reports **nine barriers** across three broad categories: verification-process understanding, developer-experience friction, and scalability. The concrete barriers include confusing verification features, unfamiliarity with proof engineering, unhelpful solver-shaped error messages, limited IDE support, insufficient learning resources, complex installation/setup, and performance/solver limitations on larger codebases. Those are not abstract worries; they are observed obstacles for real developers working with a mature liquid-type implementation. citeturn3view0turn3view2turn3view3turn3view4turn17view0turn17view1turn17view2turn17view3turn17view4turn17view5

That literature matters directly to your analyser, because it clarifies where your design differs. Your checker asks developers for **closed-set declarations**, not predicates; it propagates kinds through a hand-written rule set, not general solver obligations; and its failure modes are mostly about **unknownness and inexpressibility**, not proof search or SMT diagnostics. In other words, it gives up a great deal of expressiveness in order to avoid many of the barriers that make liquid types hard to adopt in ordinary engineering environments. That is exactly why the space between “Roslyn analyzer” and “liquid types” is real. citeturn3view5turn17view1turn17view2turn17view5turn2view4

Recent work trying to make liquid types more usable in mainstream OO settings reinforces the same point. The LiquidJava line of work reports a 30-developer evaluation and argues that better syntax, improved diagnostics, synthesis support, and aliasing tracking are all needed to make refinement-style checking feel practical in Java. That is encouraging for refinement types, but it also underlines the size of the hill they still have to climb. Your analyser is not solving that problem. It is sidestepping it by being much less expressive. citeturn19view2turn13view2

So the right verdict is this: **the analyser is not merely a thin slice of liquid types with the same adoption ceiling**. It occupies a genuinely different point in the design space. The trade is straightforward:

- liquid/refinement types can state much richer facts, including precisely the whole-function output contracts your current lattice misses, but they inherit heavier usability and tooling burdens; citeturn19view0turn19view1turn3view5turn17view1turn17view4
- your analyser has a much lower conceptual surface for authors and users, because it lives inside ordinary .NET code and ordinary analyzer infrastructure, but it can only reason about the classes of mismatch its vocabulary and extractor route can name. citeturn2view2turn2view4turn14search0

That is the whitespace. It is real. It is also narrow.

## What the record actually establishes

The evidential centre of the project is **not** ownership drift, duplicated rule locations, or the other declaration forms that currently lack an extractor and a specimen. The centre is the **quantity-axis work**, because that is where all of the following exist at once: live extraction, historical specimen, structurally similar negatives, propagation sensitivity, transfer, and mutation evidence. I would state that bluntly in the report and retire the earlier tendency to recentre the project around the less-evidenced declaration forms.

Within that centre, the caught defect is load-bearing for one specific reason: your record says the defect is **not structurally special**. The wrong ratio sits beside structurally similar correct siblings, using the same record types, nullable numerics, LINQ shapes and guarded ternaries. If no type-name, flow-shape, dependency, or test-shape cue separates the bad line from the good line, then a positive result there is evidence that the declared semantic kind is doing actual discriminative work. That matters.

Even so, the true-positive count is still **one**. The fact that it reappears in two independent places strengthens the case that the kind model found a real semantic mismatch rather than an incidental coding quirk, but it does not turn one defect class into two. Your own limit statement is the right one: one caught real defect, one missed real defect, zero caught unknown defects.

For a reviewer, the **miss is more informative than the catch**. The catch proves existence: this technique can do something useful that ordinary structure-sensitive analysis would not obviously do. The miss, however, defines the ceiling of the current technique. It says that the present lattice is fundamentally aimed at **operand-pair compatibility**, not at **function-output contracts**, and that the extractor route is brittle around carriers such as tuples and anonymous forms. A reviewer deciding whether to back the project learns more from the miss, because it says what the analyser is *not yet*. The catch says “there is something here”; the miss says “here is the wall”.

That is why the axis-parametric result needs to be split in two. The evidence supports **engine-level parametricity**: the propagation-rule machinery appears reusable across axes, with axis definitions and conversions supplied as data, not hard-coded into the analyser. Roslyn’s analyzer framework is a suitable host for that kind of reusable core. citeturn2view2turn2view4

But the evidence does **not** yet support **problem-level generalisation**. The second-system transfer shows exactly the opposite: the engine moved, the declarations moved, the binding worked, yet judged coverage on the basis axis collapsed because the domain’s salient quantities mostly sit outside the vocabulary. The right conclusion is therefore:

> **Parametricity has been shown for the propagation engine, not for domain usefulness.**

That is a meaningful result. It is also much smaller than “this framework generalises”.

## The significance of the 2.6% and the two refusals

The 53.7% and the 2.6% should be read together, not averaged away. Read properly, they say:

- when the domain vocabulary matches the closed-set axis well enough, the checker can judge a substantial fraction of expressible arithmetic sites without observed false positives on judged cases;
- when the domain vocabulary does not match, transfer can be technically clean and practically disappointing at the same time.

That second point is valuable. A lot of tool evaluations quietly turn vocabulary mismatch into “future work”. Your record does the opposite: it preserves the collapse as a finding. That is good science.

I would judge both refusals as **rigour**. In the origin product, refusing to seed the currency axis where `Native` conflates reporting and listing currency is the right decision, because either one label or two labels would make the analyser confidently wrong in opposite ways. In the second system, refusing to declare local and normalised prices as if they were the same per-unit kind is also correct, because doing so would inflate surface coverage by erasing the very distinction that matters for the real hazard. Those are not evasions. They are examples of a checker refusing to certify semantics it cannot justify.

The reason I still classify them as rigour, rather than as an admission of defeat, is that the same design instinct is visible in the literature on more expressive verification systems. One of the recurring usability problems in liquid-type tools is that they surface solver-shaped answers and tool-internal representations that are formally related to the program but not cognitively aligned with what the developer actually needs. Your analyser takes the opposite risk profile: when the semantics are undernamed, it prefers explicit silence to misleading precision. That is a defensible, even admirable, trade. citeturn17view1turn17view4

But this is the important sting in the tail: **rigour here is inseparable from a smaller surface of applicability**. So the report should not say “the refusals show the method is rigorous” and stop there. It should say: **the refusals are rigorous precisely because they reveal that the method’s honest surface is smaller than a casual reading of the architecture might suggest**. That is not a contradiction. It is the right reading of the evidence.

The same logic applies to the tuple and anonymous-carrier blind spots. In standard C# tuple usage, the runtime shape is `ValueTuple` with `Item1`, `Item2`, and so on as fields, while tuple element names are carried through metadata on the containing member via `TupleElementNamesAttribute`. That is enough for readability and ordinary compiler features, but it is not the same thing as having attachable domain declarations on a first-class named carrier type. In other words, the language construct is lightweight partly because it declines to become a semantic record type. For your analyser, that design choice becomes a real extractor boundary. citeturn10search5turn11search0turn11search6turn12search13

## The claim the evidence supports

The evidence supports one claim, and it does **not** support several larger ones.

The sentence I think the present record licenses is this:

> **In calculation-heavy .NET code whose relevant quantities can be named by a small closed vocabulary, a Roslyn-based semantic-kind checker can catch some real operand-kind mismatches with no observed false positives on judged sites, at an annotation density that appears operationally tolerable, but with substantial blind spots where the vocabulary or extractor route cannot name the semantics.**

That sentence is small, but it is honest.

The sentence the present record does **not** license is this:

> **This analyser is a generally reusable semantic verification layer for financial .NET systems, is ready to gate CI, and can be expected to catch unknown quantity bugs across domains.**

It does not license that sentence because the record still has one caught real defect, one missed real defect, no unknown-defect catches, no independently blinded external evaluation, and sharply domain-dependent judged coverage. It also does not yet justify the broader “axis-parametric framework” claim beyond engine reuse. A synthetic third axis proves a software engineering property — that the code/data separation is real — but not yet a domain-scientific property — that a third real axis in a live corpus will earn its keep.

Placed against the refinement/liquid-type record, this is exactly the kind of smaller claim that makes sense. More expressive systems can state much richer contracts, but they pay for that with documented usability, setup, IDE, proof-engineering and scaling barriers. Your analyser is valuable **because** it asks less, not because it can do the same job more conveniently. citeturn19view0turn19view1turn3view5turn17view1turn17view2turn17view3turn17view4turn17view5

That difference also matches what adoption research has found more broadly. Developers’ choices are heavily shaped by existing code, existing expertise and available libraries, not just by the theoretical elegance of a feature set. A narrower semantic checker that lives inside familiar .NET and Roslyn workflows therefore has a plausible route to use that a full new verification discipline may not. That does not prove usefulness, but it does explain why your specific trade-off is strategically sensible. citeturn18search4turn18search1

## The cheapest falsification

The cheapest experiment that could falsify the licensed claim is **not** a grand new framework effort. It is a short, pre-registered, third-corpus transfer in a domain that appears vocabulary-compatible.

The design should be lean:

First, choose a third calculation-heavy .NET subsystem whose quantities genuinely look nameable by a small closed axis. Avoid the already-inexpressible classes you have identified, because the point is not to rediscover the known edge. Before any sweep, write down the axis members, the conversions, and the predictions for one seeded historical specimen and one structurally similar negative control.

Second, cap the declaration budget aggressively — something like the same order of magnitude as your current work, not an open-ended ontology exercise. The moment the axis wants to sprawl, that itself is evidence against the claim.

Third, run the unchanged engine and report exactly four things: judged coverage of expressible arithmetic sites, positive findings, false positives, and whether the seeded specimen and negative control behave as predicted.

This experiment could falsify the present claim in days because the core analyser already exists and the result threshold is simple. I would treat any of the following as a serious falsifier:

- the new corpus looks vocabulary-compatible on inspection, but judged coverage of expressible sites still remains trivial;
- a convincing false positive appears early in the judged set;
- the seeded operand-kind specimen is missed;
- or the annotation burden for that axis blows past the current density budget without commensurate judged coverage.

If no suitable third corpus is available immediately, the second-cheapest falsifier is the one already sitting in your brief: implement the **pre-registered assignment-contract rule** against the preserved miss, then run it across the current corpora and controls without changing the predictions post hoc. That experiment would not falsify the current narrow claim; it would falsify the stronger next-step claim that the framework can grow from operand-pair checking into output-contract checking without paying a large false-positive or ad hoc-exception cost.

## Packaging and measurable success conditions

The right packaging judgement is **both**, with a shared core and clearly separated purposes.

The **Roslyn analyzer package** is the right user-facing delivery for developers. Microsoft’s documentation is explicit about what that buys you: custom diagnostics running during editing and build, editor squiggles, Error List integration, light-bulb UX, standard suppression routes, and repository-level severity control through `.editorconfig` or AnalyzerConfig. Those are real adoption advantages, especially for a technique whose selling point is lower friction than refinement-style verification. citeturn2view4turn14search0turn14search1turn14search3turn14search4

The **standalone sweep** should still remain the canonical evaluation harness. Your best evidence so far depends on corpus-wide accounting: judged-site coverage, silent-with-reason tests, preserved misses, mutation checks, byte-identical transfer, declaration density, and the deliberate decision not to overstate the tool as a gate. An IDE/package form is excellent for delivery, but poor at expressing that broader measurement discipline. If you ship only the package, you risk losing the very apparatus that keeps the claims honest.

So the packaging recommendation is:

- **one shared analysis core**;
- **a standalone corpus runner** for research, backtesting, coverage accounting, mutation work, and non-gating sweeps;
- **a Roslyn analyzer package** for day-to-day developer feedback.

I would not wire it into CI as an error gate yet. That part of your current design judgement is sound. The right interim posture is **package it, but do not oversell it**: warnings where it has high-confidence collisions, configurable severity, and an explicit statement that current coverage is partial and domain-dependent. Roslyn’s configuration model supports exactly that kind of staged rollout. citeturn14search0turn14search1turn14search5

The success conditions for this recommendation should be measurable. I would use these, and only keep the packaging strategy if they hold:

- **precision:** no more than one credible false positive per hundred judged diagnostics in pilot use;
- **coverage:** for every new axis, report judged coverage of expressible arithmetic sites, not just raw total findings;
- **burden:** declaration density for each new axis stays inside a declared budget rather than drifting upward unnoticed;
- **yield:** within the next two backtests or pilots, the tool finds at least one additional true issue or prevents one regression before release;
- **performance/adoption:** package deployment does not impose enough design-time or build-time friction that teams disable it immediately, and severity stays configurable rather than gate-like by default. citeturn14search0turn14search5turn18search4

If those measures do not move, the project should stay a standalone research artefact. If they do move, then the paired delivery model — package for interaction, sweep for evidence — is the right long-term shape.