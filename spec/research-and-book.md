## The Book of ZKPs

This section is informative. It describes the explanatory book and its relationship to the construction records.

### One technical record, two reading paths

The specification explains interoperable behavior and the status of each construction. The book explains why a construction is useful, how its cryptography works, which alternatives fit the use case and how to reproduce the evidence. Both use the same claim, disclosure, assumption and version identifiers; an accessible explanation does not strengthen the technical claim it explains.

A book chapter follows this structure: trust-graph use case; formal statement and non-goals; credential/witness requirements; relation to the reference paper; construction options; worked proof and negative examples; privacy and operational limits; open questions. The cryptographic background follows the operational material so readers can first locate the problem they need to solve.

### Matching the paper to a DTG construction

> **WG-08 — Evidence and reviewer request: formal mapping and book.** Ask for review of the exact paper-to-DTG statement and assumptions, an explanatory chapter, and a reproducible worked construction. Record volunteers explicitly; this note assigns nobody. Status: review requested, equivalence not established.


The book is organized by trust-graph outcomes, not by the predicate list of a single source paper. Personhood/liveness is one chapter family. Other families draw on their own relevant cryptographic constructions and requirements.

For each mapping to *A Cryptographic Framework for Proof of Personhood* ([ePrint 2026/333](https://eprint.iacr.org/2026/333)), record the exact paper revision, section/definition, assumptions, DTG interpretation and evidence for compatibility. Separate a directly checked statement, an editorial interpretation and a proposed DTG extension. A matching section number is not a security reduction.

Existing paper section and benchmark references in the records are inherited research pointers. Their exact equivalence to the proposed DTG profiles remains subject to full-text review and author feedback. In particular, transcript binding in a circuit does not by itself establish the paper's stronger proof-system properties; shared credential names do not establish compatible issuance or vouch interfaces. Paper benchmark numbers are not measurements of ADR-001.

The task force needs help reviewing the formal-to-credential mapping, teaching the relevant cryptography accurately and producing reproducible examples. These are proposed reviewer roles, not assignments: cryptographic reviewers check assumptions and composition; credential/task implementers check representability and custody; technical editors check that the book and spec preserve the same claim.

The proposed process for surveying related work and maintaining these shared records is described in the maintenance note at the end of this specification.

### From option research to verification

The existing proving-system entries and per-construction option rows are the starting research catalogue. For each option, carry forward its credential/signature compatibility, witness and device requirements, setup/security assumptions, disclosure, and measured versus reported costs. Similar algorithms do not imply interchangeable credentials or profiles.

The **Agent Handoff: Construction Verification Worklist** at the end of this specification turns those fit assessments into tasks V01–V12. It separates runnable component checks from missing implementations, full-statement comparisons and real task/VTC integration. Each returned result updates the relevant book explanation and evidence row; a failed fit is a useful recorded result.
