# smallboxlabs-invariants
An open-source framework for making what must remain true in software explicit, executable and verifiable.
## Status
Experimental. The current implementation demonstrates semantic basis checking across several .NET codebases. The broader invariant framework is under active development, and its APIs and rule definitions may change.

### Smallbox Invariants

An open-source Smallbox Labs project for making **what must remain true in software explicit, executable, and verifiable**.

It provides reusable machinery for describing meanings and business invariants, examining a codebase, and reporting whether each rule is:

* preserved;
* violated;
* unknown;
* not yet mechanically observable.

### What lives there

* The Roslyn analyser and verification machinery.
* Semantic dimensions such as basis, currency, time, provenance, trust, and ownership.
* Reusable failure patterns and rule definitions.
* Valid examples, controlled mutations, and sanitised historical defects that test the framework itself.
* CLI/CI integration, evidence reports, visualisation, documentation, and demonstrations.
* Optional reference calculations used to test or independently verify application calculations.

### What does not live there

* Any examined system's production code.
* Their databases, private data, credentials, and product behaviour.
* Customer source code or confidential business rules.
* One enormous shared calculation library that every application must depend upon.

Production calculations may remain local and specialised. Smallbox Invariants stands outside the application and checks whether those implementations preserve their declared meaning.

CompanyGraph and a second production system are its first real test systems and case studies. **Smallbox Labs owns and develops the public framework; the products are examined by it rather than owning it.**

<!-- The second system is deliberately unnamed here: it is CO-OWNED, and naming it as a case study is the
     co-owner's decision to make, not ours. Restore the name once they approve. CompanyGraph is named because it
     is wholly owned and its relevant defect is already described on its own public corrections page. -->

