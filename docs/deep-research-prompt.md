# Deep Research Prompt: Where Smallbox Invariants Could Be Useful

Copy the prompt below into Deep Research and attach the available project and architecture documents.

---

## Prompt

Prepare a rigorous, evidence-based research report for **Smallbox Labs** on the practical usefulness, technical position, and most credible initial applications of **Smallbox Invariants**.

The report must answer this central question:

> **Where could an open-source framework for making software invariants explicit, executable, and verifiable create real value beyond ordinary types, tests, static analysis, architecture tests, rule engines, and formal methods?**

Do not assume the project is broadly useful. Determine whether it is useful, for whom, under what conditions, and where it would add unnecessary complexity or false confidence.

### Project context

Smallbox Invariants is an experimental open-source project for representing meanings and rules that ordinary application code often leaves implicit, connecting those declarations to code analysis and verification, and reporting whether a rule is:

- preserved;
- violated;
- unknown because the necessary meaning cannot be established; or
- declared but not yet mechanically observable.

The intended structure has three parts:

1. **Generic checking machinery** that inspects code and produces evidence.
2. **Reusable rule patterns** for recurring failure classes.
3. **Project-specific declarations** that map local types, members, and boundaries to their actual domain meaning.

The production application does not have to depend on the framework at runtime. Its own calculations may remain local and specialised. Smallbox Invariants can stand outside the application and check whether the implementation preserves declared meaning.

It is **not primarily a shared production-calculation library**. Reference calculations may be useful as an independent third opinion, a benchmark, or a test oracle, but sharing one implementation between a producer and its verifier can create circular verification: the calculation merely agrees with itself.

The longer-term concept may include several distinct rule families:

- semantic quantities such as basis, currency, units, time basis, measurement date, and provenance;
- contracts on operands, returns, assignments, DTOs, and boundary crossings;
- architectural ownership and dependency rules;
- tenant, mode, account, or permission isolation;
- state-transition and irreversibility rules;
- claim, evidence, trust, and placement constraints;
- independence requirements between a producer and the mechanism grading it;
- machine-readable context and checks for AI-assisted software changes.

Do not assume that all these families belong in one mechanism. Investigate whether they require separate rule packs, analysis methods, or even separate tools.

### Current status and limitations

The public project status is experimental. The current implementation demonstrates a narrow form of semantic checking across several .NET codebases; the broader invariant framework remains a research direction.

Treat these constraints as central to the report:

- a checker must remain explicitly **unknown** when the necessary meaning cannot be established;
- a semantic rule only works where meaning can be declared and propagated without inventing knowledge;
- a valid controlled example does not prove useful coverage in an existing system;
- sharing one calculation between a producer and its verifier can create circular agreement rather than independent evidence;
- a new rule built around the defect that motivated it must be evaluated against independent controls and other codebases;
- useful coverage, false positives, false confidence, annotation effort, and refactoring resilience all matter.

### Optional private context

If private project documents or experiment records are supplied separately, use them only as confidential context for understanding the motivation and current prototype. Do not reproduce non-public source code, repository paths, commit identifiers, product data, architecture inventories, or incident details in the report.

Abstract any private examples to the smallest generic form needed to explain a failure class. Treat internal experiments as evidence about the prototype, not as independent evidence of market demand, external validity, or technical novelty.

## Research tasks

### 1. Define the problem precisely

Explain the class of defects Smallbox Invariants is attempting to address. Distinguish:

- arithmetic errors;
- mathematically valid but semantically incompatible operations;
- input-quality failures;
- output-contract failures;
- architectural-boundary violations;
- circular or nominally independent verification;
- business-rule violations;
- unsupported confidence, provenance, or placement of derived claims.

Show which of these ordinary compilers and test suites routinely catch, which they can catch with established techniques, and where a meaningful gap may remain.

### 2. Map the existing landscape and prior art

Compare the proposed approach with current, maintained tools and established research. At minimum, investigate relevant examples from:

- C# and F# type systems, units-of-measure approaches, domain value types, strongly typed identifiers, and quantity/currency libraries;
- Roslyn analyzers and source generators;
- CodeQL, Semgrep, Sonar-style static analysis, and custom linting;
- NetArchTest, ArchUnitNET, architecture fitness functions, and software reflexion models;
- design by contract, refinement types, dependent types, and typestate;
- property-based, metamorphic, mutation, differential, and model-based testing;
- formal specification and verification tools such as TLA+, Alloy, and Dafny where relevant;
- business-rule engines and policy-as-code systems;
- data-quality and data-contract tools;
- provenance standards such as W3C PROV;
- assurance cases and Goal Structuring Notation;
- tools or research that provide machine-readable architectural constraints to AI coding agents.

For each relevant category, state:

- what it already solves well;
- what it requires from adopters;
- whether it works statically, at build time, in tests, at runtime, or through review;
- whether it can express project-specific semantic meaning;
- what Smallbox Invariants would add, duplicate, or do worse.

Do not manufacture novelty by ignoring prior art. If an established tool already solves the proposed problem adequately, say so.

### 3. Identify and rank real application domains

Investigate where semantically wrong but numerically plausible software behaviour is common and consequential. Consider, but do not limit the research to:

- finance, accounting, trading, portfolios, and financial reporting;
- billing, pricing, subscriptions, tax, and payments;
- lending, mortgages, insurance, and actuarial systems;
- energy, metering, carbon accounting, and utilities;
- logistics, inventory, manufacturing, and supply chains;
- healthcare or scientific software where units and provenance matter;
- simulations, educational games, and game economies;
- multi-tenant, multi-mode, permission-sensitive, or workflow-heavy systems;
- data products and AI-assisted systems that turn source data into reader-facing claims.

Produce a ranked table with these columns:

| Domain / system type | Concrete recurring invariant | Example failure | Existing way teams handle it | Consequence if missed | Fit for Smallbox Invariants | Annotation/integration cost | Likely adopter and buyer |
|---|---|---|---|---|---|---|---|

Do not rank a domain highly merely because it contains calculations. Rank it highly only when the framework could provide a defensible advantage over existing practice.

### 4. Find the narrowest credible initial use

Recommend the most defensible scope for **Smallbox Invariants 0.1**.

Test at least these candidate starting points:

1. Semantic basis and currency checking for calculation-heavy .NET systems.
2. Declared input/output/assignment contracts across calculation boundaries.
3. Architecture and ownership rules expressed as project-specific declarations.
4. Independent-verification and evidence-boundary checks.
5. Machine-readable invariant context for AI coding agents.

For each candidate, assess:

- severity and frequency of the problem;
- ability to detect it mechanically;
- false-positive and false-confidence risks;
- annotation burden;
- overlap with existing tools;
- ease of producing a convincing example;
- ease of local/CI adoption;
- likelihood that an external team would try it.

Choose one initial wedge, explain why it wins, and explicitly state what should remain out of scope.

### 5. Examine the AI-assisted development claim

Investigate the narrower proposition:

> Can explicit, machine-checkable semantic and architectural declarations help AI coding agents place changes correctly and avoid violations that repository prose alone does not prevent?

Find relevant current research and tooling. Separate:

- providing context to the model;
- checking the generated change after the fact;
- automatically repairing violations;
- measuring whether declarations improve outcomes.

Propose a small, publishable, descriptive experiment using matched implementation tasks with and without explicit boundary declarations. Account for model variability, non-blinding, small sample size, leakage, and the danger of grading with the same mechanism that generated the answer. Do not propose statistical claims that the experiment cannot support.

### 6. Design a credible validation programme

Define what evidence would be required before calling the project broadly useful. Include:

- previously known historical defects;
- valid siblings and negative controls;
- unknown/undeclared controls;
- controlled mutations;
- previously unknown defects;
- transfer to unrelated codebases;
- false-positive rate;
- useful-coverage rate;
- annotation and maintenance cost;
- resilience to refactoring;
- mutation testing of the checking mechanism;
- evidence that the checker does not silently turn unknown into clean.

Recommend concrete success thresholds where defensible, but explain their basis. Include stop conditions that would justify narrowing or ending a rule family.

### 7. Recommend an open-source product shape

Assess the best public form for the project, including:

- NuGet analyzer package;
- CLI and CI integration;
- declaration format and rule-pack API;
- local-only analysis so private source does not leave the developer's environment;
- evidence-bundle format and optional visual renderer;
- runnable examples and sanitised historical specimens;
- documentation website and interactive demonstration;
- versioning, licensing, contribution, and governance considerations.

Explain what should be open source and what Smallbox Labs could reasonably offer as paid integration, custom rule modelling, managed reporting, or consulting without weakening trust in the open checker.

### 8. Assess practical adoption

Identify:

- the first likely users;
- who experiences the problem;
- who would approve adoption;
- the moment in a project when adoption is easiest;
- barriers in legacy and greenfield systems;
- why a team would choose this instead of stronger domain types, more tests, or an existing analyzer;
- what a five-minute successful first experience would need to look like.

Propose three realistic pilot profiles. At least one should come from an organisation with no prior connection to Smallbox Labs.

## Required report structure

1. **Executive verdict:** a direct answer to whether the project could be useful, for whom, and in what narrow form.
2. **The problem class:** what the framework does and does not address.
3. **Prior-art and competitor map:** direct comparison with existing approaches.
4. **Ranked application-domain table.**
5. **Detailed analysis of the top three applications.**
6. **Recommended Smallbox Invariants 0.1 scope.**
7. **AI-assisted development hypothesis and experiment.**
8. **Validation plan, measurements, and stop conditions.**
9. **Open-source distribution and adoption path.**
10. **Commercial and grant relevance:** brief and evidence-based, not a grant proposal.
11. **Risks, negative findings, and unresolved questions.**
12. **A concrete 90-day research and release sequence.**

End with four explicit decisions:

- the one-sentence public positioning;
- the first user and first use case;
- the first three things to build;
- the things not to build yet.

## Research and writing standards

- Use current sources and state the research date.
- Prefer primary sources, official documentation, standards, maintained repositories, peer-reviewed research, and credible real-world case studies.
- Cite factual claims close to the text they support with direct links.
- Distinguish documented fact, inference, and recommendation.
- Verify current maintenance status, licensing, and capabilities before comparing tools.
- Do not use generic market-size estimates or consultancy claims as evidence of usefulness.
- Do not treat the project's internal experiments as proof of external demand.
- Preserve negative results, unknowns, and cases the framework cannot yet observe.
- Avoid promotional language, inflated novelty claims, and claims of “formal proof” unless formally justified.
- Prefer a small number of deeply analysed use cases over a long speculative list.
- Use concrete examples and short code-like illustrations where they clarify the semantic failure.

The report should help Smallbox Labs decide whether to continue, narrow, or stop parts of the project; which first public release would be genuinely useful; and what evidence would be needed for a credible open-source or grant proposal.
