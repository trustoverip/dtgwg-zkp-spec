<!-- generated-from: records-sha256=01b4e776e2a2dae7cce575a4682923a079a06d17721a1702ba29d4e0d2254b40 — the Requests Answered, Construction Records, Proving Systems and derived Privacy Considerations sections are generated from conformance/{records,requests,stacks}; conformance/test.mjs compares this stamp, the marked generated sections and generated terms with fresh generation from these files. Edit the records, not the generated text. -->

<!-- generated-section:requests:start -->
## Requests Answered

This section is informative.

A request is kept in the requester's own form — an architecture decision record, an issue, a decision taken on a call — so that a construction can be checked against what was actually asked rather than against the construction's own paraphrase of it. Every request names the construction record that answers it; every clause of the request says where in that record it landed, or that it did not. This section is generated from the machine-readable requests in `conformance/requests/`.

The first request is Glenn Gore's ADR-001, *Community-Anchored Proof*, named by the task force as its first construction to seed the work: it defines what must be proven and what any implementation must satisfy, and deliberately does not choose how. The construction record is the how. Requests are ordered by date.

### Request ADR-001 · Community-Anchored Proof

*Choosing the first zero-knowledge use case to implement against DTG credentials — and what any implementation of it must satisfy.*

| | |
|---|---|
| kind | architecture-decision-record |
| author | Glenn Gore (stormer78), First Person Project |
| status | Proposed · 2026-08-25 |
| audience | Four working groups: DTG Credentials · ZKP · Trust Registry · Governance |
| source | https://docs.fpp.storm.ws/ (ADR-001) · named in zkp-tf #18 — ScottJeezey 2026-08-27: 'a natural first one to seed it with' |
| answered by | [Construction 010 · Community-Anchored Proof (ADR-001)](#construction-010-%C2%B7-community-anchored-proof-(adr-001)) |

> Clause texts below are close paraphrases for crosswalk purposes; the record of authority is Glenn's document. The ADR is deliberately format-agnostic and 'does not choose how to build it' — the construction record is the how.

**Decision.** The first zero-knowledge proof implemented against DTG credentials is the Community-Anchored Proof the Credentials Core Specification already describes: a relationship exists inside a shared community, without revealing who is in it.

**The statement the holder makes.** “A member of community C has a working relationship with me, and I am also a member of community C.”

**What must be proven, together, in one proof.**

1. the holder possesses a valid relationship credential
2. the holder possesses a valid membership credential from community C
3. the party who issued that relationship credential also holds a valid membership credential from the same community C — 'the hard one': a statement about someone else's credential, proven without the other party online, asked, or aware

#### Crosswalk — where each clause lands in the construction record

Status: **covered** (the record carries it as written) · **refined** (carried with a precision the request did not state) · **added** (a clause the task force found missing) · **partial** (carried in part; the gap is named) · **open** (belongs to another group or a profile). Counts: covered 15 · refined 4 · added 2 · open 4 · partial 3.

| clause | group | requirement (paraphrase) | where the record carries it | status | refinement |
|---|---|---|---|---|---|
| P1 | privacy | reveals no relationship identifier from the relationship credential | 010 adversary (verifier · verifiers-colluding) | **covered** |  |
| P2 | privacy | reveals no identifier of the counterparty who issued it | 010 adversary | **covered** |  |
| P3 | privacy | reveals no identifier of the holder beyond attributes the holder deliberately discloses | 010 disclosure set (deliberate disclosure line) | **covered** |  |
| P4 | privacy | two proofs from the same credentials cannot be linked — by one verifier or by verifiers comparing notes | 010 relation (nullifier clause, parameterised) · adversary (verifiers-colluding) | **refined** | full show-unlinkability cannot coexist with reuse detection in the same context (ePrint 2026/333 §5.3); parameterise by context: unlinkable across contexts, the declared nullifier the only link within a reuse-detecting one |
| P5 | privacy | a verifier learns the outcome and deliberately disclosed attributes, nothing further | 010 disclosure set | **covered** |  |
| S1 | soundness | a party lacking the required credentials cannot produce a verifying proof | 010 method 1–3 · conformance fixtures rejects-unsat / rejects-verify | **covered** |  |
| S2 | soundness | clause 3 cannot be satisfied unless the counterparty genuinely holds a membership credential from the same community | 010 relation (set-membership on the voucher's grant leaf, record 001) | **refined** | proves the community-issued grant half only (cred-spec §VMC, both directions; §Membership Edge Completion carve-out) — the acknowledgement is not in the presenter's hands |
| S3 | soundness | the counterparty need not be online, consulted, or aware | 010 witness (proven from root_C, not from the voucher) | **refined** | under WD02 pairwise identifiers the voucher's linkage (VRC-side ↔ VMC-side identifier) must have been supplied at issuance or avoided by one directed identifier — record 007 / HR-2; otherwise clause 3 is unprovable offline |
| S4 | soundness | the presenter proves they are the subject of the credentials, not merely a holder of copies | 010 relation (key-binding, record 004) | **covered** |  |
| S5 | soundness | bound to a verifier challenge; not replayable to another verifier or time | 010 relation (transcript-bind, record 003) · public inputs transcriptDigest | **covered** |  |
| S6 | soundness | (added by the ZKP TF) the voucher is not the holder — a self-vouch is unsatisfiable | 010 relation (distinctness, record 005) | **added** | without it a member with two identifiers vouches for themselves and clauses 1–3 verify |
| S7 | soundness | (added, WD02) the identifiers a party used in the VRC and in their VMC are controlled by one secret | 010 relation (key-binding, record 007) | **added** | cred-spec §Community-Anchored Zero-Knowledge Proof: 'the proof must additionally establish common control' |
| C1 | currency | does not verify if any relied-on credential is revoked or suspended | 010 relation (non-revocation, record 006) | **covered** |  |
| C2 | currency | states the registry state it was made against; the verifier judges recency | 010 public inputs (root_C, rl_root, epoch) · disclosure set | **covered** |  |
| C3 | currency | establishing currency must not itself identify the holder | 010 adversary (registry-operator · issuer-verifier-colluding) | **covered** | holds only if roots are fetched without a per-holder query — stated as the condition |
| C4 | currency | the delay between a change and proofs reflecting it is bounded and published | 010 horizon (status freshness, C4 bound) | **open** | the bound is a registry/governance publication, not a proof property — construction carries it as a horizon input |
| T1 | registry | a verifier confirms the community is one it recognises without learning which member | 010 public inputs (root_C at a stated registry state) | **covered** |  |
| T2 | registry | whatever the proof relies on from a registry is independently checkable by a party that did not create it | 006 set-root primitive route · public inputs set roots | **refined** | the set-root primitive (cred-tf #40): signed published root + zero-knowledge membership/status proof in the presentation; paths and openings stay private; root/witness retrieval requires an explicit privacy policy |
| T3 | registry | two verifiers checking against the same registry state reach the same verdict | 010 conformance fixtures (current family) · fixtures determinism | **covered** |  |
| T4 | registry | a registry's obligations are stated as an interface so any conformant registry can serve these proofs | — (Trust Registry TF) | **open** |  |
| D1 | deployment | proving is feasible on the devices that hold credentials — a phone or an agent — within a stated time and memory budget | 010 options (Groth16 est. ≈35–45k constraints ~2 s; blackbox 0.03 s/vouch) · stacks layer (ProveKit phone numbers) | **partial** | budgets not yet stated by the TF; the options layer supplies measured numbers per stack for the gate to set them |
| D2 | deployment | verification is cheap enough to run inline | lab: ~8–10 ms verify (Groth16) | **covered** |  |
| D3 | deployment | a proof fits the transports DTG credentials already travel over | stacks layer: proof size column (721 B Groth16 vs ~716 KB WHIR) | **open** | a profile decision; the options table shows both numbers |
| X1 | conformance | published test vectors covering proofs that must verify and proofs that must fail | 010 conformance fixtures families · runtimes/fixtures format (accept/reject/lint vectors, rejection-reason register) | **partial** | format exists and is consumed cross-language; 010's own vector family not yet cut |
| X2 | conformance | an independent implementation can verify another's proofs | registry (independent reproduction) · consumer-py (zero-shared-code consumer) | **partial** | reproduced for records 001/002/003/005; 010 composition not yet built |
| X3 | conformance | any requirement on how credentials are issued is stated explicitly and early | 010 issuance (ZK-friendly signature or published commitment; issuer linkage MAY) · cred-spec #17 | **covered** |  |
| G1 | governance | a community declares whether private presentation is required, default, or optional | — (Governance; cred-spec §Governance Considerations) | **open** |  |
| G2 | governance | the assurance a proof carries is traceable to the governance of the issuing community | 010 disclosure set (assurance class via C's governance) · doesNotEstablish (admission correctness) | **covered** |  |

#### Acceptance tests → fixture families

| test | the record's words | family |
|---|---|---|
| Accepts | a valid proof verifies and a written analysis shows the verifier learned the outcome and nothing more | `accepts` |
| Rejects | with the voucher not a member of C the proof cannot be produced or does not verify — clause 3 does real work | `rejects-unsat / rejects-verify (the construction distinguishes the two)` |
| Unlinkable | two proofs from the same credentials given to two verifiers who compare cannot be linked | `unlinkable` |
| Current | after the voucher's membership is revoked a proof relying on it stops verifying within the published bound | `current` |

#### Explicitly out of scope in the record

- the proof system (format-agnostic)
- the construction — how the three clauses combine into one proof (the construction record's method)
- how a registry publishes what proofs rely on (the set-root primitive is one answer)
- encodings and wire formats
- identity proofing itself
- delegation chains — 'the natural next case' → construction 020

#### What the record asks of each group

| group | ask |
|---|---|
| DTG Credentials | confirm the three clauses match the specified Community-Anchored Proof; identify issuance requirements |
| ZKP | choose the construction, build a reference prover and verifier, publish X1/X2 vectors |
| Trust Registry | define what a registry exposes for a proof to rely on; how currency and revocation are expressed |
| Governance | how a community declares private presentation policy; how assurance traces to governance |

#### Consequences the record names

- requirements on issuance reach every issuer — hence X3 early
- registries gain a new obligation; revocation must be rethought so currency checks do not undo privacy
- real cost on holder devices — hence D1
- a harder first milestone by intent
<!-- generated-section:requests:end -->

## Public Inputs — Shared Conventions

This section is informative in this Working Draft. Each convention names the construction record and the upstream thread it came from; the conventions are offered for ratification, not asserted.

Construction records share the concepts below. Their exact encodings, profile versions and verifier checks remain to be specified before cross-implementation compatibility can be claimed.

### Context descriptor

A structured description of the context a proof is made in — scope, purpose, epoch — whose digest is converted under an explicit profile into the field input of a context-scoped gadget. The nullifier binds to it (construction 002); the per-context pseudonym of the blinded binder derives from it (construction 008); the quiet-presentation leakage budget is stated against it. The descriptor is data a holder can read before presenting: the six questions of a context card are derived from it, not from prose beside it.

*Source: proof-of-liveness requirements v0.4 §6.1 (working context definition B1); evidence-repository instruments `canonical/` and `context-card/`.*

### Set roots

> **WG-05 — Discuss: registry contract.** Which authority, authenticated leaf format, root construction and witness-update policy does the first profile use? Agree maximum staleness and unavailable-registry behavior. Status: unresolved; a shared commitment interface does not settle the meaning of its leaves.


A [[ref: set root]] is a signed, published commitment to a set at a stated registry state. Three sets that the credentials specification treats separately are one object in these conventions:

- the **membership root** of a community — construction 001 proves a hidden leaf is under it;
- the **revocation root** at an epoch — construction 006 proves a hidden handle is *not* under it;
- the **accredited-issuer root** of a trust registry — construction 001 again, under its alias *issuer-as-predicate*: prove the issuer belongs to the set rather than naming it, because the observer is often a venue or event and therefore the most identifying element.

A presentation exposes the accepted root and a zero-knowledge proof of the relevant membership statement. Paths, openings and non-membership witnesses remain private proof inputs unless disclosure is explicitly declared. Self-contained evidence can avoid per-holder verifier lookups, but obtaining and refreshing roots and witnesses still requires a deployment policy describing its correlation surface. Roots, paths and status witnesses can be supplied by any party that holds them — a cache, a relay, the community's agent — so that no member need be online for another's presentation (the 8 September call named this ambient verifiability, after KERI); who supplies them is part of the registry-operator adversary a profile states, not of the proof.

**The registry profile determines the root construction.** A proving backend must support the published hash, leaf semantics and root authentication, or use an explicitly specified issuance/registry change. The lab uses Poseidon trees. Flock has author-reported standard-hash benchmarks, but those do not establish end-to-end costs for these credential constructions or resolve signature verification.

*Source: cred-tf #40 (the set-root unification, stormer78 2026-08-22; ScottJeezey 2026-08-24: "ours to pressure-test"); cred-tf #39 (ScottJeezey 2026-08-25).*

### Epoch and freshness

Three clocks, never collapsed: the proof artefact's freshness (the transcript's challenge), the credential's validity, and the status root's epoch. A construction's [[ref: horizon]] is the earliest of the clocks it depends on. "Current" in a fixture family means: the roots named as public inputs are the ones the verifier accepts for this epoch.

*Source: proof-of-liveness requirements v0.4 §8 (three freshness clocks), §10 (cryptoperiod and assurance horizon).*

### Transcript digest

A presentation binds a transcript containing the authenticated request context and declared disclosures. Its exact serialization, hash framing and scalar conversion belong to the selected profile. The current lab encoding and a credential-layer digestMultibase representation are different interfaces; they are not interchangeable merely because both use SHA-256.

#### Observed lab transcript encoding

The inspected reference implementation computes the following procedure. This documents the lab baseline, not an adopted cross-implementation profile:

1. Validate the transcript with the lab's current field-presence checks and serialize it with runtimes/canonical/canonical.mjs. That serializer sorts object keys and uses JSON serialization for primitive values. The existing fixtures do not establish full RFC 8785 conformance.
2. Let D be the UTF-8 bytes of the exact domain string dtg-zkp/transcript/v0 and J be the UTF-8 bytes of that serialized transcript.
3. Compute H = SHA-256(u32be(byteLength(D)) || D || u32be(byteLength(J)) || J), where u32be is an unsigned four-byte big-endian length and || denotes byte concatenation. Reject an input length that cannot be represented in four bytes.
4. The lab returns H as 64 hexadecimal characters. The circuit harness interprets those digest bytes as a big-endian unsigned integer and reduces that integer modulo the BN254 scalar prime p = 21888242871839275222246405745257275088548364400416034343698204186575808495617.
5. The resulting scalar is the circuit's public transcriptDigest input. It is not the full 256-bit digest and is not a digestMultibase string.

The context descriptor uses the same argument-framing pattern with the separate domain dtg-zkp/context-descriptor/v0. Changing the domain, framing, serializer or conversion changes the profile and requires versioned vectors. Do not silently replace the framed hash with plain SHA-256(J).

The observed component circuit has four public inputs in declaration order: context, root, nullifier, transcriptDigest. It does not supply the status-root and epoch inputs of a complete community-anchored profile. Check the compiled artifact manifest before claiming the same layout for another build.

#### Draft profile and verifier validation

For an interoperable profile, specify whether its canonical JSON is RFC 8785, how the domain-separated digest is serialized externally (including any Multibase/Multihash identifiers), and how external digest bytes map to circuit inputs. A change from the lab baseline is explicit and preserves old fixture versions. The verifier derives the expected context and transcript values from the authenticated request and compares them with the presented public inputs; it does not accept an independently supplied scalar as evidence of the request's meaning.

The reference gadget reports one additional constraint for binding an already supplied scalar. That measurement does not include JCS or SHA-256 computation inside the circuit, strict request validation, simulation extractability or same-request replay prevention.

> **WG-06a — Proposed encoding decision.** Preserve the documented domain-separated lab hash as a named versioned baseline, and select the external digest representation and strict payload schema explicitly. Confirm the canonicalization requirements and conversion vectors before adopting a wire profile. Status: draft proposal; current lab behavior is evidence, not group ratification.

*Evidence: local source review and probe, 8 September 2026; canonical.mjs, nullifier.mjs H function, circom-gadget/harness.mjs and nullifier_membership.circom. See research/transcript-profile-vector.json for the reproducible synthetic comparison. Credential-layer encoding references: cred-spec #17 and #31.*

### Declared correlation scope

Under the credentials specification's Working Draft 02, an identifier carries a holder-declared [correlation scope](https://github.com/trustoverip/dtgwg-cred-spec/pull/30) — `pairwise | directed | public`, monotonic — and roles come from credentials. The scope is a public input where a construction's disclosure depends on it: a `pairwise` identifier appears in a proof only behind a commitment; a `directed` persona identifier may be shown on purpose (construction 011); and whether a proof of common control is needed at all is decided by the declaration (constructions 007 and 012: no proof where one `directed` or `public` identifier was deliberately reused). Where a declaration is carried remains a profile dependency: the credentials specification has settled that the declaration lives in the credential, made by the issuer about its own identifier, but the property that carries it and its `@context` term are not yet named ([cred-spec #46](https://github.com/trustoverip/dtgwg-cred-spec/issues/46)); the verifier needs an authenticated source for the declaration, and its encoding can change the construction inputs.

*Source: cred-spec #22, §Correlation Scope and §Choosing a scope; cred-tf #41; cred-spec #46 (the property and `@context` term, open).*

### Public-signal order (offered for ratification)

> **WG-06 — Evidence request before ratification: wire manifest.** Replace the logical sketch below with a versioned, fixed-arity manifest checked against the compiled circuit and cross-language vectors. Include digest conversion, audience binding and optional-nullifier variants. Status: the sketch is not an adopted wire format.


An earlier composite sketch listed the following logical inputs; it is not the measured reference gadget's wire manifest:

```
[context, root_C, rl_root, epoch, nullifier?, transcriptDigest]
```

This sketch does not define a wire order. Each circuit/profile publishes a fixed-arity ordered manifest, including types, encodings and domain separation. Profiles with and without a nullifier have distinct layouts; verifiers reject unknown layouts. Reconcile the manifest with the compiled circuit and canonical fixtures before claiming interoperability.

*Source: evidence-repository `circom-gadget` signal order; construction 010 public-signal expectation.*

<!-- generated-section:constructions:start -->
## Construction Records

This section is informative in this Working Draft: every record below is at state `specified` or `constructed`. Evidence maturity is printed at the head of each record. Normative adoption is a separate task-force decision; reproduction or publication alone does not confer it.

This section is generated from the machine-readable records in `conformance/records/`. Changes are made to a record, never to this text; a record that fails validation does not render. Each record states its adversary, its horizon and what it does not establish, and labels conjecture as conjecture, because the validator refuses records that do not.

### Index of constructions

Identifiers are stable handles, not a type taxonomy or contiguous sequence. The kind column identifies primitives and compositions. Records 020 and 021 concern chains, 022 is a digest-reference primitive, and 023 and 024 concern admission; unassigned numbers are not missing records.

**Primitive constructions** — one gadget each.

| # | construction | state | priority | gadget |
|---|---|---|---|---|
| [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) | Set membership over an accredited root | `constructed` | P1 | set-membership |
| [002](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection)) | Scoped nullifier (reuse detection) | `constructed` | P1 | nullifier |
| [003](#construction-003-%C2%B7-transcript-binding) | Transcript binding | `constructed` | P1 | transcript-bind |
| [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) | Holder binding (key from secret) | `specified` | P2 | key-binding |
| [005](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer) | Distinct member / distinct issuer | `constructed` | P1 | distinctness |
| [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) | Non-revocation against a status root | `specified` | P1 | non-revocation |
| [007](#construction-007-%C2%B7-common-control-across-identifiers) | Common control across identifiers | `specified` | P1 | key-binding |
| [008](#construction-008-%C2%B7-blinded-binder-(taskcontext-hiding%3B-presentation-correlation-unresolved)) | Blinded binder (taskContext hiding; presentation correlation unresolved) | `specified` | P2 | commitment-open |
| [009](#construction-009-%C2%B7-hidden-value-equality-across-credentials) | Hidden-value equality across credentials | `specified` | P1 | hidden-equality |
| [022](#construction-022-%C2%B7-blinded-digest-references-%E2%80%94-the-digest-valued-members-of-the-credential-specification%2C-unenumerable-at-rest-and-openable-in-proof) | Blinded digest references — the digest-valued members of the credential specification, unenumerable at rest and openable in proof | `specified` | P2 | commitment-open |

**Composed constructions** — a named conjunction under one transcript and one disclosure set.

| # | construction | state | priority | composes |
|---|---|---|---|---|
| [010](#construction-010-%C2%B7-community-anchored-proof-(adr-001)) | Community-Anchored Proof (ADR-001) | `specified` | P1 | 001 ∧ 002 ∧ 003 ∧ 004 ∧ 005 ∧ 006 ∧ 007 |
| [011](#construction-011-%C2%B7-pairwise-edge-(vrc-possession%2C-directed-personas-shown%2C-pairwise-identifiers-hidden)) | Pairwise edge (VRC possession, directed personas shown, pairwise identifiers hidden) | `specified` | P2 | 003 ∧ 004 ∧ 006 ∧ 007 |
| [012](#construction-012-%C2%B7-intentional-correlation-%E2%80%94-one-controller-across-k-credentials) | Intentional correlation — one controller across k credentials | `specified` | P2 | 003 ∧ 006 ∧ 007 |
| [013](#construction-013-%C2%B7-mutual-edge-admissibility-%E2%80%94-each-half-admissible-under-the-other-community%E2%80%99s-policy%2C-neither-policy-nor-member-revealed) | Mutual edge admissibility — each half admissible under the other community's policy, neither policy nor member revealed | `specified` | P3 | 001 ∧ 003 ∧ 022 |
| [020](#construction-020-%C2%B7-delegation-chain-(vdc)-%E2%80%94-agent-acts-for-a-member) | Delegation chain (VDC) — agent acts for a member | `specified` | P2 | 001 ∧ 003 ∧ 004 ∧ 006 ∧ 009 |
| [021](#construction-021-%C2%B7-authority-chain-(vac)-%E2%80%94-an-agent-or-device-acts-as-itself-under-attenuated-authority) | Authority chain (VAC) — an agent or device acts as itself under attenuated authority | `specified` | P2 | 001 ∧ 003 ∧ 004 ∧ 006 ∧ 009 |
| [023](#construction-023-%C2%B7-two-vouch-admission-proof-%E2%80%94-an-applicant-proves-k-%E2%89%A5-2-vouches-from-distinct-current-members-to-the-issuing-community%2C-without-disclosing-which-members) | Two-vouch admission proof — an applicant proves k ≥ 2 vouches from distinct current members to the issuing community, without disclosing which members | `specified` | P1 | 001 ∧ 003 ∧ 004 ∧ 005 ∧ 006 ∧ 007 |
| [024](#construction-024-%C2%B7-hidden-vetting-admission-%E2%80%94-an-applicant-proves-k-attestations-from-pairwise-distinct-eligible-vetters-of-the-issuing-community%2C-without-disclosing-which-vetters) | Hidden-vetting admission — an applicant proves k attestations from pairwise-distinct eligible vetters of the issuing community, without disclosing which vetters | `constructed` | P1 | 001 ∧ 002 ∧ 003 ∧ 004 ∧ 005 |

### Construction 001 · Set membership over an accredited root

*This record is at state `constructed`: a runtime exists and has measured at least one construction option; no independent party has reproduced it. Informative.*

| | |
|---|---|
| kind | primitive |
| state | `constructed` |
| priority | P1 |
| constructor | mitchuski |
| requested by | talltree/ScottJeezey |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: set-membership]] gadget and nothing else.

#### Statement

A verifier learns that the holder's credential commitment is a leaf of a published membership root, without learning which leaf.

**Need.** the membership leg every DTG proof stands on (VMC from a VTC in the verifier's anchor set) · alias: issuer-as-predicate (ScottJeezey, cred-tf #39 2026-08-25) — prove an issuer belongs to an accredited set rather than naming it, because the observer is often a venue or event and therefore the most identifying element

#### Witness

*Never leaves the holder.*

- membership credential secret / commitment preimage (the VMC leaf)
- Merkle path (depth 20 ≈ 1M leaves) and path indices

#### Public inputs

- root — the accredited/membership root the verifier recognises (registry state)
- context descriptor digest

#### Relation

1. the committed leaf is in the tree at `root` — [[ref: set-membership]] · runtime `runtimes/circom-gadget/circuits/nullifier_membership.circom`

#### Disclosure set

- root
- context

#### Does not establish

- that the community's admission decision was correct (assurance boundary)
- that the leaf is current (see record 006)
- which member the holder is
- that the leaf authenticates an issuer-signed credential or binds its holder key; the enclosing profile must establish those facts and the registry leaf semantics

#### Adversary, per claim

- **verifier · verifiers-colluding** — leaf position hidden
- **registry-operator** — leaf hidden from the registry operator only if the root is fetched without a per-holder query

#### Horizon

- root cryptoperiod (root rotation)
- VMC validity

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify`

Vectors: `runtimes/fixtures/vectors`

Rejection codes: `root-unknown`, `path-invalid`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Groth16 / BN254 / Poseidon Merkle (lab) | 11,522 constraints · ~680 ms prove · ~8 ms verify · 721 B (with record 003 bound) | **measured** | CIRCUITS.md · registry 0002–0006 |
| KZG / accumulator membership (paper §3.7 hiding KZG) | constant-size opening; pairing verify | unmeasured | ePrint 2026/333 §3.7, §8 |
| Semaphore v4 tree (structurally conformant, byte-incompatible) | see cross-check | unmeasured | explorations/SEMAPHORE-V4-CROSSCHECK.md |
| Flock-class binary-field prover over a standard-hash (BLAKE3 or SHA-256) Merkle tree — the path is a batch of compressions, Flock’s native workload; removes the Poseidon requirement on the registry side and gives a post-quantum path (LIV-ALG-07) | unmeasured — conjecture: depth-20 path ≈ 20–40 compressions ≈ well under a millisecond of prover work per the published 82,100 compressions/s single-core figure; proof size in the hundreds of kB class | unmeasured | board/stacks/flock.json (blog.succinct.xyz/introducing-flock) |

#### Issuance requirements

- issuer publishes a ZK-friendly commitment per member (Poseidon leaf) or an accumulator
- the membership tree hashes leaves and internal nodes under separate domains, or every membership path has the tree's fixed depth (the lab's depth 20) — with neither, an internal node's preimage passes as a leaf (the formal model's counter-deployment `no_domain_second_preimage`)

#### Provenance

- cred-spec VMC
- liveness reqs v0.4 §13 (PR-UNQ membership leg)
- cred-tf #39 (ScottJeezey 2026-08-25): issuer-as-predicate named as a ZKP TF work item — this record
- registry: 0002–0006 (circuit reproduced, record-level run pending)
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | runtimes/01-uniqueness-nullifier NOTES + decision §13 |
| 2026-08-28 | `constructed` | mitchuski | circom-gadget nullifier_membership 10/10, numbers in CIRCUITS.md |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |
| 2026-09-23 | mitchuski | issuance requirement added from the formal model: leaf/node domain separation or a fixed path depth; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P001Membership.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.pathRoot over Formal.Gadgets.MTree — the membership clause as a Merkle path recomputation from the leaf hash to the root
- scope: the membership clause over abstract hashes; not the hash function's collision resistance, and not which registry state the root is (that the verifier recognises the root is registry policy)

| theorem | proves |
|---|---|
| `path_sound` | a path that hashes the leaf to the root proves the leaf is in the tree |
| `path_complete` | every leaf of the tree has such a path |
| `path_sound_fixed_depth` | for a tree of uniform depth d and a path of exactly d steps, soundness holds without leaf/node domain separation — the lab circuit's shape (depth 20) |
| `no_domain_second_preimage` | with variable-length paths and no domain separation, an internal node passes as a leaf (the second-preimage shape): the defence is required, not optional |

| hypothesis | carries |
|---|---|
| H-leaf, H-node | the leaf and node hashes are collision-free (modelled as injective) |
| H-domain or fixed depth | no leaf hash equals a node hash, or every path has the tree's fixed depth — one of the two must hold of the circuit (to discharge at the circuit gate) |


### Construction 002 · Scoped nullifier (reuse detection)

*This record is at state `constructed`: a runtime exists and has measured at least one construction option; no independent party has reproduced it. Informative.*

| | |
|---|---|
| kind | primitive |
| state | `constructed` |
| priority | P1 |
| constructor | mitchuski |
| requested by | ScottJeezey |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: nullifier]] gadget and nothing else.

#### Statement

A verifier learns a deterministic nullifier for this context so a second presentation in the same context is detectable, while presentations in other contexts stay unlinkable.

**Need.** PR-UNQ: one presentation per (context, scope, purpose, epoch) — not 'one unique human'

#### Witness

*Never leaves the holder.*

- holder secret (the same one behind the membership leaf)

#### Public inputs

- context descriptor (scope, purpose, epoch)
- root / enrolment population id
- nullifier — the output

#### Relation

1. nullifier = H(secret, root, scope, purpose, epoch) computed in-circuit from the same secret as the membership leaf — [[ref: nullifier]] · runtime `runtimes/circom-gadget/circuits/nullifier_membership.circom`

#### Disclosure set

- nullifier
- context

#### Does not establish

- one natural person globally
- one enrolment per issuer or ecosystem (second point of the trade curve — governance, not cryptography)
- cross-context uniqueness

#### Adversary, per claim

- **verifiers-colluding** — unlinkable across contexts
- **verifier** — linkable within a context by design (declared, governed)

#### Horizon

- epoch rollover
- enrolment-root cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `unlinkable`

Vectors: `runtimes/fixtures/vectors`

Rejection codes: `nullifier-reused`, `context-descriptor-mismatch`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Poseidon nullifier in the membership circuit (lab) | included in record 001's 11,523 | **measured** | CIRCUITS.md |
| PRF-derived context key k_{U,ctx} = PRF_K(I‖ctx) (paper §7.2) | one PRF evaluation in the commit-and-prove SNARK | unmeasured | ePrint 2026/333 §7.2.2 |

#### Issuance requirements

- none beyond the credential as specified

#### Provenance

- liveness reqs v0.4 PR-UNQ
- decision §13 scoped reuse detection
- ePrint 2026/333 §5.3 (impossibility → contexts)
- registry: 0002–0006
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | decision §13 + O2 PHC-by-nullifier |
| 2026-08-28 | `constructed` | mitchuski | same circuit as 001; scoped preimage tested 9/9 in runtime 01 |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P002Nullifier.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.NullifierRel — some secret behind this leaf gives this nullifier in this context
- scope: reuse detection over abstract hashes; unlinkability across contexts is a computational claim outside this model; does-not-establish 2 is governance

| theorem | proves |
|---|---|
| `one_leaf_one_nullifier` | one leaf has one nullifier per context |
| `reuse_refused` | nullifier-reused: once a leaf's nullifier is spent in a context, a second presentation of that leaf there is refused |
| `unbound_nullifier_unsound` | the same-secret clause is necessary: a nullifier taken from any secret lets one leaf yield two nullifiers in one context |
| `cross_context_not_detected` | does-not-establish 3: one secret gives different nullifiers in different contexts — no cross-context detection |
| `one_person_two_nullifiers` | does-not-establish 1: one person enrolled twice gives two accepted nullifiers in one context |

| hypothesis | carries |
|---|---|
| H-leaf | the leaf commitment binds its secret (modelled as injective) — carries the same-secret clause |
| spent set | the verifier keeps the context's spent nullifiers (Formal.Gadgets.Spent) |


### Construction 003 · Transcript binding

*This record is at state `constructed`: a runtime exists and has measured at least one construction option; no independent party has reproduced it. Informative.*

| | |
|---|---|
| kind | primitive |
| state | `constructed` |
| priority | P1 |
| constructor | mitchuski |
| requested by | ScottJeezey |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: transcript-bind]] gadget and nothing else.

#### Statement

A verifier checks that the proof binds the supplied transcript scalar. Interpreting that scalar as this authenticated request also requires the profile’s canonical encoding, digest conversion, audience and freshness checks.

**Need.** PR-FRE: a proof bound to this challenge and this presentation, unreplayable elsewhere

#### Witness

*Never leaves the holder.*

- nothing extra — the transcript digest is bound as a public input inside the proof

#### Public inputs

- transcriptDigest — profile-defined circuit scalar; the lab hashes domain-separated, length-prefixed canonical transcript bytes and reduces the digest modulo the BN254 scalar prime

#### Relation

1. the proof's public signals include transcriptDigest and the circuit constrains it (cannot be swapped post hoc) — [[ref: transcript-bind]] · runtime `runtimes/canonical + runtimes/circom-gadget (public signal 4)`

#### Disclosure set

- transcriptDigest

#### Does not establish

- freshness beyond what the challenge carries
- that the verifier's challenge was itself honest
- correct JCS/SHA-256 evaluation inside the circuit merely because a supplied scalar is constrained
- simulation extractability or equivalence to tag-based SE-NIZK
- rejection of reuse of the same transcript without a verifier replay policy
- strict transcript field types or expiry semantics from the lab field-presence validator; those require the selected payload schema and verifier policy
- that a bound predicate identifier is meaningful or accepted: the digest distinguishes strings (equal strings bind equally; NFC and decomposed spellings, and `v1` and `v2` identifiers, bind differently) but the reference encoder accepts an unknown predicate string structurally — which identifiers a verifier accepts, and under which comparison rule and definition revision, is the profile's decision before binding (cred-spec #52; probe of 2026-09-14)

#### Adversary, per claim

- **verifier · verifiers-colluding** — changing the constrained transcript scalar invalidates the proof; audience/time replay protection additionally assumes the verifier checks the authenticated request and its validity window

#### Horizon

- the transcript's own validity window

#### Conformance fixtures

Families: `accepts` · `rejects-verify`

Vectors: `runtimes/fixtures/vectors`

Rejection codes: `transcript-digest-mismatch`, `bare-nonce-insufficient`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| public-signal binding in Groth16 (lab) | +1 constraint (11,522 → 11,523) | **measured** | CIRCUITS.md |
| tag-based SE-NIZK: tag = transcript (paper Def. 5) | unmeasured; proof-system security requirement, not established by the lab public-input binding | unmeasured | ePrint 2026/333 §3.5 |
| Flock-class prover binding a SHA-256 digestMultibase transcript digest natively (no Poseidon detour for the canonical transcript) | unmeasured — one SHA-256 compression per 64-byte block of the canonical transcript | unmeasured | board/stacks/flock.json |

#### Issuance requirements

- none beyond the credential as specified

#### Provenance

- liveness reqs v0.4 PR-FRE
- decision §15 canonical transcript
- cred-spec #17 item 1 (JCS)
- 2026-09-14 predicate-binding probe over runtimes/canonical/canonical.mjs (sha256 d50fd9a3…): four assertions — equal/NFC-vs-NFD/v1-vs-v2/unknown-accepted; encoding evidence, not registry-aware verification (peer-lane research cycle 2026-09-14-vocabulary-binding)
- registry: 0002–0006
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | X2 context legibility + canonical runtime 11/11 |
| 2026-08-28 | `constructed` | mitchuski | +1 constraint measured, CIRCUITS.md |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-14 | mitchuski | cred-spec #52 (predicate registry): negative-space line — binding an identifier does not make it accepted or meaningful; probe cited in provenance. State unchanged (constructed). |
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P003Transcript.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: the proof's public transcript scalar; H-bind states that a verifying proof was made for the scalar it verifies against
- scope: binding of the scalar only; canonical encoding, audience, freshness and replay policy are the profile's (does-not-establish 1, 2, 5, 6, 7 out of scope by construction; the replay remedy is a verifier spent set)

| theorem | proves |
|---|---|
| `swap_fails` | a proof moved to another transcript scalar fails |
| `digest_reduction_collides` | the 256-bit digest reduced mod the BN254 scalar prime is not injective (0 and r collide): the proof binds the scalar, and the scalar stands for one request only through the digest's collision resistance — does-not-establish 3 made exact |

| hypothesis | carries |
|---|---|
| H-bind | soundness of the proof system with the transcript scalar as a public input |


### Construction 004 · Holder binding (key from secret)

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski |
| requested by | ScottJeezey |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: key-binding]] gadget and nothing else.

#### Statement

A verifier learns that the presenting key is derived from the same secret the credentials were issued to, without learning the secret or any long-term key.

**Need.** PR-HLD / ADR-001 S4: the presenter is the subject, not a party holding a copy

#### Witness

*Never leaves the holder.*

- holder PRF key / secret
- derivation randomness

#### Public inputs

- presentation key or its commitment
- transcriptDigest

#### Relation

1. pk_presentation = PRF(secret, context) and the credential commitment opens to the same secret — [[ref: key-binding]]

#### Disclosure set

- presentation key (context-scoped)

#### Does not establish

- non-transfer of the secret
- absence of coercion or account sharing
- agent authority or consent
- compatibility with non-exportable keys or arbitrary credential key formats; the selected profile must bind the actual authenticated holder key to the available witness

#### Adversary, per claim

- **verifier · issuer-verifier-colluding** — long-term key hidden

#### Horizon

- key cryptoperiod
- credential validity

#### Conformance fixtures

Families: `accepts` · `rejects-unsat`

Rejection codes: `key-not-derived-from-secret`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| EdDSA/Baby-Jubjub identity in-circuit (lab plan) | unmeasured | unmeasured | PATH-MAP P4 open item |
| L_PRF Schnorr proof of PRF-key ownership (paper §9.1) | one Schnorr proof | unmeasured | ePrint 2026/333 §7.2.2, §9.1 |

#### Issuance requirements

- issuer binds the credential to a commitment to the holder secret (pk_U ‖ com_att signed — paper §2.4)

#### Provenance

- liveness reqs v0.4 PR-HLD
- ADR-001 S4
- commit: github.com/mitchuski/dtgwg-zkp-mage (runtimes/04 = stub)

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | runtimes/04-holder-binding STUB.md + paper §7.2 languages |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P004HolderBinding.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.HolderRel — pk = PRF(s, context) for an s the credential commitment opens to
- scope: derivation from the credential secret; non-transfer, coercion and sharing (does-not-establish 1, 2) are out of scope by construction — the relation reads a secret and names no party

| theorem | proves |
|---|---|
| `key_from_credential_secret` | every opening of the credential's commitment gives the same presentation key: the key is derived from the secret the credential was issued to |
| `unbound_key_unsound` | the commitment clause is necessary: without it a key from any secret passes |

| hypothesis | carries |
|---|---|
| H-com | the credential commitment is binding (modelled as injective in (secret, randomness)) |


### Construction 005 · Distinct member / distinct issuer

*This record is at state `constructed`: a runtime exists and has measured at least one construction option; no independent party has reproduced it. Informative.*

| | |
|---|---|
| kind | primitive |
| state | `constructed` |
| priority | P1 |
| constructor | mitchuski |
| requested by | mitchuski |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: distinctness]] gadget and nothing else.

#### Statement

A verifier learns that two credentials in one proof come from two distinct members (or issuers), with the duplicate case unsatisfiable rather than merely rejected.

**Need.** ADR-001 needs an S6: the voucher is not the holder; multi-issuer needs k distinct accredited issuers

#### Witness

*Never leaves the holder.*

- two credential leaves/secrets and their paths

#### Public inputs

- root(s)
- the two nullifiers or issuer ids as constrained public signals

#### Relation

1. leaf_a ≠ leaf_b (or issuer_a ≠ issuer_b) enforced as a non-zero inverse constraint — no witness exists for equality — [[ref: distinctness]] · runtime `runtimes/circom-gadget/circuits/dual_issuer.circom`

#### Disclosure set

- that the two are distinct — nothing about which two

#### Does not establish

- that the two parties are independent in the accreditation sense (declared, not proven — X8)
- that either is honest
- distinct natural persons or independent key controllers merely from distinct leaves, keys or issuer identifiers

#### Adversary, per claim

- **verifier · verifiers-colluding** — identities of both hidden

#### Horizon

- root cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat`

Vectors: `runtimes/fixtures/vectors`

Rejection codes: `duplicate-issuer-unsatisfiable`, `duplicate-seat-unsatisfiable`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| dual_issuer k=2 (lab) | 10,717 constraints (--O2) · ~740 ms · ~8 ms · 725 B | **measured** | CIRCUITS.md |
| guardian_threshold t=3 (lab) | 16,078 constraints · ~830 ms · ~10 ms | **measured** | CIRCUITS.md |
| f_distinct over context pseudonyms (paper §5.3) | predicate inside the commit-and-prove SNARK | unmeasured | ePrint 2026/333 §5.3 |

#### Issuance requirements

- none beyond the credential as specified

#### Provenance

- X8 multi-issuer aggregation
- ePrint 2026/333 §5.3 f_distinct
- registry: 0002–0006
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | X8 + dual_issuer design |
| 2026-08-28 | `constructed` | mitchuski | dual_issuer 7/7, guardian 8/8; duplicate = no witness |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P005Distinct.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: the inverse constraint (a − b) · inv = 1 over the circuit field Fin n; and Formal.Gadgets.owners_distinct, the k-ary form composed records use
- scope: unsatisfiability of the duplicate case, proved without cryptographic assumption; independence and honesty (does-not-establish 1, 2) are declarations outside any clause

| theorem | proves |
|---|---|
| `inverse_unsat_on_equal` | the duplicate case has no witness: (a − b) · inv ≠ 1 when a = b |
| `FinOps` | the three field facts the gadget needs hold for Fin n, n > 1 — so for the BN254 scalar field outright |
| `owners_distinct` | pairwise-distinct values, each owned under a one-value-per-owner rule, come from as many pairwise-distinct owners (record 024's soundness is an instance) |
| `distinct_leaves_not_distinct_persons` | does-not-establish 3: distinct leaves held by one person |

| hypothesis | carries |
|---|---|
| one value per owner | the issuer's enrolment gives each member one leaf — carries owners_distinct |
| inverse exists when a ≠ b | completeness of the constraint needs n prime; not proved here (circuit gate) |


### Construction 006 · Non-revocation against a status root

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P1 |
| constructor | mitchuski |
| requested by | stormer78 (ADR-001 C1–C4) |
| request | zkp-tf #18 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: non-revocation]] gadget and nothing else.

#### Statement

A verifier learns that the credential's nullifier is not in the revocation set at the stated registry state, without the registry learning who asked.

**Need.** a proof must not verify if any credential it relies on is revoked, and checking must not identify the holder

#### Witness

*Never leaves the holder.*

- the credential's revocation handle (scoped, never the static PHC attribute)
- non-membership witness against rl_root

#### Public inputs

- rl_root — revocation/status root at a stated epoch
- epoch

#### Relation

1. non-membership of the handle in the set committed by rl_root (sorted-leaf neighbours or accumulator non-witness) — [[ref: non-revocation]]

#### Disclosure set

- rl_root
- epoch

#### Does not establish

- that revocation is instantaneous — only that the handle was not revoked as of `epoch` (C4's published bound)
- that the registry's revocation decision was correct
- that the verifier performed no live lookup — the construction makes the presentation self-carrying (public root + ZK proof; witness remains private); whether a deployment still phones home is a profile statement, not a proof property

#### Adversary, per claim

- **registry-operator · issuer-verifier-colluding** — the status check does not identify the holder — requires bulk/anonymous root fetch, never a per-holder query

#### Horizon

- status freshness (C4 bound)
- epoch rollover

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `current`

Rejection codes: `rl-root-stale`, `handle-revoked`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| sorted-leaf non-membership Merkle (indexed tree) | ≈ 2× record 001 | unmeasured | explorations/O4-registry-zk-revocation.md |
| RL membership check inside f with nullifier as PHC attribute (paper §5.4) — carries the paper's own linkability caveat | depends on RL representation | unmeasured | ePrint 2026/333 §5.4 |
| set-root primitive (cred-tf #40 unification): a signed, published set root + a membership or non-membership proof carried in the presentation — accumulator non-membership witness as a private proof input; the same public-input object serves anchoring (record 001), revocation status (this record) and registry membership | unmeasured — ScottJeezey: "ours to pressure-test", priority | unmeasured | cred-tf #40 (stormer78 08-22; ScottJeezey 08-24) · cred-tf #39 (ScottJeezey 08-25) |
| Flock-class prover over an indexed (sorted-leaf) non-membership tree built with the registry’s existing standard hash — the set-root primitive without a hash migration | unmeasured — conjecture ≈ 2× the standard-hash membership path | unmeasured | board/stacks/flock.json · cred-tf #40 (set-root primitive) |

#### Issuance requirements

- registry publishes rl_root per epoch, fetchable without identifying the fetcher (X4)
- the registry commits rl_root over strictly sorted handles — adjacent neighbours prove a handle absent only over a sorted list; over an unsorted one they can bracket a handle that is present (the formal model's counter-deployment `unsorted_neighbours_unsound`)

#### Provenance

- ADR-001 C1–C4, T2
- explorations/O4
- explorations/X4
- explorations/X6
- cred-tf #40 / #39: set-root-plus-proof as one primitive; "no live lookups" as the privacy-profile default (profile default, not absolute — Sankarshan)

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | O4 + X6 explorations; ADR-001 C-clauses |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |
| 2026-09-23 | mitchuski | issuance requirement added from the formal model: rl_root over strictly sorted handles; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P006NonRevocation.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.Sorted and adjacent neighbours lo < h < hi in the committed revocation list
- scope: non-membership against the committed list; the registry's decision and live-lookup behaviour (does-not-establish 2, 3) are out of scope

| theorem | proves |
|---|---|
| `neighbours_sound` | two adjacent entries of a strictly sorted list bracketing h prove h is not in the list |
| `unsorted_neighbours_unsound` | sortedness is necessary: on an unsorted list adjacent neighbours bracket a present handle |
| `stale_root_passes` | does-not-establish 1: a handle revoked after the epoch still passes against the epoch's list |

| hypothesis | carries |
|---|---|
| H-sorted | the registry commits rl_root over strictly sorted leaves — a registry obligation, not a proof property |
| neighbour membership | the two neighbours are leaves under rl_root: record 001's clause, applied twice |


### Construction 007 · Common control across identifiers

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P1 |
| constructor | mitchuski |
| requested by | sankarshanmukhopadhyay / geoffturk / stormer78 (spec side) — specified by the ZKP TF co-chair |
| request | cred-spec #9 (Sankarshan: identity linkages the ZKP constructions require; talltree 09-08 translation; geoffturk 09-10: subject-or-issuer, non-correlation carried in the requirement, chain predicates named separately) · cred-spec #31 (geoffturk 09-02: '#9 stays open — and gains weight') · cred-spec §Community-Anchored Zero-Knowledge Proof ('the proof must establish common control across them') · cred-spec §Zero-Knowledge and Selective Disclosure editor's note (merged 2026-09-10): of the four predicates listed as resting on the #9 primitive, two rest on this record — the community-anchored proof and the VMC+VAC shared-subject rule where the two identifiers differ; the two chain predicates are hidden-value equality (record 009) inside records 020 and 021 |

**Kind:** [[ref: primitive construction]] — binds the [[ref: key-binding]] gadget and nothing else.

#### Statement

A verifier checks that two identifiers appearing in DTG credentials — as subject or as issuer, whatever correlation scope each declares — are under one controller, through the selected shared-secret derivation relation, while the secret remains private under the construction assumptions. The proof introduces no value that correlates across presentations; any cross-presentation correlation depends on the enclosing disclosure set and context policy.

**Need.** under WD02's three correlation scopes, two `pairwise` identifiers of one controller differ by construction, so any proof that reads one party out of two credentials must first prove one holder controls both identifiers — whether the identifiers appear as credential subject or as credential issuer (the VRC-issuer case is statement 3 of record 010) — without a field that says so

#### Witness

*Never leaves the holder.*

- the holder secret s
- per-identifier derivation material: for each identifier, the salt or key-derivation path under which it was minted from s
- the two credentials that name the identifiers (their bytes stay with the holder; only what the enclosing record discloses is shown)

#### Public inputs

- the two identifiers exactly as the credentials carry them — or their ZK-openable commitments, when the identifiers themselves are hidden by the enclosing record
- transcriptDigest — the presentation transcript this proof is bound to

#### Relation

1. identifier A's public key or commitment opens to (s, salt_A) and identifier B's opens to (s, salt_B) for one and the same s — a different secret behind either identifier is unsatisfiable — [[ref: key-binding]]

#### Disclosure set

- the outcome (one controller / not shown)
- transcriptDigest
- the identifiers only as far as the enclosing record already discloses them — this record adds no identifier to the disclosure set

#### Does not establish

- that the controller is one natural person — two agents or two people sharing a secret satisfy the clause (that is record 002's uniqueness, under its own declaration)
- that either credential is currently valid or unrevoked (record 006)
- that the holder intended the two identifiers to be correlated beyond this verifier — the proof is a disclosure to the party it is made to, not a widening of either identifier's declared scope
- the counterparty's common control: a presenter can prove only what is derived from a secret in the presenter's hands; a counterparty's linkage needs the counterparty's witness or the counterparty's own attestation (see record 010)
- that arbitrary independently generated or hardware-protected keys derive from a shared available scalar; derivation and custody are profile requirements
- a chain predicate: that a child credential's `issuer` equals its parent's `credentialSubject.id` across credentials signed by different parties involves no holder secret and is hidden-value equality (record 009 inside records 020 and 021), not common control — the two must not be read as one primitive (cred-spec #9, geoffturk 2026-09-10)

#### Adversary, per claim

- **verifier · verifiers-colluding** — no cross-presentation handle: the proof is transcript-bound and emits no identifier-derived value; two verifiers comparing proofs learn only what the enclosing records disclosed to each
- **issuer-verifier-colluding** — the secret s is never revealed and no per-identifier salt is; an issuer who minted one identifier's credential learns nothing about the other from the proof

#### Horizon

- the shorter of the two identifiers' key-validity periods — after a rotation the old key no longer opens to s under the recorded path and the clause must be re-proven against the rotated material
- the hash/commitment cryptoperiod of the derivation

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable`

Rejection codes: `co-control-unproven (unsat: distinct secrets)`, `identifier-not-zk-openable (verify-fail: identifier carries no openable commitment — an issuance failure, X3)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| no proof — the holder declares one `directed` identifier and uses it in both credentials (WD02 §Choosing a scope: correlation evident on the face of the credentials) | zero constraints; the cost is the declaration itself | unmeasured | cred-spec §Choosing a scope / §Community-Anchored Zero-Knowledge Proof |
| Groth16/Poseidon: two Poseidon commitment openings sharing the secret input (the circom-gadget leaf commitment, twice) — conjecture ~500–600 constraints total (~65%), unmeasured; one compile settles it | unmeasured (conjecture ≈ 2 × the lab's Poseidon leaf commitment) | unmeasured | runtimes/circom-gadget (Poseidon commitment + nullifier already bind a leaf to a secret) |
| blackbox commit-and-prove: the same opening under the paper's hiding commitments (Construction II show, N=1) | paper Table 1 class | unmeasured | ePrint 2026/333 §7.2 |

#### Issuance requirements

- each identifier that may need to be proven co-controlled must be, or carry, a ZK-openable commitment to the holder secret: a SNARK-native key (e.g. a BabyJubJub or BLS12-381 Multikey) or a published Poseidon/KZG commitment beside an Ed25519 key — the X3 requirement of record 010 applied to identifiers rather than signatures (cred-spec #17)
- the credential layer carries the requirement to be able to prove co-control, never a field that states the link (cred-spec #9, the 08-25 position)
- the credential layer's candidate requirement (cred-spec #9, 2026-09-10): a party controlling more than one DID appearing in DTG credentials, as subject or issuer, regardless of each identifier's declared scope, MUST be able to prove in zero knowledge that those DIDs are under its control, without disclosing them and without the proof introducing a value that correlates across presentations — this record is the construction that requirement points at; the requirement text is the credential specification's to write
- a VRC MAY carry its issuer's linkage proof to its VMC-side identifier (the one-line MAY proposed on #9, 2026-08-25 and taken up 2026-09-10) — the credential layer's member, produced by the counterparty at issuance by running this record; record 010 consumes it
- where the commitment lives — the task force's preference, stated for the credential specification to give it a home: as a verification method in the identifier's DID document (a `Multikey` entry carrying the commitment or the SNARK-native key), not as a member of any credential. The commitment is a property of the identifier — one per identifier, shared by every credential that names it, resolved the way a verifier already resolves the signing key — so it changes no credential schema and leaves the credential layer one requirement (which DID methods can carry it) instead of a member on every type. A credential member is the fallback only for a profile whose DID method cannot carry a second verification method
- the WD02 example set read against this line (the credential maintainer's answer to question 2 of zkp-tf #23): `did:key` over Ed25519 carries exactly one key and cannot carry a second verification method, so an Ed25519 `did:key` identifier has no place for the commitment and cannot satisfy this record as it stands; `did:peer` (numalgo 2 and 4) and `did:webvh` can carry one. The first implementation should mint the narrow-scope identifiers that may be co-proven as `did:peer` with the commitment as a second verification method, or as `did:key` over a SNARK-native key type — and that is the first thing for it to find out (WG-14)
- whether the requirement can be a MUST: yes, conditioned on the identifier's key profile rather than on a derivation the whole graph shares. The sentence should bind an identifier minted under a profile that declares co-control provable (a derivable key with a published commitment, or a SNARK-native key); a key held in a secure element with no available scalar is outside that profile, and co-control for it is established at issuance by the party who can prove it (the issuance-time attestation route of record 010) or not at all. WG-02's decision on derivation selects the first profile; it does not need to precede the sentence

#### Provenance

- cred-spec §Correlation Scope / §Choosing a scope / §Community-Anchored Zero-Knowledge Proof (correlation-scope revision, merged 2026-09-05)
- cred-spec #9 (identity linkages required by the ZKP constructions)
- cred-spec #31 disposition table (#9 stays open and gains weight; cross-TF work with the ZKP TF)
- ADR-001 S4 (holder binding) — generalised to two identifiers
- cred-spec #9 comments of 2026-09-08 (talltree) and 2026-09-10 (geoffturk): the requirement sentence, the subject-or-issuer widening, the chain-predicate caveat, the xref path
- cred-spec §Zero-Knowledge and Selective Disclosure (editor's note merged 2026-09-10) editor's note — 'what is waiting on the ZKP task force'
- zkp-spec PR #8 review (geoffturk, 2026-09-16, the credential maintainer's reading of the interface): records 007, 009, 010 and 011 confirmed against cred-spec #9 as settled on 2026-09-10; two asks new to the credential layer — where the ZK-openable commitment lives, and whether the requirement can be a MUST — answered in the issuance lines above

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-02 | `requested` | geoffturk / stormer78 (cred-spec #31, PR #30) · sankarshanmukhopadhyay (cred-spec #9) | cred-spec #31 disposition row for #9: 'four things lean on the unencoded common-control linkage … the resolution is cross-TF work with the ZKP task force' |
| 2026-09-05 | `specified` | mitchuski | specified from PR #30's §Community-Anchored text + #9 + the lab's key-binding gadget shape; cost line labelled conjecture per drafting rule 4 |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-11 | mitchuski | cred-spec #9 (2026-09-10): statement widened to identifiers held as subject or issuer; the '#31 four dependants' line narrowed — the VDC/VAC chain predicates are hidden-value equality (new record 009), not common control; candidate requirement sentence and the VRC-carried issuer linkage MAY recorded as issuance lines. State unchanged. |
| 2026-09-21 | mitchuski | Review of 2026-09-16 (credential maintainer): the commitment's home stated as a task-force preference (DID-document verification method; credential member only as fallback); the requirement sentence conditioned on a key profile rather than on universal derivation; the WD02 example set found unable to satisfy this record as it stands (Ed25519 `did:key` cannot carry the commitment) — WG-14. Citations by section title. State unchanged. |
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P007CommonControl.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.CommonControl — one secret behind both identifiers under the derivation
- scope: one secret behind two identifiers; one natural person, validity, intent, the counterparty's control and the chain predicate (does-not-establish 1–6) are out of scope by construction — the clause reads a secret

| theorem | proves |
|---|---|
| `distinct_secrets_unsat` | co-control-unproven: identifiers minted from different secrets have no witness |
| `common_control_complete` | identifiers minted from one secret always satisfy the clause |

| hypothesis | carries |
|---|---|
| H-com | the derivation binds (secret, salt) (modelled as injective) |

#### Reviews

Recorded reviewer sign-off on this record's clauses; a review is not a state advance and confers no evidence state.

| date | reviewer | scope | verdict | evidence |
|---|---|---|---|---|
| 2026-09-23 | Denys Popov (DenisPopov15) | clause 1's key-profile condition and the WG-14 finding | `refined` | PR #11 review on conformance/records/007.json: an Ed25519 did:key has no space for the commitment; did:peer (numalgo 2 and 4), did:web and did:webvh — and other methods with anchoring — can carry one |


### Construction 008 · Blinded binder (taskContext hiding; presentation correlation unresolved)

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski (record) · ScottJeezey (named the work item) |
| requested by | ScottJeezey for the ZKP TF · bmiller59 (#39 postulate) · sankarshanmukhopadhyay |
| request | cred-tf #39 (ScottJeezey 08-25: 'a blinded, non-correlating form of the binder (taskContext), with salted commitments available now and PRF-derived per-context pseudonyms as the fuller construction') · cred-spec §Trust Task Context Binding · cred-spec PR #18 (parked) · cred-tf #40 (the artifact gap) |

**Kind:** [[ref: primitive construction]] — binds the [[ref: commitment-open]] gadget and nothing else.

#### Statement

A verifier that holds a trust-task context learns that the presented credential was issued within that exchange, while the credential at rest and every other presentation of it carry no plaintext binder that recognises the exchange or the holder.

**Need.** a credential bound to the trust-task exchange it was issued in currently carries the binder in the clear; the binder (id/threadId pairing) is then a durable correlator across every presentation of that credential

#### Witness

*Never leaves the holder.*

- the taskContext value — under cred-spec PR #56 (open, 2026-09-17) the `id` of the exchange's initiating document together with `taskDigestMultibase`, the task digest of that document; before it, the id/threadId pairing
- the blinding salt u the issuer used when committing to it
- the credential carrying the commitment

#### Public inputs

- Route 1 currently treats commitment C as visible (digestMultibase-encoded). Repeated C values can correlate presentations; hiding C in a proof or a verifiable rerandomization route remains an unresolved design requirement.
- what the verifier already holds of the exchange: the taskContext digest it expects (route 1) or the context descriptor for the per-context pseudonym (route 2)
- transcriptDigest

#### Relation

1. Route 1: prove that C opens to (taskContext, secret blinding value) for the expected exchange. Route 2 (proposed): the holder proves correct derivation of a context pseudonym from its secret key and context descriptor; the verifier checks that proof without learning or recomputing with the holder secret. Both routes still require binding to the issuer-authenticated credential. — [[ref: commitment-open]]

#### Disclosure set

- the outcome (bound to this exchange / not shown)
- transcriptDigest
- route 2 only: the per-context pseudonym, which is by construction the same value every time this holder presents in this context — a declared, context-scoped link and nothing wider
- route 1 as currently specified: visible C, which is stable for this credential and can correlate presentations

#### Does not establish

- unlinkability of presentations carrying the same visible commitment C; hiding plaintext alone does not prevent equality-based correlation
- that the trust task completed, or what was done in it — completion evidence is a framework artifact outside any credential (the artifact gap, cred-tf #39/#40)
- that the binder's plaintext is not held elsewhere — the framework holds it in the Trust Task documents; this record blinds only the copy the credential carries
- durable-versus-task-dependent status of the claim (Outcome Interpretability is the credential layer's statement, not this proof's)

#### Adversary, per claim

- **verifier** — route 1 intends to hide a low-entropy taskContext from a verifier without the opening, assuming an independent uniformly random 128-bit secret blinding value and the commitment hash assumptions; a public or disclosed opening does not provide this protection
- **verifiers-colluding** — verifiers colluding across contexts can link any repeated visible C in route 1. Route 2 cross-context unlinkability is a design objective, not established by this record; it depends on PRF key secrecy, domain separation and the absence of other stable presentation identifiers
- **issuer-verifier-colluding** — the issuer that placed C and a verifier together can link C to the exchange (the issuer knows u) — stated, not hidden: issuer–verifier collusion is outside this record's protection

#### Horizon

- route 1 plaintext hiding lasts only while the opening remains secret from the named verifier and the hash assumptions hold. Closing a thread does not erase retained openings or prevent correlation through a retained visible C.
- route 2: the context descriptor's epoch; the pseudonym rotates with it
- the commitment's hash cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable`

Rejection codes: `binder-mismatch (unsat: C does not open to the supplied taskContext)`, `binder-plaintext-present (lint: credential carries taskContext in the clear beside C)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| route 1 — proposed salted-commitment opening using available primitives; no record-specific measured implementation | unmeasured — one Poseidon opening, conjecture ≈ 250–300 constraints (~70%) | unmeasured | runtimes/canonical + runtimes/circom-gadget |
| route 2 — PRF-derived per-context pseudonym (the record-002 nullifier construction with the context descriptor as domain) | the lab's domain-tagged nullifier: measured inside the 11,523-constraint gadget; standalone unmeasured | unmeasured | runtimes/circom-gadget (nullifier binds context; record 002) |

#### Issuance requirements

- issuers carry a committed form of the exchange citation in place of the plaintext pair — under cred-spec PR #56 that pair is `taskContext` (the initiating document's `id`) and `taskDigestMultibase` (its task digest), both durable correlators of the credential across presentations; this is a change to cred-spec §The `taskContext` Property and §The `taskDigestMultibase` Property, and the one member this record asks the credential layer for. Filed against the credential specification as issue #58 on 2026-09-21 (the review asked that it be an issue the Credentials TF can schedule; PR #18 is parked and is not it)
- C is digestMultibase-encoded (WD02 D-A) so both layers agree on the encoding
- the framework (Trust Tasks) commits to the taskContext in a form the proof can open — 'we can only blind what the framework gives us a committed form of' (ScottJeezey, cred-tf #39)
- Specify generation, distribution and retention of the secret blinding value; do not publish it beside a low-entropy plaintext-hiding commitment.

#### Provenance

- cred-tf #39 (ScottJeezey 2026-08-25 — ZKP TF work items on the record)
- cred-tf #40 (the artifact gap; delegation as a design-time case)
- cred-spec §Trust Task Context Binding / §The `taskContext` Property
- cred-spec #31 D-A (digestMultibase settled) · trustoverip/dtgwg-trust-tasks-tf#236 (§4.9.3)
- DTG ZKP TF meeting notes 2026-09-08 (Arka Rai Choudhuri): two parties who interact again in the same context reuse the same pseudonyms and can be linked; breaking that needs a new mechanism — the one case in which freshness does not hold; route 2's per-context pseudonym is that case by construction
- cred-spec PR #50 (2026-09-12) §The identity commitment: a per-application 32-byte salt carried inside the card to vetters and never to the community; every vetter recomputes the same commitment; a fresh salt per application leaves separate applications unrelated — this record's route 1 shape with the profile's own retention caveat (a vetter who keeps a card can recognise the commitment later)
- cred-spec PR #56 (albertoleon7794, 2026-09-17, open): `taskContext` becomes the initiating document's `id` (not the threadId) and gains `taskDigestMultibase`, the task digest of that document, taken with `proof` removed — a sixth digest-valued member, and a second plaintext correlator beside the first
- zkp-spec PR #8 review (geoffturk, 2026-09-16, the credential maintainer's reading of the interface): this record is scoped to the trust-task binder; the five digest-valued members of cred-spec #38 are record 022; the taskContext change is to be filed against the credential specification as an issue
- cred-spec #58 (filed 2026-09-21): a committed form of the trust-task citation — what the credential layer decides (placement, member names, encoding, retention of the blinding value, pairing with outcome evidence) and what stays here

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-25 | `requested` | ScottJeezey (ZKP TF co-chair, cred-tf #39) | cred-tf #39 comment 2026-08-25T14:41Z: 'we are treating these as work items: a blinded, non-correlating form of the binder (taskContext)…' |
| 2026-09-05 | `specified` | mitchuski | specified from Scott's two routes + cred-spec §Trust Task Context Binding + the lab's descriptor-digest and nullifier shapes; costs labelled conjecture |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-07 | reviewer (Codex; local editorial review) | Narrowed plaintext-hiding claims, made visible-C correlation explicit, corrected PRF verification and retention assumptions. Evidence state unchanged; design and implementation questions remain open. |
| 2026-09-11 | mitchuski | Provenance: the 8 September call's same-context pseudonym caveat recorded against route 2. State unchanged. |
| 2026-09-13 | mitchuski | Provenance: cred-spec PR #50's salted identity commitment recorded as an instance of route 1, with its retention caveat. State unchanged. |
| 2026-09-21 | mitchuski | Review of 2026-09-16: witness and issuance re-read against cred-spec PR #56 (initiating-document id + task digest); the #38 digest members moved to their own record, 022; the taskContext ask filed as a credential-specification issue. State unchanged. |
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P008Binder.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.BinderRel — the commitment C opens to the taskContext (route 1)
- scope: route 1 only; route 2 (context pseudonym) is proposed and unmodelled; completion, plaintext custody and outcome status (does-not-establish 2–4) are out of scope

| theorem | proves |
|---|---|
| `binder_opens_once` | binder-mismatch: one commitment opens to one taskContext |
| `visible_value_links` | does-not-establish 1 (the open design requirement): a shown value depending only on the credential is equal in every presentation, so equality correlates them — hiding the plaintext does not change that |

| hypothesis | carries |
|---|---|
| H-com | the commitment binds (taskContext, salt) (modelled as injective) |


### Construction 009 · Hidden-value equality across credentials

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P1 |
| constructor | mitchuski |
| requested by | geoffturk / stormer78 (cred-spec #9; §Zero-Knowledge and Selective Disclosure editor's note) — specified by the ZKP TF co-chair |
| request | cred-spec #9 (geoffturk 2026-09-10, point 4: 'a child's issuer MUST equal its parent's subject, so proving chain validity without disclosing the chain is hidden-value equality across credentials signed by different parties. No holder secret is involved … name the chain predicates separately so they do not appear covered while having no stated primitive') · cred-spec §Zero-Knowledge and Selective Disclosure editor's note (merged 2026-09-10): the VDC chain, the VAC chain and 'two credentials presented together share a subject' listed as predicates waiting on the ZKP task force |

**Kind:** [[ref: primitive construction]] — binds the [[ref: hidden-equality]] gadget and nothing else.

#### Statement

A verifier learns that a hidden field of one authenticated credential equals a hidden field of another authenticated credential — the two signed by different parties — without learning the value, and without the proof introducing a value that correlates across presentations.

**Need.** three credential-layer predicates need a proof that a hidden value in one signed credential equals a hidden value in another signed credential — a child hop's `issuer` and its parent's `credentialSubject.id`; the subject of a VMC and the subject of a VAC presented together — with no holder secret in the relation, so record 007 does not apply and nothing else names it

#### Witness

*Never leaves the holder.*

- the two credentials (their bytes stay with the holder) and the openings of the two committed fields being compared
- the issuer signatures or the leaf commitments that authenticate each credential's content — whichever the enclosing record binds (the equality clause on its own compares two witnesses; the enclosing record's signature-verify or set-membership clauses are what make them authenticated)

#### Public inputs

- whatever the enclosing record discloses of the two credentials — this record adds no public input of its own
- transcriptDigest — the presentation transcript this proof is bound to

#### Relation

1. field_a (opened from credential A's authenticated content) equals field_b (opened from credential B's authenticated content): field_a − field_b = 0 — a differing pair is unsatisfiable, and neither value is a public signal — [[ref: hidden-equality]]

#### Disclosure set

- the outcome (equal / not shown)
- transcriptDigest
- nothing about the value: no digest, commitment or pseudonym derived from it leaves the proof — the enclosing record decides what else is shown

#### Does not establish

- common control: equal identifiers in two credentials say that the same DID appears in both, not that the presenter controls it — key control is record 004, and two different identifiers under one hand is record 007
- that either credential is authentic, valid or unrevoked on its own — the enclosing record's signature-verify (or set-membership) and non-revocation clauses establish that; this clause compares two witnesses those clauses have already bound
- that the two credentials were meant to be presented together — intentional correlation is the holder's declaration (record 012), and a shared subject widens no identifier's declared scope
- anything about the value's meaning: an equal `issuer` and `credentialSubject.id` across two hops establishes the link the chain rule requires (cred-spec §Delegation Edges, §Attenuation) and nothing about who that party is

#### Adversary, per claim

- **verifier · verifiers-colluding** — the compared value is not disclosed and no value derived from it is emitted; two verifiers comparing proofs learn only what the enclosing records disclosed to each
- **issuer-verifier-colluding** — the issuer of either credential learns nothing about the other credential from the proof — the equality is proven over openings the holder supplies, never over a value the issuer can recognise in a public signal

#### Horizon

- the shorter of the two credentials' validity periods — the equality is a statement about two credentials as issued, and a re-issued credential must be re-compared
- the hash/commitment cryptoperiod of the leaf or field commitments the openings are made against

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable`

Rejection codes: `hidden-values-differ (unsat: the two openings are not equal)`, `field-not-openable (verify-fail: a compared field carries no commitment the proof can open — an issuance failure, X3 applied to the compared fields)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Groth16/Poseidon: two Poseidon openings of the compared fields (the circom-gadget leaf commitment, twice) plus one equality constraint — conjecture: the openings are the whole cost, the equality is one constraint | unmeasured (conjecture ≈ 2 × the lab's Poseidon leaf commitment + 1; one compile settles it) | unmeasured | runtimes/circom-gadget (Poseidon commitment already binds a leaf to hidden content) |
| as-signed credentials (ECDSA/Ed25519 rails): equality of two signed fields inside a Longfellow-class or ProveKit circuit — the signature checks are the cost, the equality is free | unmeasured; dominated by two signature verifications | unmeasured | board/stacks/siros-longfellow.json · board/stacks/provekit.json |
| no proof — the shared value is disclosed and compared in the clear (the credential specification's stated fallback: 'every requirement here can be checked by presenting the credentials themselves … the cost is privacy rather than correctness') | zero constraints; the cost is the disclosure of the whole chain or both subjects | unmeasured | cred-spec §Zero-Knowledge and Selective Disclosure editor's note, 'What holds until this work lands' |

#### Issuance requirements

- each field that may be compared must be, or carry, a ZK-openable commitment inside the authenticated credential content: for the chain predicates that is `issuer` and `credentialSubject.id` on every hop; for the shared-subject rule it is `credentialSubject.id` on the VMC and the VAC — the X3 requirement applied to the compared fields rather than to signatures (cred-spec #17)
- a credential whose compared field is a `pairwise` identifier that differs from the identifier in the other credential cannot satisfy this record by construction; the holder either declared one `directed` identifier for both, or the enclosing record composes record 007 for that pair instead

#### Provenance

- cred-spec #9, geoffturk 2026-09-10 point 4 — the chain predicates as hidden-value equality, to be named separately
- cred-spec §Zero-Knowledge and Selective Disclosure (editor's note merged 2026-09-10) — the VDC chain, the VAC chain and the shared-subject predicates
- cred-spec §Delegation Edges (acceptance `issuer` = grant `credentialSubject.id`), §Delegation Chains, §Attenuation (child `issuer` = parent `credentialSubject.id`), §Authority and membership are separate credentials (shared-subject proof when both are proven with the subject withheld)
- record 005 (distinctness) — the dual: a non-zero-inverse constraint proves ≠, a zero-difference constraint proves =
- zkp-spec PR #8 review (geoffturk, 2026-09-16, the credential maintainer's reading of the interface): "Record 009 is the right separation" — the chain predicates and the shared-subject rule are hidden-value equality, not common control; the credential specification's editor's note is to be reworked to two primitives (007, 009) citing 009, 011, 012 and 021
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-10 | `requested` | geoffturk / stormer78 (cred-spec #9, PR #42) | cred-spec #9 comment 2026-09-10T11:40Z: 'the VDC and VAC chain predicates are not this primitive … name the chain predicates separately'; PR #42 editor's note lists the three predicates |
| 2026-09-11 | `specified` | mitchuski | specified from the merged VDC/VAC chain rules and the shared-subject rule; bound to the new `hidden-equality` gadget (the dual of record 005's distinctness); cost lines labelled conjecture per drafting rule 4 — DRAFT for review, drop to requested if the task force prefers to bind this to commitment-open |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P009HiddenEquality.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: the constraint field_a − field_b = 0 over the circuit field Fin n
- scope: the equality clause alone; common control, authenticity, intent and meaning (does-not-establish 1–4) are out of scope by construction — the clause compares two witnesses

| theorem | proves |
|---|---|
| `sub_zero_iff_eq` | a − b = 0 exactly when a = b in Fin n, any n > 0 — the constraint is equality, both ways, without primality |

| hypothesis | carries |
|---|---|
| authenticated openings | the compared fields are the openings the enclosing record's signature-verify or membership clauses bind — carried by the enclosing record |


### Construction 010 · Community-Anchored Proof (ADR-001)

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P1 |
| constructor | mitchuski + DenisPopov15 (construction) · stormer78 (record) |
| requested by | stormer78 — ADR-001 Proposed 2026-08-25, docs.fpp.storm.ws |
| request | zkp-tf #18 (Scott 08-27: 'a natural first one to seed it with') |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [002](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection)) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [005](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [007](#construction-007-%C2%B7-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A maintainer checks authenticated evidence of a relationship between two distinct member credentials of community C, under the declared holder-linkage, validity and status assumptions. Hidden identifiers remain private against the verifier and colluding verifiers only under the stated construction assumptions, disclosure set and horizon; community roots, context, optional nullifier and other disclosed metadata remain visible.

**Need.** the first ZK use case against DTG credentials: a relationship exists inside a shared community, without revealing who is in it

#### Witness

*Never leaves the holder.*

- the VRC (the voucher → the presenter): the vouch, its statement, and the pairwise-scope identifier pair it was issued between
- the presenter's VMC from C — the community-issued grant (and the presenter’s acknowledgement half)
- the voucher's VMC grant from C as it sits in C's membership root (the leaf and its path — no copy of the voucher's acknowledgement exists on the presenter's side)
- the presenter's holder secret, and the derivation material linking the presenter’s VRC-side identifier to the presenter’s VMC-side identifier (record 007) — unless the presenter declared one `directed` identifier for both
- the voucher's linkage: either one `directed` identifier used in both the voucher’s VMC and the VRC (WD02's honest default for intra-community edges), or a co-control attestation the voucher issued alongside the VRC (record 007 run by the voucher at issuance — the vouch-under-community-credential shape of ePrint 2026/333); the presenter cannot derive this from the presenter’s own secret
- non-revocation witnesses for the VRC and both VMC handles

#### Public inputs

- context descriptor (scope, purpose, epoch)
- root_C — C's membership root at a stated registry state
- rl_root and epoch — revocation state
- nullifier (only if this context declares reuse detection; otherwise absent)
- transcriptDigest — one transcript for the whole show, including the maintainer's challenge

#### Relation

1. ADR clause 1 — the VRC verifies as a vouch made by the holder of the voucher's credential over the presenter's key — [[ref: signature-verify]]
2. ADR clause 2 — the presenter's VMC grant is a leaf of root_C — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
3. ADR clause 3 — the VRC issuer's VMC grant is a leaf of root_C (offline: proven from the root, not from the voucher) — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
4. S7 (WD02 §Community-Anchored Zero-Knowledge Proof) — the identifier the presenter used in the VRC and the identifier the presenter’s VMC grant names are controlled by one secret; likewise the voucher's VRC-issuing identifier and the voucher’s VMC-grant identifier (from the voucher’s linkage artifact, or trivially if the voucher used one `directed` identifier) — otherwise clauses 1–3 are about four unrelated identifiers — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-%C2%B7-common-control-across-identifiers))
5. S6 — the two authenticated member leaves are distinct; this rejects reuse of one leaf, but does not by itself reject one controller with multiple memberships — [[ref: distinctness]] ([[ref: construction record]] 005, [Distinct member / distinct issuer](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer))
6. S4 — the presenter's presentation key derives from the secret the presenter’s VMC/VRC bind to — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret)))
7. C1–C3 — neither VMC handle nor the VRC handle is in the set under rl_root at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
8. P4 (parameterised) — if the context declares reuse detection, emit the scoped nullifier; else emit none — [[ref: nullifier]] ([[ref: construction record]] 002, [Scoped nullifier (reuse detection)](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection)))
9. S5 — the whole show is bound to transcriptDigest — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the outcome (verifies / does not)
- root_C, rl_root, epoch (registry state the show was made against — ADR C2)
- context descriptor
- transcriptDigest
- nullifier — only in contexts that declare reuse detection
- anything the presenter deliberately discloses (P3), e.g. an assurance class carried by C's governance (G2)

#### Does not establish

- that the voucher endorses this request — a VRC is standing, not per-request; S5 binds the proof, not the relationship
- that the presenter is one natural person (that is PR-UNQ in a different context, record 002 under its own declaration)
- that C's admission decision for either member was correct (assurance boundary — accreditation carries assurance)
- the voucher's consent to this disclosure — the VRC's effective disclosure is the wider of its two halves (cred-spec §Privacy Considerations: the effective disclosure of an edge)
- that the voucher's membership was consented in the sense of §VMC (Verifiable Membership Credential), both directions — clause 3 proves the community-issued grant only; the acknowledgement half is not in the presenter's hands
- key non-transfer, absence of coercion, agent authority
- that the voucher is still a member in any sense stronger than 'not revoked as of epoch'
- that the voucher's two identifiers are co-controlled when the voucher supplied no linkage and used pairwise identifiers for both — then clause 3 is unprovable by the presenter, and the record says so rather than reading a link out of a field (cred-spec #9)
- distinct humans or controllers merely from unequal member leaves
- a complete implementation from the existence of component runtimes
- unconditional anonymity against network observers, hosted provers or unique disclosed context
- the voucher linkage from vetting evidence: an identity-vetting statement's `identityCommitment` (a salted commitment to the applicant's identity claims) or `livenessConfirmed` flag (cred-spec PR #50) is not the voucher-to-membership relation clause 4 needs — committing to identity claims does not supply it, and a vetting statement's own pass establishes neither admission, current membership nor identity truth

#### Adversary, per claim

- **verifier · verifiers-colluding** — P1/P2 — no pairwise-scope identifier of the edge, no counterparty identifier
- **verifiers-colluding** — P4 — proposed cross-context proof unlinkability against colluding verifiers, conditional on the selected proof system and absence of correlatable disclosures; context nullifiers intentionally link reuse and registry/context metadata can also correlate presentations
- **registry-operator · issuer-verifier-colluding** — C3 — currency check does not identify the presenter; holds only if rl_root/root_C are fetched without a per-holder query

#### Horizon

- earliest of: VRC validity · either VMC validity · epoch rollover · status freshness (C4 bound) · root_C cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `voucher-not-member (unsat)`, `self-vouch (unsat)`, `vrc-signature-invalid (verify)`, `transcript-digest-mismatch (verify)`, `handle-revoked (unsat at epoch)`, `rl-root-stale`, `vetting-evidence-not-linkage (unsat: a valid vetting statement and identity-claim commitment are presented, but no voucher-to-membership relation — a test requirement from the 2026-09-13 review, no vector yet)`, `liveness-flag-not-membership (lint: `livenessConfirmed` with no current membership evidence must not create a membership edge)`, `linkage-artifact-swapped (verify-fail: a correctly signed linkage artifact from another VRC issuer or membership credential is rejected under the selected linkage profile)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Groth16 / BN254 / Poseidon — candidate composition, with credential authenticity, holder linkage and non-revocation still requiring implementation | unmeasured for the complete statement; component figures cannot be added into a validated end-to-end estimate | unmeasured | CIRCUITS.md numbers per component |
| blackbox: Gro15 SPS credentials + hiding KZG + Groth–Sahai for the algebraic part, commit-and-prove SNARK for f (paper §8–9) | paper-reported benchmark pointer only; exact revision, workload and applicability to ADR-001 require verification | unmeasured | ePrint 2026/333 §10 |
| legacy rails: ECDSA/Ed25519 credentials proven as-signed (Longfellow / Crescent → vouchable, paper App. A) | unmeasured for the selected credential format and complete ADR-001 statement | unmeasured | zkp-tf #17 (SIROS catalog), ePrint 2026/333 App. A |
| post-quantum route: Flock-class binary-field prover for the hash side (membership, non-revocation, transcript) — signature clauses over curve-based credentials remain the open cost | unmeasured; proof size hundreds of kB vs ~1 kB Groth16 — a profile trade (ADR-001 D3) | unmeasured | board/stacks/flock.json |
| blind-signature vouch — the voucher blind-signs at vouch time and the presenter later proves possession of the unblinded signature under the community's key, so clause 3 is answered without the presenter holding the voucher's membership path or the voucher being online (the Berkeley team's current build, described on the 8 September call; not in ePrint 2026/333) | unmeasured — no construction published; whether the vouch verifies under the community's key or the voucher's, and how it composes with clauses 4 and 7, is the open design question | unmeasured | DTG ZKP TF meeting notes 2026-09-08 (Sanjam Garg, Arka Rai Choudhuri); a follow-up paper modelling the community was described as forthcoming |

#### Issuance requirements

- X3, concretely: the VMC and VRC signatures or a published commitment must be ZK-friendly — either SPS on BLS12-381 (blackbox), a SNARK-native signature, or an additional Poseidon/KZG commitment alongside `eddsa-jcs-2022` (cred-spec #17)
- C publishes root_C and rl_root per epoch, fetchable anonymously (T2, C3)
- membership leaf = the community-issued grant (§VMC, the membership pair): the proof covers the grant half
- a VRC issued from a pairwise-scope identifier by a member who wants it usable in community-anchored proofs carries the issuer's co-control attestation to their VMC-side identifier (record 007 at issuance) — or the member declares `directed` and uses one identifier; the credential layer names the option, not the link (cred-spec #9)

#### Provenance

- ADR-001 Community-Anchored Proof (Proposed 2026-08-25)
- cred-spec §Community-Anchored Zero-Knowledge Proof
- cred-spec §Edge Verifiability (issue #21; merged 2026-09-05)
- cred-spec §VMC (Verifiable Membership Credential), both directions (issue #8; merged 2026-08-28)
- ePrint 2026/333 §2.3–2.4, §5.3–5.4, §7.2, §8–10
- cred-spec §Community-Anchored Zero-Knowledge Proof: "the proof must additionally establish common control" · cred-spec #31 row #9
- cred-spec §Community-Anchored Zero-Knowledge Proof (merged WD02): a verifier 'SHOULD treat statement 3 as establishing that the community attested the VRC issuer's membership, and SHOULD NOT treat it as establishing that the issuer acknowledged that membership' — the record's does-not-establish line, now in the credential specification's own words
- DTG ZKP TF meeting notes 2026-09-08: ADR-001 confirmed as the first proof; the blind-signature vouch alternative; same-context pseudonym reuse as the one case where freshness does not hold; the credential signature scheme as the non-swappable choice
- cred-spec PR #50 (2026-09-12, supersedes #49): identity vetting as a community statement predicate — its identityCommitment and livenessConfirmed are not the voucher linkage; the card-digest byte-input ambiguity (received bytes vs canonical form) is carried, not adopted (review of 2026-09-13)
- zkp-spec PR #8 review (geoffturk, 2026-09-16, the credential maintainer's reading of the interface) — the answer to question 2 of zkp-tf #23 (which credential, holder-key and offline-voucher artifacts the first implementation supports): the WD02 examples as they stand — `did:key` and `did:peer` Ed25519 identifiers, the VMC pair with the community-issued grant as the membership leaf, a VRC issued from a `pairwise` identifier carrying the issuer's linkage proof under the MAY. Read against record 007's issuance line the Ed25519 `did:key` half of that set cannot carry the commitment (record 007, WG-14)
- cred-spec §Community-Anchored Zero-Knowledge Proof: Governance Considerations 1 confirmed to carry the membership-pair rule; statement 3's negative space confirmed to match the SHOULD / SHOULD NOT wording (review of 2026-09-16)
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | ZKP_TF_RUN-2026-08-28.md §3 (ten refinements) + this record; method fully bound to gadgets; composed disclosure set and does-not-establish written fresh |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-05 | mitchuski | WD02 three-scope vocabulary (PR #30); S7 common-control clause via record 007; voucher-side linkage stated as witness + issuance option — re-specified, state unchanged |
| 2026-09-11 | mitchuski | 8 September call and merged WD02 text: blind-signature vouch added as a construction option (conjecture; source = the call); provenance cites the merged §Community-Anchored ZKP whose SHOULD/SHOULD NOT on statement 3 matches this record's negative space. State unchanged. |
| 2026-09-13 | mitchuski | cred-spec PR #50 (vetting as a statement predicate, superseding #49): negative-space line — vetting evidence is not the voucher linkage; three fixture requirements recorded as rejection codes without vectors; provenance. State unchanged. |
| 2026-09-21 | mitchuski | Review of 2026-09-16: the credential maintainer's answer to question 2 of #23 recorded; the example set's Ed25519 `did:key` identifiers found unable to carry the commitment record 007 requires (WG-14). Citations by section title. State unchanged. |
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Composed/R010.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.R010.Accepts and soundness — two distinct members, both grants under the membership root, every handle unrevoked, a VRC from the voucher over the presenter's key
- scope: clauses 1–5 and 7; the presentation key (clause 6), the nullifier (clause 8) and the transcript (clause 9) are records 004, 002 and 003; distinct controllers from unequal leaves (does-not-establish 9) is record 005's negative space

| theorem | proves |
|---|---|
| `Formal.R010.soundness` | clauses 1–5 and 7 as gadget relations establish two distinct current members related by a VRC (record 001 twice, record 006 per handle) |
| `Formal.R010.self_vouch_unsat` | the rejection code self-vouch: one member leaf in both roles is never accepted (contrast record 023) |

| hypothesis | carries |
|---|---|
| H-sig | clause 1: a verifying VRC is one the voucher issued over the presenter's key |
| H-leaf, H-node, H-domain (record 001) | the membership tree's hashes are collision-free and leaves and nodes are hashed under separate domains |
| H-sorted (record 006) | the revocation root is committed over strictly sorted handles |
| H-com (record 007) | the identifier derivation binds (secret, salt) |


### Construction 011 · Pairwise edge (VRC possession, directed personas shown, pairwise identifiers hidden)

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski |
| requested by | cred-spec §Pairwise Zero-Knowledge Proof |
| request | zkp-tf #18 |

**Composes:** [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [007](#construction-007-%C2%B7-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A verifier learns that two disclosed persona identifiers (declared `directed`) hold a valid relationship credential between them, without learning the pairwise-scope identifiers under it and without a handle that correlates this presentation with any other.

**Need.** prove two known personas have a relationship without exposing the private pairwise channel (cred-spec §Pairwise Zero-Knowledge Proof, WD02 wording: disclose the parties’ `directed` persona identifiers while hiding the underlying `pairwise` ones)

#### Witness

*Never leaves the holder.*

- the VRC and the pairwise-scope identifier pair it was issued between
- the co-control witnesses linking each disclosed `directed` persona identifier to its hidden pairwise identifier (record 007; cred-spec #9: co-control proven in ZK, never a field) — the counterparty’s half is theirs to supply
- the presenter’s holder secret

#### Public inputs

- the two `directed` persona identifiers (disclosed on purpose)
- rl_root, epoch
- transcriptDigest

#### Relation

1. the VRC verifies under the issuing pairwise identifier’s key — [[ref: signature-verify]]
2. each disclosed persona identifier is co-controlled with its hidden pairwise identifier (record 007) — the presenter’s from their own secret, the counterparty’s from the counterparty’s attestation — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-%C2%B7-common-control-across-identifiers))
3. the presenter’s presentation key derives from the secret behind their pairwise identifier — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret)))
4. the VRC handle is not revoked at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
5. bound to one transcript — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the two `directed` persona identifiers
- rl_root, epoch
- transcriptDigest

#### Does not establish

- any community-level assurance (that is record 010)
- that the personas are distinct natural persons
- the relationship's content beyond what the statement discloses
- the counterparty’s persona↔pairwise linkage without the counterparty’s attestation (record 007 negative space)

#### Adversary, per claim

- **verifier · verifiers-colluding** — pairwise identifiers hidden; no cross-presentation correlator minted by the linkage itself

#### Horizon

- VRC validity
- status freshness

#### Conformance fixtures

Families: `accepts` · `rejects-verify` · `unlinkable`

Rejection codes: `co-control-unproven`, `vrc-signature-invalid`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Groth16 composition of 004+006+003 with an in-circuit signature check | unmeasured — dominated by the signature gadget (X3 again) | unmeasured | board/README |
| paper Construction II show with N=1 vouch (§7.2) | paper Table 1 | unmeasured | ePrint 2026/333 §7.2 |

#### Issuance requirements

- as record 010's X3 line

#### Provenance

- cred-spec §Pairwise Zero-Knowledge Proof (WD02 wording)
- cred-spec #9 (F post: co-control as requirement, not field)
- cred-spec §Correlation Scope
- zkp-spec PR #8 review (geoffturk, 2026-09-16, the credential maintainer's reading of the interface): record 011 answers the first two questions of cred-spec #9 implicitly — the persona-to-pairwise link is a co-control witness (record 007 in the presenter's hands), and the VPC plays no part in the proof; the counterparty's persona-to-pairwise link needs the counterparty's attestation, the same asymmetry as statement 3 of the community-anchored proof. Stated on #9 on 2026-09-21

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `specified` | mitchuski | cred-spec construction 1 + #9 F post |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-05 | mitchuski | WD02 vocabulary; co-control routed through record 007 — re-specified, state unchanged |
| 2026-09-21 | mitchuski | Review of 2026-09-16: the record's implicit answers to cred-spec #9's first two questions made explicit in provenance and stated on the thread; the counterparty-attestation asymmetry noted. State unchanged. |
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Composed/R011.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: clauses 2 and 3 composed: the presenter's key (record 004) and the persona ↔ pairwise linkage (record 007)
- scope: the composition of clauses 2 and 3; the VRC signature (1), revocation (4) and transcript (5) are records' own gadgets; the counterparty's linkage rests on its attestation

| theorem | proves |
|---|---|
| `Formal.R011.key_behind_persona` | the presenter's key is derived from the secret behind the disclosed persona, not only the hidden pairwise identifier |
| `Formal.R011.persona_linkage_needed` | without clause 2 the key check says nothing about the persona |

| hypothesis | carries |
|---|---|
| H-com (record 007) | the identifier derivation binds (secret, salt) |
| H-sig | clause 1: the VRC verifies under the issuing pairwise identifier's key (not modelled here) |


### Construction 012 · Intentional correlation — one controller across k credentials

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski |
| requested by | talltree / geoffturk / stormer78 |
| request | cred-spec #22 (talltree 08-29: 'the ZK proof simply needs to prove the same person controls the DIDs') · cred-spec §Choosing a scope |

**Composes:** [003](#construction-003-%C2%B7-transcript-binding) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [007](#construction-007-%C2%B7-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A verifier learns that the k credentials in front of it — memberships, relationships, personas — name identifiers all controlled by the presenter, and that none is revoked, while learning no identifier the presenter did not choose to show and receiving no handle that recognises the presenter elsewhere.

**Need.** under three correlation scopes, a holder who used `pairwise` identifiers (distinct by definition) or distinct wider-scoped ones, and now wishes to be recognised as the same party across several communities or relationships, needs one proof — and no other case needs any

#### Witness

*Never leaves the holder.*

- the holder secret s and each identifier's derivation material
- the k credentials (VMCs, VRCs, VPCs) — the holder declares per presentation which of their identifiers are shown and which stay hidden behind commitments
- non-revocation witnesses for each credential handle

#### Public inputs

- the identifiers the holder chooses to disclose (possibly none — the proof can be over commitments alone)
- rl_root and epoch
- transcriptDigest — one transcript for the whole show

#### Relation

1. for each pair (identifier_1, identifier_i), i = 2..k: both open to the same s — k−1 common-control clauses sharing one witness — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-%C2%B7-common-control-across-identifiers))
2. no credential handle is in the set under rl_root at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
3. the whole show is bound to transcriptDigest — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the outcome (one controller / not shown)
- the holder-declared set of credentials shown to share a controller — a per-presentation choice, the disclosure this record exists to make
- the identifiers the holder chose to disclose, and no others
- rl_root, epoch, transcriptDigest

#### Does not establish

- that the presenter is one natural person (k credentials, one secret: an agent holding a person's secret satisfies every clause — record 002 under its own declaration establishes uniqueness)
- anything about credentials not in the show: intentional correlation is declared per presentation and does not widen any identifier's declared scope
- that the communities involved consented to be named together — the disclosure is the holder's
- what any of the credentials asserts beyond existence and non-revocation (a VPC's persona content, a VRC's statement) unless disclosed

#### Adversary, per claim

- **verifier · verifiers-colluding** — no identifier beyond the disclosed set, and no cross-presentation handle: two verifiers shown different subsets cannot join them through this proof
- **issuer-verifier-colluding · registry-operator** — the issuer of any one credential in the show learns nothing about the others from the proof; the revocation-state fetch must not be a per-holder query (record 006 C3)

#### Horizon

- earliest of: any shown credential's validity · epoch rollover · the shortest identifier key-validity among the k (record 007)

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `co-control-unproven`, `handle-revoked`, `show-not-declared (lint: a credential in the witness set has no disclosure declaration)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| no proof — where the holder deliberately reused one `directed` or `public` identifier across the credentials, the correlation is on their face (WD02 §Choosing a scope) | zero | unmeasured | cred-spec §Choosing a scope |
| Groth16 composition: (k−1) × record-007 openings + k non-revocation legs + 1 transcript constraint | unmeasured; conjecture linear in k with the 007 and 006 per-leg costs | unmeasured | board/cards/007.json, 006.json |

#### Issuance requirements

- as record 007: every identifier that may later be co-proven is, or carries, a ZK-openable commitment to s (X3 applied to identifiers)

#### Provenance

- cred-spec #22 (talltree 2026-08-29T22:40Z: the three-scope ZK observation)
- cred-spec §Choosing a scope · Privacy Consideration 2 (intentional correlation via personas)
- cred-spec §VPC (Verifiable Persona Credential) — the credential-layer instrument for the same intent

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-29 | `requested` | talltree (cred-spec #22) | cred-spec #22 comment 2026-08-29T22:40Z: 'it reduces the set of ZK proofs needed for intentional correlation … the ZK proof simply needs to prove the same person controls the DIDs' |
| 2026-09-05 | `specified` | mitchuski | composed from 007 + 006 + 003 under one transcript; disclosure set and negative space written fresh (composition rule) |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Composed/R012.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: clause 1: k − 1 common-control clauses against identifier_1
- scope: clause 1; non-revocation (2) and the transcript (3) are records 006 and 003

| theorem | proves |
|---|---|
| `Formal.R012.star_to_clique` | with H-com and k ≥ 2, the k − 1 clauses give every pair of the k identifiers one controller |
| `Formal.R012.star_without_binding_not_clique` | without a binding derivation they do not — the record's 'sharing one witness' carries the statement then |

| hypothesis | carries |
|---|---|
| H-com (record 007) | the identifier derivation binds (secret, salt) |
| k ≥ 2 | at k = 1 there is no clause and nothing establishes a secret behind identifier_1 |


### Construction 013 · Mutual edge admissibility — each half admissible under the other community's policy, neither policy nor member revealed

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P3 |
| constructor | mitchuski |
| requested by | stormer78 (OpenVTC implementation, cred-spec #25) · geoffturk (WD02 restatement) — requested by the ZKP TF co-chair on the record |
| request | cred-spec #25 (stormer78 2026-08-24: forming a cross-community edge needs both communities' policies to admit it, and neither side can learn the other's policy before publishing a half; first signal of inadmissibility is a rejection after one half is already out) · cred-spec #25 (mitchuski 2026-08-25: step one is plain published admissibility predicates, no proof machinery; the stronger form — 'my half would be admissible under the counterparty's policy' proven without revealing the policy or the member — is future work for the ZKP task force) · cred-spec #25 (geoffturk 2026-09-07: the two predicates in WD02 vocabulary — which correlation scopes a community accepts for the subject of a VRC its members publish, and whether it admits a non-member subject) |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [022](#construction-022-%C2%B7-blinded-digest-references-%E2%80%94-the-digest-valued-members-of-the-credential-specification%2C-unenumerable-at-rest-and-openable-in-proof) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

Each party to a proposed cross-community edge learns that the counterparty's committed half would be admitted under this party's community's published admissibility policy — the half's form (its subject's correlation scope and whether it claims membership) is one the policy accepts, and where the form claims membership, the subject is a member of the counterparty's community — without learning the counterparty's identifier, membership or anything of its half beyond the yes; neither half is revealed until both answers are yes.

**Need.** turn a failed cross-community publish into a pre-flight check that reveals nothing about either membership: each party proves its half would be admissible under the other community's published policy commitment before either half is published, and the two proofs are exchanged commit-before-reveal so that whoever goes first has not already disclosed

#### Witness

*Never leaves the holder.*

- the prover's proposed half — the relationship credential half it would publish (its subject identifier, declared correlation scope and membership status) — as canonical bytes, and the salt u under which it was committed (record 022)
- the Merkle path of the half's form (correlation scope, membership claimed) to the counterparty community's policy root
- where the form claims membership: the subject's community-issued grant and its Merkle path to the prover's own community root (record 001)

#### Public inputs

- the counterparty community's published admissibility-policy root — a commitment to the forms it accepts, the two WD02 predicates (which correlation scopes it accepts for the subject of a VRC its members publish; whether it admits a non-member subject) as a committed set
- the prover's community membership root at a stated epoch, where the form claims membership
- C — the commitment to the prover's half, exchanged before either proof
- transcriptDigest — over both commitments and the exchange's challenge

#### Relation

1. C opens to the canonical bytes of the proposed half under the salt u — the half is fixed before either party answers, and the half later revealed is the one the proof was about — [[ref: commitment-open]] ([[ref: construction record]] 022, [Blinded digest references — the digest-valued members of the credential specification, unenumerable at rest and openable in proof](#construction-022-%C2%B7-blinded-digest-references-%E2%80%94-the-digest-valued-members-of-the-credential-specification%2C-unenumerable-at-rest-and-openable-in-proof))
2. the half's form (the subject's correlation scope, membership claimed or not) is a leaf of the counterparty community's policy root — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
3. where the form claims membership, the subject's grant is a leaf of the prover's own community root at the stated epoch — a claimed membership is real, never merely declared — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
4. the proof is bound to transcriptDigest over both commitments and the exchange's challenge, so an admissibility answer cannot be replayed against a different half or a different exchange — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- each direction's outcome (the counterparty's committed half has a form this community's policy accepts / not shown)
- the two commitments C and transcriptDigest
- the policy roots and the membership root, already published
- the halves themselves, only after both answers are yes — a rule of the exchange, not of either proof

#### Does not establish

- that the edge will be accepted once published — admissibility under a policy commitment is not acceptance by a community's verifier
- privacy of either policy — proving a form is in the counterparty's accepted set needs that set's path, so the policies are published (the request's step one); hiding a policy from prospective counterparties is not addressed
- fairness — after both answers are yes, nothing compels a party to reveal its half; a party can learn the yes and walk away (the formal model's no_fairness)
- that the half's declared correlation scope is honest beyond the form — a non-member form proves nothing about the subject, and the scope is the prover's declaration inside its own committed half
- that the policy root is current beyond the stated epoch or version
- that either party is a member of anything beyond what the policy's form asked

#### Adversary, per claim

- **verifier** — the counterparty learns that this party's half has a form its policy accepts, not the half, its subject identifier or the membership behind it — holds while C is hiding (the salt u) and while the policy accepts more than one form; a policy that accepts exactly one form turns yes into the form
- **verifier** — a party that answers first cannot be met by a counterparty that re-commits to a different half after seeing the answer — commitments are frozen once an answer exists (the formal model's commit_frozen)

#### Horizon

- earliest of: each community's policy root version, the prover's membership root epoch, and the exchange challenge
- the exchange itself: an unanswered exchange expires with its challenge; a new exchange needs new commitments

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify`

Rejection codes: `half-inadmissible (unsat: the half's form is not a leaf of the counterparty's policy root)`, `membership-unproven (unsat: the form claims membership and no grant leaf exists under the prover's root)`, `commitment-mismatch (unsat: C does not open to the half the proof reads)`, `transcript-digest-mismatch (verify: the proof was made for another exchange)`, `answer-before-commitments (exchange: refused until both commitments are in)`, `reveal-before-both-yes (exchange: refused)`, `recommit-after-answer (exchange: refused)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| step one, no proof — the two predicates published as discoverable fields on the community profile (the proposed answer on #25; reveals nothing about membership) | zero; a pre-flight read | unmeasured | cred-spec #25 proposed answer · geoffturk 2026-09-07 WD02 restatement |
| Groth16 / BN254 / Poseidon composition — the lab's membership circuit (record 001) against the policy root and, where claimed, the membership root, with a Poseidon commitment opening (record 022) and transcript binding (record 003); the exchange runs outside the circuit | unmeasured | unmeasured | this record's clauses over the lab's components; record 001's runtime |

#### Issuance requirements

- a community publishes its admissibility predicates as a Merkle root over the forms it accepts (correlation scope × membership status), per policy version — the registry-ZK interaction the credential specification leaves to this task force
- the tree hashes forms and internal nodes under separate domains, or every path has the tree's fixed depth (record 001's requirement)
- the proposed half's canonical bytes are committed with a fresh salt before the exchange (record 022's route 1)
- the exchange runs in order — both commitments, then the proofs, then revelation only after both answers are yes; no commitment changes once an answer exists

#### Provenance

- cred-spec #25 — relationship policy discovery (stormer78 2026-08-24; mitchuski 2026-08-25; geoffturk 2026-09-07)
- cred-spec §Scope the holder cannot declare alone (WD02) — the pattern of a community stating a policy the holder cannot see, applied to a different fact
- docs.fpp.storm.ws DTG conformance review, item X4 'Across communities'
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-11 | `requested` | mitchuski (on cred-spec #25, 2026-08-25) — requested by stormer78's issue and geoffturk's 2026-09-07 restatement | cred-spec #25: 'There is a stronger form the ZKP task force can carry as future work: proving my half would be admissible under the counterparty's policy without revealing the policy or the member' — placed on the request register 2026-09-11 (open item D23); every field above is a placeholder until specified |
| 2026-09-23 | `specified` | mitchuski | statement, witness, public inputs, four clauses composed from 001 · 003 · 022, disclosure set, six doesNotEstablish lines (policy privacy and fairness named), two adversary claims, horizon, seven rejection codes (three for the exchange), two options, four issuance lines; formal model of one direction and of the exchange ordering |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Composed/R013.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.R013.Accepts for one direction; the exchange as a state machine (Formal.R013.step, Safe)
- scope: clauses 1–3 as a model and the exchange ordering; clause 4 (transcript) is record 003's; the commitment's hiding and the policy's privacy are outside the model

| theorem | proves |
|---|---|
| `Formal.R013.soundness` | clauses 1–3: the committed half is the one whose form was proven, the form is a leaf of the counterparty's policy tree, and a claimed membership is a leaf of the prover's tree (records 022 and 001 twice) |
| `Formal.R013.exchange_safe` | over any sequence of events, a half is revealed only after both answers are yes, and no answer exists before both commitments |
| `Formal.R013.commit_frozen` | no commitment changes once an answer exists |
| `Formal.R013.no_fairness` | does-not-establish 3: after both yes answers the exchange can stop with nothing revealed |

| hypothesis | carries |
|---|---|
| H-digest (record 022) | clause 1: the commitment is collision-free over (bytes, salt) |
| H-leaf, H-node, H-domain (record 001) | clauses 2 and 3: both trees' hashes are collision-free, leaves and nodes under separate domains |


### Construction 020 · Delegation chain (VDC) — agent acts for a member

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P2 |
| constructor | construction: sankarshanmukhopadhyay · DenisPopov15 · mitchuski (per ScottJeezey, cred-tf #40) · record: stormer78 (§VDC, merged 2026-09-06) |
| requested by | stormer78 / sankarshanmukhopadhyay |
| request | cred-spec §VDC (Verifiable Delegation Credential), open question 6 of its merge review; ADR-001 §05 'deserves its own record once this one is proven' · cred-tf #40 (stormer78 08-22 design note; ScottJeezey 08-24: "on our list alongside Q2") · cred-spec #31 pre-merge list for #19 |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [009](#construction-009-%C2%B7-hidden-value-equality-across-credentials) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A verifier learns that the presenting agent holds a delegation chain rooted at a principal who is a member of a recognised community, with scope narrowing at every hop, no hop expired or revoked, and every hop accepted by its delegate — without learning the principal. (Profile case; the core single-hop VDC is verified by five local checks and no proof.)

**Need.** prove an agent may act for a principal within scope S until T, chain attenuation-only, without revealing the principal — in the chained PROFILE only: the WD02 core VDC is single-hop, principal-issued, delegate-countersigned and needs no chain proof (cred-tf #40 §1)

#### Witness

*Never leaves the holder.*

- the VDC chain (grant credentials), each hop's scope, validUntil, maxDepth
- the principal's membership leaf
- the agent's key secret
- each hop’s acceptance countersignature (`delegation.accepts` = SAID of the grant) — the acceptance is constitutive, not optional evidence (cred-tf #40 §3, KERI two-seal shape)

#### Public inputs

- root_C, rl_root, epoch
- the invoked scope term (disclosed)
- transcriptDigest

#### Relation

1. act ∈ scope_n ⊆ … ⊆ scope_root (set inclusion over exact matches), validUntil monotone along the chain, and depth bounded by every ancestor: a hop below a parent bearing `maxDepth` n bears at most n − 1, no hop lies more than n steps below an ancestor bearing n, and no hop exists below a parent that omits `maxDepth` or sets it to 0; the chain terminates in a root delegation issued by the principal (cred-spec §Delegation Chains, WD02, checks 2–5) — [[ref: chain-resolve]]
2. each hop's `issuer` equals its parent's `credentialSubject.id` — hidden-value equality across credentials signed by different parties, with no holder secret in the relation (cred-spec §Delegation Edges; #9 2026-09-10) — [[ref: hidden-equality]] ([[ref: construction record]] 009, [Hidden-value equality across credentials](#construction-009-%C2%B7-hidden-value-equality-across-credentials))
3. each hop’s delegate countersigned the grant: `accepts` matches the grant digest (digestMultibase, WD02 D-A) and verifies under the delegate’s key — [[ref: signature-verify]]
4. principal is a leaf of root_C — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
5. the agent's presentation key derives from the leaf-hop delegate secret — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret)))
6. no hop revoked at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
7. bound to one transcript — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the invoked scope term
- root_C, rl_root, epoch
- transcriptDigest

#### Does not establish

- that the principal authorised this specific act (grant ≠ invocation — the invocation is a trust-task artifact)
- the principal's identity
- that the agent is not also acting for others
- that the principal has not declined renewal — in the core, revocation is non-renewal within one validUntil; the profile’s credentialStatus re-adds a live lookup and this record’s non-revocation leg is what lets the presentation carry it instead
- what the delegate actually did in the principal’s name — the invocation record lives on the framework side (the artifact gap, cred-tf #40 Q8)
- chain-length hiding without a validated fixed-shape or padded profile
- that the principal is the party the verifier intends to deal with — §Delegation Chains check 5 is the verifier's own check, outside the proof; the proof shows the root's issuer is a leaf of root_C, not that it is the intended party
- the delegate's demonstration of key control at the moment of the request (§Invocation Binding) beyond what clause 4 binds into this transcript — how the demonstration is requested and carried is the trust task's
- chain length: without a validated fixed-shape or padded profile the number of hops is disclosed by the proof's shape (cred-spec Privacy Considerations item 13)

#### Adversary, per claim

- **verifier · verifiers-colluding** — principal hidden under the selected proof assumptions and declared disclosure, against the verifier and colluding verifiers; hiding chain length additionally requires validated padding/fixed shape and metadata analysis, which are not established here

#### Horizon

- the shortest validUntil in the chain
- status freshness

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify`

Rejection codes: `scope-escalation (unsat)`, `depth-exceeded`, `hop-revoked`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| bounded monolithic proof of a fixed maximum chain depth; padding semantics and authenticated hop checks to be defined | unmeasured | unmeasured | editorial alternative for review, 2026-09-08 |
| recursive/folding proof per hop | unmeasured | unmeasured | PATH-MAP P4 (PLONKish/folding counter-proposal welcome) |

#### Issuance requirements

- VDC as an edge credential type (cred-spec §VDC, merged WD02 2026-09-06) with ZK-friendly signatures — X3 applies
- the delegator issues from an identifier declared `directed` and scoped to the context in which the appointment is exercised (cred-spec Privacy Considerations item 10); the delegate accepts each principal's appointment under a distinct identifier
- grant and acceptance digests are digestMultibase (§Digest Encoding); the acceptance is REQUIRED and carries no scope of its own — the verifier reads scope from the grant (§Delegation Edges)
- `parent` and `maxDepth` exist only where re-delegation is explicitly authorised (absent or 0 = single hop, the default); `credentialStatus` is CONDITIONAL on every VDC, chained or not — REQUIRED where validity exceeds the governing freshness window, otherwise short validity and re-issuance (§VDC schema); this record's non-revocation leg is what lets a chained presentation carry status without a live lookup
- the chain is presented whole — a verifier MUST reject a chain it cannot complete from the presentation alone — which is exactly the disclosure this record removes (§Delegation Chains; Privacy Considerations item 13)

#### Provenance

- cred-spec §VDC (Verifiable Delegation Credential) — merged 2026-09-06 (over WD02): §Delegation Edges, §Delegation Chains (five chain checks; 'chain validity is a candidate for zero-knowledge presentation'), §Invocation Binding
- cred-spec §Zero-Knowledge and Selective Disclosure editor's note (2026-09-10): 'proving a VDC chain valid without disclosing it' named as a predicate waiting on the ZKP task force
- cred-spec #9 (geoffturk 2026-09-10): the chain predicates are hidden-value equality, not common control — record 009
- liveness reqs v0.4 delegation evidence
- ADR-001 §05
- ePrint 2026/333 App. B.1
- cred-tf #40 (delegation as a design-time case; the chain-resolution boundary) · cred-spec #31 (#19 pre-merge checklist)

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-09-05 | `specified` | mitchuski | ScottJeezey accepted delegation-chain validity as a ZKP TF target (cred-tf #40, 2026-08-24); predicate shape + acceptance clause + core/profile split from stormer78’s note; issuance lines from cred-spec #31 |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-11 | mitchuski | Door D17 — re-read against the merged VDC text (cred-spec main, 2026-09-06/10): depth restated as the per-ancestor bound; the issuer-equals-parent-subject clause added and bound to record 009 (hidden-value equality); `credentialStatus` corrected — conditional on every VDC, not only chained ones; issuance and provenance now cite the merged sections; three negative-space lines added (intended-party check, invocation demonstration, chain length). State unchanged. |
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Chains/Records.lean (with Presentation.lean and Gadgets/ChainResolve.lean) in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Chains.Statement with the root's own depth — the act conferred and unexpired at every hop, depth within the root, every issuer its parent's subject, the root's issuer a member, no hop revoked
- scope: clauses 1–4 and 6; the agent's key (5) and the transcript (7) are records 004 and 003; chain-length hiding is a profile property

| theorem | proves |
|---|---|
| `Formal.Chains.r020_soundness` | the local per-hop checks establish the global statement |
| `Formal.Chains.escalation_unsat` | the rejection code scope-escalation: a hop that does not confer the act makes the chain unacceptable |
| `Formal.Chains.linked_of_constraint` | clause 2: the in-circuit constraint issuer − subject = 0 is equality (record 009) |
| `Formal.Gadgets.leaf_act_all` | clause 1: checking the act at the leaf gives it at every hop |
| `Formal.Gadgets.depth_within_every_ancestor` | clause 1: no hop lies more than n steps below any ancestor bearing n |
| `Formal.Gadgets.no_decrement_unbounded` | clause 1: without 'a child bears at most n − 1' a chain runs deeper than its root allows |

| hypothesis | carries |
|---|---|
| H-sig | clause 3: each hop's countersignature verifies |
| H-leaf, H-node, H-domain (record 001) | the membership tree's hashes are collision-free and leaves and nodes are hashed under separate domains |
| H-sorted (record 006) | the revocation root is committed over strictly sorted handles |


### Construction 021 · Authority chain (VAC) — an agent or device acts as itself under attenuated authority

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski |
| requested by | stormer78 / geoffturk (spec side) — specified by the ZKP TF co-chair |
| request | cred-spec §Zero-Knowledge and Selective Disclosure editor's note (merged 2026-09-10): 'Holder holds a VAC conferring action X at scope S, and its chain is valid and unrevoked: each link is issued by its parent's subject, narrows its parent, no link is revoked, depth is within every limit its links set, and the root is issued by the party governing S — without disclosing the chain' · cred-spec §VAC (Verifiable Authority Credential) (merged 2026-09-10): §Attenuation, §Invocation, §Withdrawal, §Authority and membership are separate credentials · cred-spec #9 (2026-09-10): the VAC chain predicate is hidden-value equality, not common control |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [009](#construction-009-%C2%B7-hidden-value-equality-across-credentials) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A verifier learns that the presenting party holds authority to perform action X at scope S, conferred through a chain of attenuations that starts with the party governing S, narrows at every link, respects every `maxAttenuation` and the depth ceiling, has no revoked link, and whose leaf subject is the presenter — without learning any link's identifiers or the chain's ancestry beyond what the presenter discloses.

**Need.** a VAC chain is presented whole on every use, so every verifier sees the identifier of the party that equipped the agent (cred-spec Privacy Considerations item 13); the credential specification names the zero-knowledge form as waiting on this task force and states what holds until it lands

#### Witness

*Never leaves the holder.*

- the VAC chain: every VAC from the presented leaf up to the one issued by the governing party (`authority.parent` digests, `scope`, `actions`, `validUntil`, `maxAttenuation` per link)
- the governing party's accreditation leaf and path for scope S, where S's governing party is itself proven from a registry root rather than disclosed
- the leaf subject's key secret (the presenter acts as itself and must demonstrate key control at invocation — §Invocation)
- non-revocation witnesses for every link that carries `credentialStatus`
- the presenter's VMC leaf and path, where the governing party requires the leaf subject to independently qualify (§Attenuation, 'Who may hold derived authority')

#### Public inputs

- root_G — the root under which the party governing S is accredited (or the governing party's identifier, where the profile discloses it)
- the invoked action X and scope term S (disclosed — the act is attributed to the presenter)
- rl_root and epoch — revocation state
- transcriptDigest — one transcript for the whole show, including the verifier's challenge

#### Relation

1. X ∈ actions_leaf ⊆ … ⊆ actions_root, scope never widened, validUntil monotone, depth ≤ 8 and within every `maxAttenuation` any link sets (a link below a parent bearing n bears at most n − 1; none exists below a parent bearing 0); the root is a VAC issued directly by the governing party (`authority.parent` absent) — cred-spec §Attenuation — [[ref: chain-resolve]]
2. each link's `issuer` equals its parent's `credentialSubject.id`, and each link's `authority.parent` equals the digest of its parent — hidden-value equality across differently-signed credentials — [[ref: hidden-equality]] ([[ref: construction record]] 009, [Hidden-value equality across credentials](#construction-009-%C2%B7-hidden-value-equality-across-credentials))
3. every link verifies as signed by its issuer over the hidden content the chain clauses read — [[ref: signature-verify]]
4. the governing party of S is a leaf of root_G — proven from the root, not disclosed — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
5. the presenter's key derives from the leaf subject's secret — the VAC is not a bearer credential (§Invocation) — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret)))
6. no link carrying `credentialStatus` is in the set under rl_root at epoch — revocation of an ancestor cascades, so the check runs on every link that carries status (§Withdrawal) — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
7. where the governing party requires it, the leaf subject is also a leaf of the scope's membership root — authority and membership stay separate credentials, and when both are proven with the subject withheld the presentation includes a shared-subject proof: record 009 where one identifier is used in both, record 007 where the two identifiers differ (§Authority and membership are separate credentials) — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
8. the whole show is bound to transcriptDigest — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the outcome (holds authority for X at S / does not)
- X and S — the act is performed as the presenter and attributed to the presenter
- root_G, rl_root, epoch
- transcriptDigest
- the presenter's own identifier, where the profile discloses it (the presenter acts as itself); nothing about any ancestor

#### Does not establish

- that the presenter is someone the scope will deal with — a valid chain establishes narrowing by parties entitled to narrow, not that the leaf subject independently qualifies; that is the governing party's policy call (§Attenuation) and clause 7 is present only where the policy asks for it
- delegation: the presenter acts as itself, and nothing here appoints it to act in anyone's name (§Authority is not delegation — that is record 020)
- that the governing party's own permission to govern S is current beyond 'accredited under root_G at the stated state'
- chain length: the number of links is disclosed by the proof's shape unless a fixed-shape or padded profile is validated — hiding it is a profile property this record does not claim
- that the action was performed, or performed within scope — the invocation and its receipt are trust-task artifacts (the artifact gap)
- unconditional hiding from the governing party: where the root carries `credentialStatus`, the status fetch tells the root's status host that some verifier checked the chain, and when (cred-spec Privacy Considerations item 14) — a profile that fetches rl_root without a per-chain query is the mitigation, not this proof
- distinct controllers along the chain: a party attenuating to itself under a second identifier satisfies every clause

#### Adversary, per claim

- **verifier · verifiers-colluding** — no ancestor identifier is disclosed — the verifier learns the leaf's authority, not who equipped the presenter or through whom; against colluding verifiers the chain contributes no cross-presentation handle beyond what the presenter discloses of itself
- **registry-operator** — the status check on links that carry `credentialStatus` does not identify the presenter or the chain when rl_root is fetched without a per-chain query; the root-status timing leak of item 14 is stated, not hidden
- **issuer-verifier-colluding** — an issuer of one link learns from the proof nothing about the links below it — attenuations it never saw stay unseen, as the credential specification intends ('a governing party withdraws derivations it never saw')

#### Horizon

- the shortest `validUntil` in the chain (attenuation never extends validity; REQUIRED on every VAC)
- status freshness for links that carry `credentialStatus` (the governing party's freshness window)
- root_G cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify`

Rejection codes: `action-not-conferred (unsat: X absent from the leaf's actions)`, `attenuation-widens (unsat: a link confers an action, scope or validity its parent did not)`, `attenuation-limit-exceeded (unsat: a link lies below a `maxAttenuation` bound or below a link bearing 0)`, `depth-ceiling-exceeded (unsat: more than 8 links)`, `link-issuer-mismatch (unsat: a link's issuer is not its parent's subject — record 009)`, `link-revoked (unsat at epoch)`, `root-not-governing (unsat: the root's issuer is not a leaf of root_G)`, `leaf-key-mismatch (verify-fail: the presenter's key does not derive from the leaf subject's secret)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| bounded monolithic proof of a fixed maximum chain depth (≤ 8, the credential specification's ceiling), padded to a fixed shape so chain length is not disclosed; hop checks over Poseidon-committed VAC content | unmeasured — conjecture: 8 × (signature-verify + two openings) dominates; the chain arithmetic is cheap | unmeasured | record 020's editorial alternative (2026-09-08), applied with the VAC's fixed ceiling |
| recursive/folding proof per link — one step per attenuation, the leaf proof carrying the accumulated statement | unmeasured | unmeasured | PATH-MAP P4 (PLONKish/folding counter-proposal welcome) |
| no proof — the chain is presented whole and every check is performed on disclosed credentials (the credential specification's stated fallback, at the cost of Privacy Considerations item 13) | zero constraints; the cost is disclosing the ancestry | unmeasured | cred-spec §Zero-Knowledge and Selective Disclosure editor's note, 'What holds until this work lands' |

#### Issuance requirements

- VAC content that the chain clauses read — `issuer`, `credentialSubject.id`, `authority.scope`, `authority.actions`, `authority.parent`, `authority.maxAttenuation`, `validUntil` — must be ZK-openable inside the authenticated credential: X3 applied to the VAC (cred-spec #17)
- `authority.parent` is a digestMultibase digest of the parent (§Digest Encoding); the unsalted-digest concern of cred-spec #38 applies to it — record 022 (blinded digest references) is the construction for that member and the four others; record 008's blinding question is the trust-task binder's
- the governing party publishes rl_root per epoch, fetchable without a per-chain query; where the root carries `credentialStatus` the governing party states the timing correlation it accepts (cred-spec Privacy Considerations item 14)
- a governing party that requires derived subjects to independently qualify says so in its governance framework, so a profile knows whether clause 7 is in the statement (§Attenuation)

#### Provenance

- cred-spec §VAC (Verifiable Authority Credential), merged 2026-09-10: §Attenuation (by default; `maxAttenuation`; depth ceiling 8; 'Who may hold derived authority'), §Invocation (not a bearer credential), §Withdrawal (cascade), §Authority is not delegation, §Relationship to the VDC, §Authority and membership are separate credentials
- cred-spec §Zero-Knowledge and Selective Disclosure editor's note (2026-09-10): the VAC chain predicate waiting on the ZKP task force; 'implementations SHOULD NOT defer shipping a rule of this specification on the grounds that its zero-knowledge form is unspecified'
- cred-spec #9 (geoffturk 2026-09-10): chain predicates are hidden-value equality — record 009
- cred-spec Privacy Considerations items 13 (chain disclosure) and 14 (status on a root)
- record 020 (delegation chain) — the sibling record; the two differ in default (attenuation by default vs re-delegation opt-in), in attribution (as itself vs in another's name) and in cascade
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-10 | `requested` | stormer78 / geoffturk (cred-spec PR #29 → #42) | cred-spec PR #42 merged 2026-09-10: the VAC chain predicate listed as waiting on the ZKP task force; cred-spec #9 2026-09-10 names it separately from common control |
| 2026-09-11 | `specified` | mitchuski | specified from the merged §VAC rules (attenuation, invocation, withdrawal, shared subject) as a sibling of record 020; composition and negative space written fresh; every cost line conjecture — DRAFT for review |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Chains/Records.lean (with Presentation.lean and Gadgets/ChainResolve.lean) in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Chains.Statement with the depth ceiling 8, plus clause 7's leaf membership where the governing party requires it
- scope: clauses 1–4, 6 and 7; the presenter's key (5) and the transcript (8) are records 004 and 003; distinct controllers along the chain (does-not-establish 7) are outside any clause

| theorem | proves |
|---|---|
| `Formal.Chains.r021_soundness` | the local per-link checks establish the global statement, and the leaf subject's membership where required |
| `Formal.Chains.r021_ceiling_unsat` | the rejection code depth-ceiling-exceeded: more than 8 links is never accepted, whatever maxAttenuation says |
| `Formal.Chains.escalation_unsat` | the rejection code action-not-conferred |
| `Formal.Chains.linked_of_constraint` | clause 2: the in-circuit constraint issuer − subject = 0 is equality (record 009) |
| `Formal.Gadgets.scope_within_every_ancestor` | clause 1: no link widens its parent's scope, across the whole chain |

| hypothesis | carries |
|---|---|
| H-sig | clause 3: every link's signature verifies over the content the chain clauses read |
| H-leaf, H-node, H-domain (record 001) | the membership tree's hashes are collision-free and leaves and nodes are hashed under separate domains |
| H-sorted (record 006) | the revocation root is committed over strictly sorted handles |


### Construction 022 · Blinded digest references — the digest-valued members of the credential specification, unenumerable at rest and openable in proof

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `specified` |
| priority | P2 |
| constructor | mitchuski |
| requested by | geoffturk / stormer78 (cred-spec #38) · ScottJeezey (cred-tf #39) — specified by the ZKP TF co-chair on the maintainer's request of 2026-09-16 |
| request | cred-spec #38 (geoffturk 2026-09-05: every digest-valued binder is an unsalted JCS-SHA-256 digest over often low-entropy content — enumerable where the referenced credential is not disclosed; blinding deferred from WD02) · cred-spec #38 (stormer78 2026-09-07: five members, of two kinds — the acknowledgement, the acceptance and the statement adopt the exact content they name; the VDC's and VAC's `parent` are chain references re-checked against a presented parent) · cred-tf #40 E3 (stormer78: forty published scope strings, a committed subset matched by enumeration in milliseconds) · cred-tf #39 (ScottJeezey 2026-08-25: salted commitments now, PRF-derived pseudonyms later) · zkp-spec PR #8 review (geoffturk 2026-09-16, request 3: 'give #38 a record' — widen 008 or open a sibling; the disposition of which members carry a salt and where it lives stays on #38) |

**Kind:** [[ref: primitive construction]] — binds the [[ref: commitment-open]] gadget and nothing else.

#### Statement

A verifier learns that a digest-valued reference carried by a presented credential names exactly the credential the enclosing record's clauses read — the community-issued grant a member-issued VMC acknowledges, the appointment a VDC accepts, the credential a VSC's statement is about, the parent a VDC or a VAC derives from — while the reference value is not enumerable by a party not shown the referenced credential and, where the enclosing record hides the reference, is not shown at all.

**Need.** a verifier shown a digest-valued reference but not the credential it names must not be able to recover that credential by trying the plausible values; and a proof that reads the referenced credential (an acknowledged grant, an accepted appointment, an attested object, a parent in a chain) must be able to open the reference to it without the reference becoming a durable correlator of the presenting credential

#### Witness

*Never leaves the holder.*

- the referenced credential's canonical bytes — its JSON representation without the top-level `proof`, canonicalized per JCS (RFC 8785), as §Digest Encoding prescribes — or, where the enclosing record needs only the digest, the digest itself
- the blinding value u under which the digest was taken (a 32-byte random salt), held by whoever holds the referenced credential
- the referencing credential that carries the reference (its bytes stay with the holder; only what the enclosing record discloses is shown)

#### Public inputs

- the reference value as the referencing credential carries it (route 1: a salted digest, visible; route 2: nothing — the reference is opened inside the proof and the enclosing record's public inputs stand in for it)
- transcriptDigest — the presentation transcript this proof is bound to
- the hash algorithm the reference declares in its Multihash header (`sha2-256` unless a governing VTC or VTN permits another), so verifier and circuit agree on the function being opened

#### Relation

1. the reference opens to (canonical bytes of the referenced credential, u) under the declared digest function: reference = Multibase(Multihash(H(bytes ∥ u))) — a different credential or a different salt is unsatisfiable; and when the enclosing record reads the referenced credential (the grant's fields in record 010, the parent's fields in records 020 and 021, the object in a VSC-based record) the bytes opened here are the bytes those clauses read — one credential, not one for the digest and another for the predicate — [[ref: commitment-open]]

#### Disclosure set

- the outcome (the reference names the credential the clauses read / not shown)
- transcriptDigest
- the reference value only as far as the enclosing record already discloses it — route 1 shows the salted digest, which is stable for the referencing credential; route 2 adds nothing to the disclosure set

#### Does not establish

- that the referenced credential is currently valid, unrevoked or accepted (record 006; the enclosing record's own clauses)
- that the party issuing the referencing credential was entitled to reference that credential — an acknowledgement by a non-member, an acceptance by the wrong delegate, a witness with no standing: governance and the enclosing record decide that, not the opening
- unlinkability of presentations that show the same salted digest (route 1): hiding the plaintext behind a salt stops enumeration and nothing else; a stable visible reference still links every presentation of the referencing credential, exactly as record 008 says of a visible commitment C
- which digest-valued members carry a salt and where the salt lives — that disposition is the credential specification's (cred-spec #38); this record states what the proof needs of whichever placement is chosen
- that a chain reference (a VDC's or VAC's `parent`) needs blinding for safety: the credential specification re-checks every link against the parent actually presented, so a chain reference is unforgeable within a presented chain without a salt; blinding it serves uniformity and enumeration resistance, not chain soundness

#### Adversary, per claim

- **verifier** — a verifier shown the reference but not the referenced credential learns nothing about that credential's content that it could not have guessed without the reference — the salt removes the enumeration oracle
- **verifiers-colluding** — route 2 only: two verifiers comparing what they were shown cannot link two presentations of the referencing credential through the reference, because neither saw a reference value

#### Horizon

- the salt's confidentiality: the claim fails for any party that holds u and the schema — the referencing credential's holder, the referenced credential's holder, and whoever either gave the salt to; the disposition on #38 decides who those are
- the digest function: opening a SHA-256 digest in-circuit is the cost horizon of route 2; a profile that permits another Multihash algorithm changes the gadget, not the record
- the validity horizon of the enclosing record — this record adds none of its own

#### Conformance fixtures

Families: `accepts` · `rejects-unsat`

Rejection codes: `reference-mismatch (unsat: no (bytes, u) opens the presented reference to the credential the clauses read)`, `reference-unsalted (lint: a profile that requires blinding presented a reference whose hashed representation carries no salt)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| salt in the referenced credential's hashed block (an ACDC-style `u` member: 32 random bytes the issuer places in the credential, so every digest of that credential is unpredictable without the credential itself). Route 1: no change to the digest computation of §Digest Encoding beyond the bytes it hashes; a verifier shown the referenced credential recomputes as today; enumeration is defeated for every reference to that credential at once, and the salt travels with the credential that owns it | zero outside a proof; inside a proof, a SHA-256 preimage over the canonical bytes — on the order of 25–30k R1CS constraints per 64-byte block in circom's sha256 (conjecture for this record, not measured); a 1.5 KB credential is roughly 24 blocks | unmeasured | cred-spec #38 (the ACDC `u` salty-nonce placement named in the issue); KERI/ACDC specification, blinding of SAIDs by a `u` field; circomlib sha256 constraint counts as commonly reported |
| salt held beside the reference, outside the credential at rest (record 008's route 1 shape): the referencing credential carries H(bytes ∥ u) and the two parties to the reference hold u; the referenced credential is unchanged. Blinds one reference rather than every reference to a credential, and requires a distribution and retention rule for u | as above in-circuit; a retention rule outside it | unmeasured | cred-tf #39 (salted commitments available now); record 008 route 1 and its retention caveat |
| a SNARK-native commitment (Poseidon or Pedersen over a field encoding of the digest) as the reference — cheap to open in-circuit, but a second hashed representation beside §Digest Encoding's, which is the encoding migration #38 was scoped to avoid; recorded as the not-preferred option | a few hundred constraints to open; a second encoding for every consumer | unmeasured | cred-spec #38 ('so a blinding scheme can later change what is hashed without a second encoding migration') |

#### Issuance requirements

- the members this record covers are the five §Digest Encoding names — the member-issued VMC's `digestMultibase`, a VSC's `object.digestMultibase`, a VDC's `delegation.parent` and `delegation.accepts`, a VAC's `authority.parent` — and, once cred-spec PR #56 merges, `taskDigestMultibase` as a sixth; record 008 covers the trust-task citation itself
- the hashed representation of a referenced credential must contain a salt the proof can treat as a witness; the proof-side preference is the first option above (a salt member inside the referenced credential), because it blinds every reference at once, moves with the credential, and changes neither the referencing credential nor the encoding — the disposition is #38's
- content-binding references (acknowledgement, acceptance, statement object) must stay bound to the exact content they name: a salted digest preserves that binding, a re-randomizable commitment would not without a further opening
- chain references (a VDC's and a VAC's `parent`) are re-checked by the verifier against the parent presented, so a profile may leave them unsalted where the whole chain is disclosed, and must salt them where a chain is proven without disclosure (records 020 and 021)
- the encoding of the reference is unchanged: Multibase base-58-btc over a Multihash of the salted digest, compared as decoded bytes, as §Digest Encoding requires

#### Provenance

- cred-spec #38 — 'Digest-valued binders are unsalted and enumerable; blinding is deferred from WD02' (geoffturk 2026-09-05; stormer78 2026-09-07 scope note: five members, two kinds; mitchuski 2026-09-08: record 008's reading)
- cred-spec §Digest Encoding — the five members, the JCS-SHA-256-Multihash-Multibase procedure, and the editor's note pointing at this task force for the blinding construction
- cred-spec §Zero-Knowledge and Selective Disclosure editor's note (merged 2026-09-10): 'none of the digest-valued members … is salted … blinding them is cross-cutting work with the same task force'
- cred-tf #39 (ScottJeezey 2026-08-25: salted commitments now; PRF-derived per-context pseudonyms as the fuller construction) · cred-tf #40 E3 (stormer78: enumeration of a committed scope subset)
- cred-spec PR #56 (open, 2026-09-17): `taskDigestMultibase` — a task digest over the initiating document, the same encoding, a sixth member
- zkp-spec PR #8 review (geoffturk 2026-09-16, request 3): give #38 a record, widen 008 or open a sibling; the disposition stays on #38
- records 020 and 021: the chain clauses already read `parent` inside the relation; this record is the opening they compose
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-07 | `requested` | geoffturk / stormer78 (cred-spec #38) · ScottJeezey (cred-tf #39) | cred-spec #38 scope note: five digest-valued members of two kinds; the issue's third tracked item is coordination with the ZKP task force's blinded-binder work |
| 2026-09-21 | `specified` | mitchuski | the credential maintainer's review of PR #8 (2026-09-16) asked for a record a reader coming from #38 will find; this record: statement, witness, public inputs, one clause bound to commitment-open (the opening, with the opened bytes tied to what the enclosing record reads), the two reference kinds kept apart in the issuance lines, three placements with their costs (conjecture), the disposition left to #38 |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | formal model built (Lean, gadget library): the clause as a definition, its soundness, the counter-deployments that show which hypotheses are necessary, and the negative space classified; no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/Gadgets/P022DigestRef.lean in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.Gadgets.RefRel — the reference opens to (the bytes the enclosing clauses read, u)
- scope: the opening and the one-credential clause; validity, entitlement and salt placement (does-not-establish 1, 2, 4) are out of scope; route 1 linkability (line 3) is record 008's visible_value_links

| theorem | proves |
|---|---|
| `reference_opens_once` | reference-mismatch: a reference names one credential |
| `two_credential_gap` | the one-credential clause is necessary: opened and read separately, the proof passes about a credential that is not the referenced one |
| `chain_sound_unsalted` | does-not-establish 5: a chain reference re-checked against the presented parent is sound with no salt — blinding serves enumeration resistance, not chain soundness |

| hypothesis | carries |
|---|---|
| H-digest | the declared digest is collision-free over (bytes, salt) (modelled as injective) |


### Construction 023 · Two-vouch admission proof — an applicant proves k ≥ 2 vouches from distinct current members to the issuing community, without disclosing which members

*This record is at state `specified`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `specified` |
| priority | P1 |
| constructor | mitchuski |
| requested by | ScottJeezey (chair, on the record for Round 1) · Arka Rai Choudhuri / Berkeley (the construction being built) · Glenn Gore (the Linux Plumbers integration) — specified by the ZKP TF co-chair on the chair's request |
| request | zkp-spec PR #11 review (ScottJeezey 2026-09-21, Round 1 position, 'Build'): 'we need a record for the two-vouch admission proof. Per Arka, what Berkeley is building and Glenn is integrating for Linux Plumbers is an admission proof (a non-member proves two vouches from distinct members to the issuer), which is a different statement from record 010. That's the LPC-relevant construction and it has no home yet. Proposing we add it and reference Berkeley's reference code as the constructor.' · general #31 (stormer78 2026-08-26): the Phase 4 join — one member invite, vouched connections with at least two other members, an ID check, presented once to the community, whose rules engine decides |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [005](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer) ∧ [006](#construction-006-%C2%B7-non-revocation-against-a-status-root) ∧ [007](#construction-007-%C2%B7-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

The issuing community learns that the applicant — the subject named in a pending join request — holds relationship credentials issued by at least k distinct current members of this community (k = 2 in the reference policy), each naming the applicant's identifier as subject and each issued by a member whose community-issued grant is a leaf of the community's current membership root, without learning which members issued them and without any voucher taking part in the presentation.

**Need.** the join moment of an open-source community's onboarding: an applicant who is not yet a member presents evidence that at least two distinct current members vouch for them, to the community that will issue the membership, without the community learning which members vouched and without the vouchers being online — a different statement from record 010, where a member shows a verifier a relationship with an offline voucher

#### Witness

*Never leaves the holder.*

- the k relationship credentials (VRCs) the vouchers issued to the applicant, with their signatures
- for each voucher: the voucher's community-issued VMC grant and its Merkle path to root_C (the voucher's membership, proven from the root, not from the voucher)
- for each voucher: the linkage between the identifier the voucher used as VRC issuer and the identifier the voucher's grant names — one `directed` identifier reused, or the voucher's co-control attestation carried by the VRC (record 007 run by the voucher at issuance; the MAY of cred-spec #9)
- the applicant's holder secret and the derivation of the identifier(s) the VRCs name (record 004); where the applicant used a different `pairwise` identifier toward each voucher, the openings that prove them one controller's (record 007)
- non-revocation witnesses for each voucher's membership handle under rl_root at the epoch

#### Public inputs

- root_C and rl_root at a stated epoch — the community's membership and revocation roots, fetched by the applicant without a per-applicant query
- k — the vouch threshold the community's admission policy declares (the reference policy: 2)
- the applicant's admission identifier — the subject the join request names and the VMC would be issued to; disclosed by construction, since the applicant is applying
- context descriptor — the community and the join-request exchange (the Trust Tasks `vtc/join-requests` submission), so the proof cannot be replayed to another community or another request
- transcriptDigest — the presentation transcript, bound to the join request

#### Relation

1. each of the k VRCs verifies as issued by its voucher's VRC-side identifier over a subject equal to the applicant's admission identifier, or to an identifier the applicant proves co-controlled with it (clause 5) — [[ref: signature-verify]]
2. each voucher's community-issued VMC grant is a leaf of root_C — the voucher is a current member, proven from the root while the voucher is offline — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root))
3. for each voucher, the identifier that issued the VRC and the identifier the voucher's grant names are one controller's — reused `directed` identifier, or the voucher's issuance-time attestation opened here — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-%C2%B7-common-control-across-identifiers))
4. the k authenticated member leaves are pairwise distinct — the same member cannot be counted twice under two identifiers or two credentials — [[ref: distinctness]] ([[ref: construction record]] 005, [Distinct member / distinct issuer](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer))
5. the applicant's presentation key derives from the secret the VRCs' subject identifier(s) bind to; where the VRCs name different `pairwise` identifiers of the applicant, those identifiers open to the same secret — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret)))
6. no voucher's membership handle is in the set under rl_root at the epoch — a vouch from a revoked member does not count — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-%C2%B7-non-revocation-against-a-status-root))
7. the whole show is bound to transcriptDigest and to the context descriptor of this community and this join request — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding))

#### Disclosure set

- the outcome (at least k distinct current members vouch for this applicant / not shown)
- k, root_C, rl_root, epoch — the policy threshold and the registry state the proof was made against
- the applicant's admission identifier and the context descriptor of the join request
- transcriptDigest
- under a community policy that requires it, the vouchers' identities as a deliberate disclosure — then this record's privacy claims do not apply to that presentation and the flow is the credentials themselves

#### Does not establish

- that any voucher consented to be counted toward this admission — a relationship credential is evidence of a relationship, and whether it is a vouch is the community's reading under its rules (general #31: 'vouching is social, not technical')
- that the applicant is not already a member — a member can hold k VRCs; if the policy needs 'not a member', the statement gains a non-membership clause against root_C (record 006's gadget applied to the membership set), which this record does not include
- that no voucher is the applicant — the statement has no clause excluding it, and an applicant who is already a member can vouch for itself through its own grant, so one of the k counted members is the applicant (the formal model's counter-deployment `self_vouch_counted`); the refusal is the community's admission check, which must refuse an applicant who is already a member (record 024 carries the key-level exclusion as its clause 7)
- that the k vouchers are k distinct natural persons — distinct member leaves, not distinct people (record 002's negative space; personhood is never inferred)
- the invitation or the identity check — the VIC and the vetting statement are presented as credentials beside this proof, not proven inside it (the vetting statement's PASS limits carry: cred-spec PR #50)
- that the community will admit the applicant — the proof's outcome is one input to the community's policy engine; verifying it, accepting it under policy and issuing the VMC are three acts (WG-04)
- what the VRCs say beyond naming the applicant as subject — attributes, scopes and the relationship's own terms are outside the statement
- that a community's admission path mints the witness this record needs — an admission that issues no relationship credentials has no VRC to prove over; record 024 states the hidden-vetting statement such a path can run

#### Adversary, per claim

- **verifier** — the issuing community learns nothing about which members vouched beyond the count reaching k — no voucher identifier, VRC-side or VMC-side, no path position, no leaf
- **verifiers-colluding** — two join requests by one applicant to two communities, or a repeated request to one, cannot be linked through this proof beyond the disclosed admission identifier — the proof emits no identifier-derived value; the admission identifier is disclosed by the applicant's own act
- **registry-operator · issuer-verifier-colluding** — fetching root_C and rl_root does not identify the applicant or the vouchers — holds only if roots are fetched without a per-applicant query

#### Horizon

- earliest of: each VRC's validity · each voucher's membership validity · epoch rollover · status freshness (the community's published bound) · root_C cryptoperiod
- the join request's own validity — a proof bound to a withdrawn or decided request (Trust Tasks `vtc/join-requests/withdraw`, `decide`) is not replayable to the next one

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `voucher-not-member (unsat: a VRC issuer with no grant leaf under root_C)`, `voucher-duplicate (unsat: one member leaf behind two VRCs)`, `vouch-count-below-k (unsat: fewer than k distinct vouchers)`, `subject-mismatch (unsat: a VRC naming a subject the applicant cannot open to the admission identifier)`, `voucher-linkage-missing (unsat: a `pairwise` VRC issuer with no co-control attestation and no directed reuse — the offline-linkage question of zkp-tf #18)`, `voucher-revoked (unsat at epoch)`, `vrc-signature-invalid (verify)`, `transcript-digest-mismatch (verify)`, `join-request-context-mismatch (verify: the proof was made for another community or another request)`, `rl-root-stale`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| Berkeley's reference code for the admission proof — the construction being built for the Linux Plumbers integration (per Arka Rai Choudhuri; Glenn Gore integrating), named by the chair as the constructor; link, proving system and measured figures to be recorded when the code is public | unmeasured here; to be filled from the reference code's own figures with revision and workload named | unmeasured | zkp-spec PR #11 (ScottJeezey 2026-09-21); ePrint 2026/333 and its forthcoming community follow-up (the 8 September call) |
| Groth16 / BN254 / Poseidon composition — k parallel instances of record 010's clauses 1–3, 6, 7 (VRC authenticity, voucher membership, voucher linkage, non-revocation) sharing one holder-binding clause (004), one distinctness clause over the k leaves (005) and one transcript binding (003); the applicant's own membership clause of 010 dropped, since the applicant is not a member | unmeasured for the complete statement; roughly k × the voucher-side share of record 010 plus one shared binding — component figures cannot be added into a measurement | unmeasured | this record's composition of 001 · 003 · 004 · 005 · 006 · 007; record 010's option 1 |
| vouchers disclosed — the applicant presents the k VRCs and the vouchers' membership evidence in the clear; the community checks distinctness and membership against its registry. No proof; the privacy claims above do not apply | zero | unmeasured | general #31 (the flow as drawn); a community policy that requires knowing its vouchers |
| blind-signature vouch — each voucher blind-signs at vouch time and the applicant later proves possession of k unblinded signatures from distinct members (record 010's option 5, applied k times); composition with the membership and non-revocation clauses is the open design question | unmeasured — no construction published | unmeasured | DTG ZKP TF call 2026-09-08 (Sanjam Garg) |

#### Issuance requirements

- as record 010's X3 line: the VRCs' signatures must be provable in-circuit, or a ZK-openable commitment must sit beside each (cred-spec #17); the signature scheme is the non-swappable choice
- the community publishes root_C and rl_root per epoch, fetchable anonymously, and declares k in its admission policy (Governance Considerations 4; WG-05)
- each voucher's VRC-side identifier must be linkable to the voucher's VMC-side identifier offline: the voucher reuses one `directed` identifier, or the VRC carries the voucher's co-control attestation (the MAY of cred-spec #9; record 007 at issuance) — the same offline-linkage requirement as record 010, now on k vouchers
- the applicant's identifier(s) named in the VRCs must be, or carry, a ZK-openable commitment to the applicant's secret where they differ from the admission identifier (record 007; WG-14) — or the applicant uses one `directed` identifier toward every voucher and the community, in which case clause 5 is holder binding alone
- the join request (Trust Tasks `vtc/join-requests/submit`, with `supplement` for a deferred answer) carries the presentation, and its exchange is the context descriptor the proof binds — the citation of that exchange on the VMC later issued is record 008's concern (cred-spec #58)
- the invitation (VIC) and the identity check (the vetting statement) travel beside the proof as credentials; the policy engine combines them with the proof's outcome under the community's rules (WG-04)
- the context the proof binds should include a challenge the community mints per applicant and spends when the proof is counted — a proof verifies as often as it is submitted, so a join-request context alone lets the same bytes count twice; an applicant answering `requestMore` obtains a fresh challenge

#### Provenance

- zkp-spec PR #11 review (ScottJeezey 2026-09-21, Round 1 position): the build ask — an admission proof, a different statement from 010, Berkeley's reference code as constructor, LPC-relevant
- general #31 (stormer78 2026-08-26) — the Phase 4 join: 1 member invite + 2 member vouches + ID check, presented once; 'rules do the gatekeeping, not people'; mapped to records in the reply of 2026-09-21 (AA)
- DTG ZKP TF call 2026-09-08 (Arka Rai Choudhuri): the community-modelling follow-up to ePrint 2026/333 is what the Linux-kernel work ties to; the blind-signature vouch (Sanjam Garg)
- cred-spec §VMC (Verifiable Membership Credential), both directions; §Membership Edge Completion — the voucher's membership is the community-issued grant half (Governance Considerations 1)
- cred-spec §Community-Anchored Zero-Knowledge Proof — statement 3's offline-linkage requirement, applied here to each voucher
- trust-tasks-tf `vtc/join-requests` (submit · supplement #526 · withdraw #518 · decide) and #543 (a community can ask an applicant to tell it about themselves) — the exchange the proof binds
- record 010 — the sibling statement (a member shows a verifier a relationship with an offline voucher); this record drops the presenter's own membership and multiplies the voucher side by k
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-21 | `requested` | ScottJeezey (chair) on zkp-spec PR #11, for the record on zkp-tf #23 | Round 1 position, 'Build': the two-vouch admission proof has no home; add it and reference Berkeley's reference code as the constructor |
| 2026-09-21 | `specified` | mitchuski | statement, witness, public inputs, seven clauses composed from 001 · 003 · 004 · 005 · 006 · 007, disclosure set, negative space, three adversary claims, horizon, ten rejection codes, four options (the reference code named, unmeasured); written from the chair's ask, the Phase 4 flow and record 010's sibling statement — the constructor's own figures are the next evidence |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | doesNotEstablish line added from the formal model: no clause excludes a self-vouch by an applicant who is already a member; the refusal is placed on the admission check; no state change |
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/R023/Soundness.lean (with Model.lean, NegativeSpace.lean) in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.R023.Statement — at least k pairwise-distinct members, each with its grant leaf in the membership tree and its handle unrevoked, each linked through one controller to a VRC whose subject is the applicant's admission identifier or opens to it
- scope: clauses 1–6 as a counting model; clause 7 (transcript) is record 003's; consent, invitation, identity check and admission are outside any clause

| theorem | proves |
|---|---|
| `Formal.R023.soundness` | the verifier's relation (clauses 1–6 as gadget relations) establishes the statement, through record 001's path_sound, record 005's distinct-leaves lemma and record 006's neighbours_sound |
| `Formal.R023.no_vrc_no_statement` | does-not-establish 8: an admission path that issues no VRC can never satisfy the statement, for any k ≥ 1 |
| `Formal.R023.self_vouch_counted` | does-not-establish 3: an applicant who is already a member vouches for itself through its own grant — the relation accepts k = 2 and the statement holds |

| hypothesis | carries |
|---|---|
| H-sig | clause 1: a verifying VRC is one a member issued |
| H-leaf, H-node, H-domain (record 001) | the membership tree's hashes are collision-free and leaves and nodes are hashed under separate domains |
| H-sorted (record 006) | the revocation root is committed over strictly sorted handles |
| H-com (record 007) | the identifier derivation binds (secret, salt) |


### Construction 024 · Hidden-vetting admission — an applicant proves k attestations from pairwise-distinct eligible vetters of the issuing community, without disclosing which vetters

*This record is at state `constructed`: a runtime exists and has measured at least one construction option; no independent party has reproduced it. Informative.*

| | |
|---|---|
| kind | composed |
| state | `constructed` |
| priority | P1 |
| constructor | mitchuski |
| requested by | specified and constructed by the ZKP TF co-chair |
| request | zkp-spec PR #11 (ScottJeezey 2026-09-21, Round 1 position, 'Build'): the admission proof being built for the Linux Plumbers integration · trust-tasks-tf `vtc/join-requests/manifest/0.2`: `vetting.ext` / `vetting.extCritical`, with `org.openvtc.hidden-vetting` as the example namespace of a criterion that must not receive named statements · general #31 (stormer78 2026-08-26): the Phase 4 join, whose identity check is the vetting step |

**Composes:** [001](#construction-001-%C2%B7-set-membership-over-an-accredited-root) ∧ [002](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection)) ∧ [003](#construction-003-%C2%B7-transcript-binding) ∧ [004](#construction-004-%C2%B7-holder-binding-(key-from-secret)) ∧ [005](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

The issuing community learns that k pairwise-distinct eligible vetters of the live period each attested this applicant's per-application identifier with the stated facts, without learning which of its vetters attested, any vetter's DID, or whether a vetter has attested for anyone else.

**Need.** the identity-vetting step of a community's join: the criterion needs k statements from distinct eligible vetters, and a community running hidden vetting counts them without learning which of its vetters made them — so no vetter's DID reaches the community

#### Witness

*Never leaves the holder.*

- for each attesting vetter: its secret and the evidence that it is an eligible vetter of the live period (in the Groth16 route, the opening of its enrolled commitment and its Merkle path)
- for each attestation: the profile-specific spend witness — either a secret-derived slot within the cap, or a valid blind-issued token and its opening
- the applicant's per-application secret, from which the attested identifier derives

#### Public inputs

- the community, eligible-vetter set commitment and live period, plus the selected cap profile and its parameters: per-vetter quota or aggregate token budget for a defined vetter set and accounting interval
- k — the criterion's `minStatements`, with its per-method floors and independence caps
- the requirements digest of the criterion version the applicant started under
- the applicant's per-application identifier and join DID — disclosed by construction, since the applicant is applying
- the challenge — minted by the community for this applicant and spent when the proof is counted
- transcriptDigest over the challenge, the requirements digest, the join DID and every attestation's public values

#### Relation

1. each attester is an eligible vetter of the live period, proven without its identifier — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-%C2%B7-set-membership-over-an-accredited-root)) · runtime `runtimes/circom-gadget/circuits/vetting_attest.circom`
2. each attestation carries a tag derived from its vetter's secret and the applicant's identifier — deterministic for one vetter and one applicant, unlinkable across applicants, and not recomputable from anything public — [[ref: nullifier]] ([[ref: construction record]] 002, [Scoped nullifier (reuse detection)](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection))) · runtime `runtimes/circom-gadget/circuits/vetting_attest.circom`
3. the k tags are pairwise distinct, so one vetter attesting twice counts once — [[ref: distinctness]] ([[ref: construction record]] 005, [Distinct member / distinct issuer](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer)) · runtime `runtimes/circom-gadget/harness-vetting.mjs`
4. each attestation consumes an authorized serial in the declared community, profile and accounting interval; the same spend cannot increase the count twice. A profile may derive serials from a stable vetter secret with bounded slots, or use blind-issued transferable tokens under an aggregate issuance budget. A conflicting reuse is refused; a profile may acknowledge an identical resubmission without adding a counted attestation — [[ref: nullifier]] ([[ref: construction record]] 002, [Scoped nullifier (reuse detection)](#construction-002-%C2%B7-scoped-nullifier-(reuse-detection))) · runtime `runtimes/circom-gadget/circuits/vetting_attest.circom`
5. the attested identifier derives from the applicant's per-application secret, which the applicant holds — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-%C2%B7-holder-binding-(key-from-secret))) · runtime `runtimes/circom-gadget/circuits/vetting_bind.circom`
6. the submission is bound to transcriptDigest — the community's challenge, the requirements digest, the join DID and every attestation's public values — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-%C2%B7-transcript-binding)) · runtime `runtimes/circom-gadget/circuits/vetting_bind.circom`
7. no attester is the applicant — [[ref: distinctness]] ([[ref: construction record]] 005, [Distinct member / distinct issuer](#construction-005-%C2%B7-distinct-member-%2F-distinct-issuer))

#### Disclosure set

- the outcome (k distinct eligible vetters attested this applicant / not shown)
- k tags — one per attestation, standing in the community's counting facts where a vetter DID stands on the named path
- per statement: the facts the community's counting rule reads (method, verified claim types, validity window, identity commitment)
- serials and their declared spend scope, checked against the community ledger; a profile defines whether identical resubmissions are refused or acknowledged without increasing the count
- the vetter-set commitment, period, selected cap profile and accounting parameters; the requirements digest, join DID, challenge and transcriptDigest

#### Does not establish

- that the k attesters are k distinct people — distinct tags mean distinct vetter secrets; a vetter who hands its secret to another lets one person attest under two tags
- that a transferable-token budget limits each vetter: vetters can pool tokens. A per-vetter quota requires a profile that binds authorized spends to a stable enrolled secret and bounds slots, or another construction demonstrating equivalent enforcement. Neither profile bounds distinct people or prevents sharing a secret (DG-024-4)
- that no attester is the applicant, in the Groth16 route — clause 7 is not built there; it holds only while applicants are outside the vetter set
- that the vetting session happened as attested — the vetter's check of the person is outside the proof, as on the named path
- anything to a party other than the issuing community — it keeps the vetter set and verifies the proof; no third party can audit that admission followed the criterion
- that the applicant is admitted — verifying the proof, counting it under policy and issuing the membership grant are three acts (WG-04)
- any relationship between the applicant and the vetters — no relationship credential (VRC) is involved; a policy requiring a VRC pair with the vetters would disclose what this proof withholds
- that proof acceptance enforces the complete admission policy: per-method floors, independence caps, statement validity and eligibility checks belong to the named policy evaluator. The lab harness checks total tag count and returns method facts; it does not implement the complete evaluator
- that sequential replay tests establish concurrent or durable single-use behavior: the ledger must atomically prevent additional counting across requests, retries and restarts; the current asynchronous lab harness requires further review

#### Adversary, per claim

- **verifier · issuer-verifier-colluding** — the community learns which of its eligible vetters attested no better than chance over the vetter set, less whatever the disclosed per-statement facts narrow that set
- **verifier** — one vetter's attestations for two applicants cannot be linked through their tags or serials

#### Horizon

- earliest of: the vetter-set period · the vetter-set commitment in force (a vetter removed from the set cannot attest under the new commitment) · each statement's validity and the criterion's `maxStatementAge` · the requirements digest's `requirementsGrace`
- the challenge — single use: spent when the proof is counted, so an applicant answering `requestMore` obtains a fresh challenge

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `tag-duplicate (two attestations from one vetter)`, `token-serial-spent (conflicting or additional counted reuse; the lab profile refuses every repeat)`, `vetter-not-enrolled (unsat)`, `vetter-cap-exceeded (secret-derived profile only: a slot at or above the cap)`, `vetter-root-stale (verify: an attestation made under a replaced vetter-set commitment)`, `statement-metadata-mismatch (verify)`, `requirements-digest-mismatch (verify)`, `join-did-mismatch (verify: an attestation made for another applicant)`, `binding-proof-invalid (unsat: a binding made without the applicant secret)`, `challenge-replayed (verify)`, `challenge-not-minted (verify)`, `attestation-count-below-k`, `unsupported-extension (the criterion's `extCritical` names a namespace the client does not implement: it stops before gathering named statements)`

#### Construction options

*Candidate constructions, each evaluated against the construction-selection criteria ([DTG-ZKP-REQ] §16.1), with its cost as measured or as conjectured.*

| construction | cost | status | source |
|---|---|---|---|
| the construction the chair named on PR #11 for the Linux Plumbers integration (Berkeley, with Glenn Gore integrating) — not public; its construction and figures are recorded here when its authors publish them | not recorded | unmeasured | zkp-spec PR #11 (ScottJeezey 2026-09-21) |
| vetters named — the named path: each vetter issues a signed vetting statement and the community counts vetter DIDs. No proof; the privacy claims above do not apply | zero | unmeasured | trust-tasks-tf `vtc/join-requests/manifest/0.2` (VettingRequirements) |
| Groth16 / BN254 / Poseidon (lab) — each vetter proves its own attestation (vetting_attest: leaf of the vetter-set root, tag = Poseidon(tag, secret, applicantId), serial = Poseidon(tag, secret, period, slot) with slot < cap, statement bound) and the applicant proves one binding (vetting_bind: applicantId from the applicant secret, transcript over the challenge, requirements digest, join DID and every attestation). The tag must come from the vetter's secret — a tag derived from anything public the community could recompute for every vetter — and the applicant does not hold vetters' secrets, so the route takes k + 1 proofs. Removing one vetter is a new root with nobody re-enrolled; the cap is per vetter and in-circuit, with no token issuance; tag distinctness is checked by the community over the public tags; clause 7 is not built | vetting_attest 5,677 constraints, vetting_bind 238 (--O2); one attestation proof ~813 ms, made by each vetter at its own time, the binding ~123 ms (snarkjs/wasm, Node 22, win32/x64); community verification at k = 5 ~110 ms; submission at k = 5 1,376 B with compressed points (2,144 B uncompressed), computed from point sizes, 7,009 B as snarkjs JSON; lab-only trusted setup (one contributor, fixed entropy) | **measured** | evidence repository runtimes/circom-gadget/test-vetting.mjs, run 2026-09-23 — H1–H12 12/12 |

#### Issuance requirements

- the community publishes the eligible-vetter set, live-period policy and selected cap profile. A secret-derived profile specifies a stable enrolled secret and bounded slots per accounting interval. A transferable-token profile specifies the exact vetter set, total issuance budget, quota/tick rules, token expiry and carry-over treatment; it does not imply a cap on each holder
- the criterion publishes its parameters under `vetting.ext` in a namespace the community controls and names that namespace in `vetting.extCritical` (trust-tasks-tf `vtc/join-requests/manifest/0.2`), so a client that does not implement it refuses instead of gathering named statements
- the community mints a challenge per applicant, keeps it, and spends it when the proof is counted
- no signed vetting statement and no relationship credential is issued on this path
- the requirements digest binds the cap profile, community, accounting interval and policy parameters. Define the serial namespace and identity of an identical spend; ledger updates and counting must be atomic and durable. An acknowledged retry adds neither a spend nor an attestation, and does not reuse a consumed challenge
- name the admission policy evaluator and version: it applies per-method floors, independence caps, validity and eligibility rules to verified facts before a separate issuance decision. Assign these checks explicitly when the proof harness does not implement them

#### Provenance

- zkp-spec PR #11 (ScottJeezey 2026-09-21): the admission proof for the Linux Plumbers integration; record 023 is the vouch-shaped statement specified from that ask
- trust-tasks-tf `vtc/join-requests/manifest/0.2`: `VettingRequirements` (`minStatements`, `minByMethod`, `eligibleVetters.role`, `independence`), `ext` and `extCritical`
- general #31 (stormer78 2026-08-26): the Phase 4 join — invite, vouches, identity check
- record 023 — the sibling admission statement over member-issued relationship credentials
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-23 | `specified` | mitchuski | statement, witness, public inputs, seven clauses composed from 001 · 002 · 003 · 004 · 005, disclosure set, negative space, two adversary claims, horizon, rejection codes, three options — from the chair's ask and the trust-tasks vetting requirements |
| 2026-09-23 | `constructed` | mitchuski | Groth16 route built in the evidence repository's lab (vetting_attest + vetting_bind + the community's admission check), H1–H12 12/12; clauses 1–6 carry runtimes, clause 7 is not built |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-23 | mitchuski | clause 4 loosened to admit blind, transferable serials capped per vetter set, alongside serials derived from the vetter's secret; doesNotEstablish line added for the per-vetter cap (decision gate DG-024-4, zkbook/DECISION-024-clause4.md); no state change |
| 2026-09-23 | mitchuski | Local draft: align witness, public inputs, disclosure, issuance and fixtures with the two cap profiles; distinguish idempotent acknowledgement from double counting and identify external policy and atomic-ledger obligations. No state promotion or task-force adoption; no new runtime or formal conformance claim. |
| 2026-09-23 | mitchuski | formal model built (Lean, composed from the gadget library); no state change |

#### Formal verification

*A machine-checked model of this record. It proves the listed properties of the abstract relation or policy model; it is not evidence of the construction's cryptography — the hypotheses name what remains to discharge against the construction. It confers no evidence state.*

- system: Lean 4.33.1, core library only (no Mathlib); axiom footprint within propext, Classical.choice and Quot.sound — no sorry, no added axiom
- location: formal/Formal/R024/Soundness.lean (with Model.lean, SingleUse.lean, NegativeSpace.lean) in the evidence repository (uncommitted at this revision)
- reproduce: sh formal/scripts/check-axioms.sh — builds, prints each theorem's axiom footprint, fails on sorry or a non-standard axiom
- statement: Formal.R024.Statement, read for a class-credential construction — at least k pairwise-distinct members, none the applicant, each holding a credential under a live label whose tag on the applicant's identifier is among those counted
- scope: the counting rule and the single-use rules over abstract primitives; not the construction's cryptography, unlinkability or the adversary claims; per-vetter cap attribution (DG-024-4) is not modelled

| theorem | proves |
|---|---|
| `Formal.R024.soundness` | if the counting rule accepts (tags extractable, pairwise distinct, at least k), the statement holds — through record 005's owners_distinct with record 002's tag as the value |
| `Formal.R024.completeness` | k pairwise-distinct honest vetters give k pairwise-distinct tags — clause 3 does not over-collapse |
| `Formal.Gadgets.run_once` | clause 4 and the challenge: over any run of presentations a spent value is counted at most once |
| `Formal.R024.not_minted_refused` | a challenge the community did not issue is refused |
| `Formal.R024.batch_over_cap_refused` | an issuance batch over the cap is refused (DG-024-4: both cap profiles share this) |
| `Formal.R024.distinct_members_not_distinct_people` | does-not-establish 1: one person holding two memberships — the rule accepts and the statement holds |
| `Formal.R024.rekey_counts_twice` | where one proof mixes periods, a vetter whose secret changes between them counts twice: the secret must be stable across periods |

| hypothesis | carries |
|---|---|
| H-Σ (Extractable) | clauses 1, 2 and 7: soundness of the construction's proof — every accepted tag comes from a credential issued under a live label, not under the applicant's key |
| H-stable (SecretStable) | a vetter's secret is one across periods, where a proof mixes them |
| H-applicant (ApplicantSecret) | an applicant holding a credential holds it over its own key; vacuous for one holding none |
| H-fresh, H-tag | completeness only: two members never share a secret; tags do not collide for one applicant |
<!-- generated-section:constructions:end -->

<!-- generated-section:stacks:start -->
## Proving Systems

This section is informative.

A proving-system entry records facts a reader can check — proof system, field, setup, licence, audit statement, published figures with their source and a verification date — and never a recommendation. Recommendations require separate task-force review informed by the reproduction ladder over verification-registry rows; a figure in this section is the proving system's own or the evidence repository's, and says so. Entries of different kinds are not comparable rows: a catalog of circuits over credentials as already signed and a general-purpose prover answer different questions, and the kind is stated first. This section is generated from `conformance/stacks/`.

| kind | entries | what the kind means |
|---|---|---|
| general-stack | [flock](#proving-system-%C2%B7-flock-%E2%80%94-binary-field-snark-for-batched-boolean-computation-(standard-hashes)) · [provekit](#proving-system-%C2%B7-provekit-(world)-%E2%80%94-noir-%E2%86%92-whir-client-side-proving) | general-purpose proving systems — any statement the frontend expresses; issuer-agnostic |
| hand-rolled | [lab-groth16-circom](#proving-system-%C2%B7-lab-groth16-(circom-2-%C2%B7-snarkjs-%C2%B7-bn254-%C2%B7-poseidon)) | constructions written and measured in the evidence repository as reference implementations |
| as-signed-catalog | [siros-longfellow](#proving-system-%C2%B7-siros-circuit-catalog-%E2%80%94-longfellow-libzk-v1-(mdl-%2F-eudi-pid%2C-as-signed)) | catalogs of circuits that prove over credentials exactly as already signed — substrate for the legacy-rails route, not DTG construction routes |

### Proving system · SIROS circuit catalog — Longfellow libzk v1 (mDL / EUDI PID, as signed)

| | |
|---|---|
| id | `siros-longfellow` |
| kind | as-signed-catalog |
| maintainer | SIROS Foundation (catalog); Google (Longfellow libzk) |
| frontend | prebuilt circuits for ISO 18013-5 mDL and EUDI PID presentations over ECDSA-signed documents |
| proof system | Longfellow libzk v1 (Google) — ZK over existing ECDSA/mdoc rails; also catalogued: Microsoft vega-prover fork (zk-cred-vega, P-256 mDL prover/verifier keys) |
| field | as required by the signed document's curve (P-256) |
| setup | per artifact — the catalog pins downloaded artifacts by sha256, not builds |
| post-quantum | no (ECDSA rails) |
| credential model | as-signed: proves statements over credentials exactly as issuers already issue them — no issuer change, no commitment choice; this is substrate for the legacy-rails route, not a DTG construction route |
| platforms | as published per artifact |
| verifier | per system |
| licence | BSD-2-Clause (catalog repo); per-entry `source.license` field — citable; artifacts stay in the catalog, the book cites entry ids |
| audit | 'Every circuit here is experimental and unvetted' |
| maturity | experimental catalog — 14 entries (manifest v1, 2026-09-05): 10 Longfellow libzk v1 (systemVersion 6/7/8, 1–4 attributes, ~300 KB each) + 4 vega-mc P-256 keys (r11 deprecated, r12 active, 135–157 MB) |
| independent implementations | 0 |
| provenance | repo: https://github.com/sirosfoundation/go-zk-circuits · manifest: https://api.circuits.siros.org/v1/manifest.json · version: manifestVersion 1 · entrySchema: id · aliases · system · systemVersion · docTypes · published · status · params · artifact{url, sha256, size, zstd, uncompressed} · source{origin, toolchain, license, openSource} · publishedAt · notes · content-addressed |
| verified | 2026-09-05 — api.circuits.siros.org/v1/manifest.json, go-zk-circuits README |

#### Published figures (the proving system's own, or the evidence repository's — never this specification's)

*No benchmark figures recorded.*

#### What it covers, per gadget

| gadget | coverage |
|---|---|
| [[ref: set-membership]] | no (not a DTG statement) |
| [[ref: nullifier]] | no |
| [[ref: transcript-bind]] | challenge binding per system |
| [[ref: key-binding]] | device binding per mdoc |
| [[ref: distinctness]] | no |
| [[ref: signature-verify]] | yes — ECDSA over the signed document (the whole point) |
| [[ref: non-revocation]] | per system |
| [[ref: range]] | attribute predicates (age) — yes |
| [[ref: commitment-open]] | no |
| [[ref: chain-resolve]] | no |
| [[ref: hidden-equality]] | no (not a DTG statement; equality of two signed fields would be a new circuit) |

#### Notes

- Legacy rails: prove over what was already signed. The DTG constructions assume issuers who can choose commitments; the two classes must stay visibly apart in any comparison.
- The catalog records provenance (content-addressed bytes, origin commit, toolchain, licence) and declines to claim correctness; the registry records independent reproduction — complementary halves (zkp-tf #17, 2026-08-25). Pilot: run one entry (`longfellow-libzk-v1_8_1_4259_2945`) through the acceptance flow.

### Proving system · Flock — binary-field SNARK for batched Boolean computation (standard hashes)

| | |
|---|---|
| id | `flock` |
| kind | general-stack |
| maintainer | Succinct · Espresso Systems · NYU — designers Ron Rothblum (Technion), Benedikt Bünz (Espresso Systems, NYU), William Wang (NYU) |
| frontend | R1CS over binary fields; batches of Boolean circuits — standard hash functions (SHA-256, Keccak-f[1600], BLAKE3) are the native workload |
| proof system | Flock: a SNARK for proving batches of Boolean computations; Ligerito (Reed–Solomon, hash-based) polynomial commitment; ring-switching from the Binius line of work |
| field | binary fields (GF(2^k) towers) — no prime-field arithmetic, no elliptic curve in the prover |
| setup | transparent — no trusted setup, no toxic waste |
| post-quantum | plausibly post-quantum: security rests on hash functions; designed for the hash-based signature schemes (Lamport, Winternitz, XMSS) Ethereum's post-quantum transition targets |
| credential model | Candidate backend for batched standard-hash computation. Author-reported hash benchmarks are not end-to-end measurements of DTG membership, revocation, transcript or signature verification; compatibility depends on the selected registry and credential profile. |
| platforms | rust · x86_64 · apple-silicon |
| verifier | local · server |
| licence | Apache-2.0 / MIT (benchmark repository, dual-licensed); check LICENSE in succinctlabs/flock before citing the core as such — compatible with an Apache-2.0 code / CC BY 4.0 docs deliverable |
| audit | none claimed — research release with a paper (June 2026) |
| maturity | research release (2026-06-25), under active optimisation for Ethereum's post-quantum throughput; not yet a credential-presentation toolkit |
| independent implementations | 0 |
| provenance | repo: https://github.com/succinctlabs/flock · paper: https://github.com/succinctlabs/flock/blob/main/paper/flock-paper.pdf · benchmarkRepo: https://github.com/Layr-Labs/flock-challenge (Apache-2.0 / MIT) · optimisedAt: https://www.yukon.org/flock — the BLAKE3 R1CS prover being made fast for Ethereum on x86; a prebuilt, checksum-pinned verifier controls private inputs, timing, correctness and score |
| verified | 2026-09-05 — blog.succinct.xyz/introducing-flock, Layr-Labs/flock-challenge README (local clone), yukon.org/flock |

#### Published figures (the proving system's own, or the evidence repository's — never this specification's)

| statement | device | figure | source |
|---|---|---|---|
| BLAKE3 compressions | Apple M4 Max, single core | 82,100 compressions/s | blog.succinct.xyz/introducing-flock (2026-06-25) |
| SHA-256 compressions | Apple M4 Max, single core | 42,100 compressions/s | same |
| Keccak-f[1600] permutations | Apple M4 Max, single core | 30,700 permutations/s | same |
| BLAKE3 compressions | Apple M4 Max, ten cores | > 660,000 compressions/s — 'enough to prove the hashing for roughly 4,000 transactions per second' under Lean Ethereum's leanVM design | same |
| 2^18 = 262,144 BLAKE3 compressions, one proof | official benchmark runner (Apple M3 Max 10P / c7i.4xlarge x86) | proof ≈ 436–438 kB; timing = median of 100 fresh, verified runs | Layr-Labs/flock-challenge README (measurement contract) |
| relative | — | 8.4× Binius64 on SHA-256; 14× Binius64 and Plonky3 on BLAKE3; 1.8× Hashcaster on Keccak (single core) | blog.succinct.xyz/introducing-flock |

#### What it covers, per gadget

| gadget | coverage |
|---|---|
| [[ref: set-membership]] | yes — a Merkle path over BLAKE3/SHA-256 is a batch of compressions, Flock's native shape (unmeasured for our tree) |
| [[ref: nullifier]] | yes where the PRF is a standard hash (BLAKE3-keyed) — the Poseidon nullifier would be re-specified |
| [[ref: transcript-bind]] | yes — a SHA-256 digestMultibase transcript digest is provable natively (the canonical transcript needs no Poseidon detour) |
| [[ref: key-binding]] | as commitment-open over hash commitments; curve-based key derivation would be a Boolean circuit — cost unknown |
| [[ref: distinctness]] | yes — Boolean comparison is cheap in binary fields |
| [[ref: signature-verify]] | hash-based signatures (XMSS/Winternitz) natively; ECDSA/Ed25519 as Boolean circuits — cost unknown, likely the expensive case |
| [[ref: non-revocation]] | yes — indexed (sorted-leaf) tree over a standard hash |
| [[ref: range]] | yes — Boolean comparison |
| [[ref: commitment-open]] | yes — hash commitments |
| [[ref: chain-resolve]] | unmeasured — no recursion story published for the credential case |
| [[ref: hidden-equality]] | yes — Boolean equality is cheap in binary fields; the openings over standard hashes are the native shape |

#### Notes

- Included to investigate whether standard-hash proving can support existing registry commitments. Applicability, proof size and total credential costs require a matched workload; no recommendation follows from isolated hash throughput.
- Where it is useful for the constructions: any clause that is 'a hash chain over standard hashes' — set membership (001), non-revocation (006), transcript binding (003) — and therefore the composed community-anchored proof (010) on its hash side; signature clauses over curve-based credentials are the open cost.
- Compare setup assumptions, security, proof size, issuance compatibility and full workload costs under the same profile. Existing benchmark sizes do not establish the corresponding DTG proof size.
- The measurement discipline of its benchmark harness — a pinned verifier that decides correctness and timing, many fresh runs, a median — is the same shape as this specification's reproduction ladder and is cited in PLAN §2.4 as prior art for how a cost row earns 'measured'.

### Proving system · ProveKit (World) — Noir → WHIR client-side proving

| | |
|---|---|
| id | `provekit` |
| kind | general-stack |
| maintainer | World Foundation · Atheon · Reilabs · Nethermind |
| frontend | Noir (ACIR → R1CS lowering) |
| proof system | Spartan-style prover with WHIR polynomial commitments; Skyscraper hash for BN254 commitments; optional Groth16 recursive wrapper (Go/gnark) for on-chain verification |
| field | BN254 scalar field |
| setup | transparent for the WHIR proof; the Groth16 wrapper reintroduces a trusted setup |
| post-quantum | hash-based core — migration path plausible; the pairing-based wrapper is not |
| credential model | issuer-agnostic: any statement Noir expresses; shipped statements prove over existing credentials (passport, WebAuthn) as signed |
| platforms | rust · javascript-wasm (~340 KB) · swift (iOS) · kotlin (Android) · python · c-ffi |
| verifier | local · server · browser · on-chain (Groth16 wrapper) |
| licence | MIT — compatible with an Apache-2.0 code / CC BY 4.0 docs deliverable |
| audit | v1.0.0 described as 'the current stable, audited release' — *claimed; reviewed commit and file set not located (§16.1 audit-scope rule)* |
| maturity | production — 'used in production by World, Atheon, and a handful of other partners' (v1.0.0) |
| independent implementations | 0 |
| provenance | repo: https://github.com/worldfnd/provekit · version: v1.0.0 |
| verified | 2026-09-05 — provekit.org, docs.provekit.org, github README |

#### Published figures (the proving system's own, or the evidence repository's — never this specification's)

| statement | device | figure | source |
|---|---|---|---|
| Passport P1 | iPhone SE 3 | 2.43 s prove · 2.55 MB payload · 716 KB proof | provekit.org/benchmarks (v1.0.0) |
| WebAuthn | Moto E15 (2 GB, 32-bit) | 27.9 s prove · 2.39 MB payload · 716 KB proof · ~494 MB peak RSS | same |
| OPRF | iPhone SE 3 | 1.20 s prove · 1.65 MB payload · 635 KB proof | same |
| baselines quoted by the vendor | same devices | Circom+Groth16 payload 27–1754 MB, proof ~1 KB; Noir+Barretenberg payload ~271 MB, proof 16–21 KB | same |

#### What it covers, per gadget

| gadget | coverage |
|---|---|
| [[ref: set-membership]] | yes (Poseidon/Skyscraper Merkle in Noir) — unmeasured for our tree |
| [[ref: nullifier]] | yes — unmeasured |
| [[ref: transcript-bind]] | yes — unmeasured |
| [[ref: key-binding]] | yes — unmeasured |
| [[ref: distinctness]] | yes — unmeasured |
| [[ref: signature-verify]] | yes — ECDSA/RSA examples shipped (passport, WebAuthn) |
| [[ref: non-revocation]] | yes (indexed tree) — unmeasured |
| [[ref: range]] | yes |
| [[ref: commitment-open]] | yes |
| [[ref: chain-resolve]] | recursion via the Groth16 wrapper — unmeasured |
| [[ref: hidden-equality]] | yes — unmeasured |

#### Notes

- Proof size ~700× Groth16; proving payload ~100× smaller — the trade the §25 gate weighs per profile (ADR-001 D1/D3).
- The 'audited' claim names no commit or file set; the stack file says so until located (the §16.1 audit-scope rule).

### Proving system · Lab Groth16 (circom 2 · snarkjs · BN254 · Poseidon)

| | |
|---|---|
| id | `lab-groth16-circom` |
| kind | hand-rolled |
| maintainer | the evidence repository (mitchuski) — benchmarking vehicle, not the task force's selection |
| frontend | circom 2.x → R1CS |
| proof system | Groth16 over BN254 (snarkjs); Poseidon for all in-circuit hashing |
| field | BN254 scalar field; Baby Jubjub for in-circuit keys (planned) |
| setup | trusted setup per circuit — lab-only fixed-entropy setup, unusable for production, reproducible from a clean clone; production ceremony = an open item |
| post-quantum | no — pairing-based; migration path is a different stack (LIV-ALG-07) |
| credential model | issuer-chosen commitments: Poseidon leaf commitments; ZK-friendly signatures or an additional Poseidon/KZG commitment beside the credential signature (X3) |
| platforms | node · browser-wasm (snarkjs) · x86_64 · darwin-arm64 |
| verifier | local · server · browser · on-chain (Solidity verifier) |
| licence | Apache-2.0 (lab code); circomlib LGPL-3.0 in repo LICENSE vs GPL-3.0 declared in npm metadata — flagged — circomlib licence ambiguity is material for an Apache-2.0 deliverable; noted in NOTE-2026-08-19 |
| audit | none — reproduction and behaviour are not review |
| maturity | most mature toolchain available; the lab's three circuits are source-complete with deterministic setup scripts |
| independent implementations | 1 |
| provenance | repo: github.com/mitchuski/dtgwg-zkp-mage — runtimes/circom-gadget · version: artifacts.manifest.json (pinned required digests) · content-addressed |
| verified | 2026-09-05 — CIRCUITS.md, registry/data |

#### Published figures (the proving system's own, or the evidence repository's — never this specification's)

| statement | device | figure | source |
|---|---|---|---|
| nullifier_membership (depth-20 Merkle + domain-tagged nullifier + in-circuit transcript binding) | lab machine | 11,523 constraints · ~680 ms prove · ~8 ms verify · 721 B proof | CIRCUITS.md; registry rows 0002–0006 (independent reproduction, win32/x64 ↔ darwin/arm64 byte-identical required digests) |
| dual_issuer k=2 (two distinct accredited issuers; duplicate unsatisfiable) | lab machine | 10,717 constraints (--O2) · ~740 ms · ~8 ms · 725 B | CIRCUITS.md |
| guardian_threshold t=3 | lab machine | 16,078 constraints · ~830 ms · ~10 ms · 723 B | CIRCUITS.md |

#### What it covers, per gadget

| gadget | coverage |
|---|---|
| [[ref: set-membership]] | measured (11,523 with nullifier + binding) |
| [[ref: nullifier]] | measured (inside the same circuit) |
| [[ref: transcript-bind]] | measured (+1 constraint) |
| [[ref: key-binding]] | planned (Baby Jubjub identity) — unmeasured |
| [[ref: distinctness]] | measured (dual_issuer, guardian_threshold) |
| [[ref: signature-verify]] | unmeasured — the X3 case; EdDSA-on-Baby-Jubjub feasible, Ed25519/ECDSA expensive |
| [[ref: non-revocation]] | unmeasured — indexed tree ≈ 2× set membership (conjecture) |
| [[ref: range]] | cheap — unmeasured |
| [[ref: commitment-open]] | Poseidon opening — measured as the leaf commitment inside 001 |
| [[ref: chain-resolve]] | unmeasured |
| [[ref: hidden-equality]] | unmeasured — one zero-difference constraint over two Poseidon openings; the openings are the cost |

#### Notes

- The only stack with registry rows today: the reproduction ladder places it at reproduced-cross-arch for constructions 001/002/003/005.
- Chosen for toolchain maturity so the selection conversation happens against measured numbers; a PLONKish or folding counter-proposal through the same gate is invited (PATH-MAP P4).
<!-- generated-section:stacks:end -->

## Security Considerations

This section is informative.

1. **Soundness is a property of a construction, not of this document.** A construction record states clauses; only a runtime realises them, and only an independent reproduction shows that the realisation behaves. A record at state `specified` has not been shown sound by anyone. Readers MUST NOT read a record's presence in this specification as a claim that its construction is secure.
2. **Reproduction is not audit.** A registry entry records a reproduction claim and its supporting artifacts. The evidence-repository build-report acceptance checker compares submitted metadata with a manifest; it does not itself execute the suites, verify a proof or authenticate the independence of the submitter. Evaluate those forms of evidence separately. It does not establish that the circuit is free of under-constrained signals, unchecked booleanity or interface drift between an audited commit and a shipped one. An audit claim in a proving-system entry that names no reviewed commit and file set is printed as "claimed; not located".
3. **Trusted setup.** Constructions on pairing-based proving systems depend on a setup whose entropy must be destroyed. The reference constructions' setup is a fixed-entropy laboratory setup, unusable in production and marked as such; production deployment requires a ceremony, and the registry treats setup-chain digests as advisory because a real ceremony is machine-local by design.
4. **Composition.** Two constructions that are individually sound may leak jointly. Composed construction records are required to declare their own disclosure set and negative space rather than inherit the union of their parts, and are bound to one presentation transcript. Implementations MUST NOT present the components of a composed construction as separate proofs and claim the composed record's properties.
5. **Common control.** Under the Credentials Core Specification's three correlation scopes, two `pairwise` identifiers of one party differ by construction. A construction that reads one party out of two credentials must prove common control of both identifiers (construction 007) or rely on the party having declared one `directed` identifier; a presenter cannot prove a counterparty's common control without the counterparty's witness or attestation. The identifiers concerned may appear as credential subject or as credential issuer — the VRC-issuer case is statement 3 of construction 010. A chain predicate is a different relation: that a child credential's `issuer` equals its parent's `credentialSubject.id` involves no holder secret and is hidden-value equality across credentials signed by different parties (construction 009, inside constructions 020 and 021), not common control; reading the two as one primitive leaves the chain predicates unspecified while appearing covered (cred-spec #9). Records that need either say so; implementations that assume either silently are unsound.
6. **Self-vouching.** A community-anchored proof without a distinctness clause (S6) accepts a member vouching for themselves under two identifiers. The clause is added in construction 010 and is required of any implementation of that record.
7. **Replay and transcript binding.** Every construction is bound to one canonical transcript digest. A proof presented against a different transcript is a different proof and MUST fail verification.
8. **Post-quantum horizon.** Pairing-based constructions have no post-quantum path; hash-based proving systems do, at a cost in proof size. Each record's horizon and each proving system's entry state which applies. Suite agility requirements are those of [DTG-ZKP-REQ] §9.9.

## Privacy Considerations

This section is informative. Items 1–6 are written by the editors; the numbered list that follows them is generated from the construction records' adversary and negative-space fields and is regenerated whenever a record changes.

1. **A privacy claim without an adversary is not a claim.** Every privacy property in a construction record names the party it holds against — the verifier, verifiers colluding, issuer and verifier colluding, or the registry operator — and the horizon over which it holds. Properties not named are not claimed.
2. **Nullifiers are declared links.** A scoped nullifier is emitted only in contexts that declare reuse detection, and within such a context it is the only link between presentations by one holder. Full unlinkability and reuse detection cannot coexist in one context ([PoP-2026] §5.3); records parameterise the choice by context descriptor rather than promising both.
3. **Registry state fetches.** Establishing that a credential is current must not itself identify the holder. Constructions expose accepted roots and proofs while retaining membership paths and openings as private witnesses. A deployment describes how root and witness retrieval can correlate the holder, including any live state fetches. "No live lookups" is a profile default, not an absolute.
4. **Proof size as a correlator.** Proof size, timing and error surface can distinguish constructions and therefore holders. Records list proof size per option; profiles should fix one option per context so that the choice of construction does not itself disclose.
5. **What the credential layer holds.** A construction can blind only what the credential or framework gives it in committed form. Durable correlators that live in Trust Task artefacts — identifiers, thread identifiers, the task-context pairing — are outside any construction's protection until the framework commits to them (construction 008, [DTG-CRED-TF-39]); the credential specification's digest-valued references are enumerable until their hashed representation carries a salt (construction 022, [DTG-CRED-38]).
6. **Intentional correlation is the holder's act.** A proof of common control across identifiers (constructions 007, 012) discloses to the party it is made to and widens no identifier's declared scope. Verifiers MUST NOT infer from such a proof that the identifiers may be correlated elsewhere.

> **WG-15 — Discuss: blinded digest references (record 022).** Which of the credential specification's digest-valued members carry a salt, and where the salt lives, is that specification's disposition (cred-spec #38). Record 022 states what the proof needs of each placement — a salt the proof can treat as a witness, content-binding references kept bound to exact content, chain references salted only where a chain is proven undisclosed — and its proof-side preference (a salt member inside the referenced credential). Status: record specified; disposition open on #38.

<!-- generated-section:privacy:start -->
### Privacy claims as recorded, per construction

*Generated from the `adversary` field of every construction record. A claim appears here only against the party it is made against; a claim absent here is not made.*

- **Construction 001** — against verifier, verifiers-colluding: leaf position hidden
- **Construction 001** — against registry-operator: leaf hidden from the registry operator only if the root is fetched without a per-holder query
- **Construction 002** — against verifiers-colluding: unlinkable across contexts
- **Construction 002** — against verifier: linkable within a context by design (declared, governed)
- **Construction 003** — against verifier, verifiers-colluding: changing the constrained transcript scalar invalidates the proof; audience/time replay protection additionally assumes the verifier checks the authenticated request and its validity window
- **Construction 004** — against verifier, issuer-verifier-colluding: long-term key hidden
- **Construction 005** — against verifier, verifiers-colluding: identities of both hidden
- **Construction 006** — against registry-operator, issuer-verifier-colluding: the status check does not identify the holder — requires bulk/anonymous root fetch, never a per-holder query
- **Construction 007** — against verifier, verifiers-colluding: no cross-presentation handle: the proof is transcript-bound and emits no identifier-derived value; two verifiers comparing proofs learn only what the enclosing records disclosed to each
- **Construction 007** — against issuer-verifier-colluding: the secret s is never revealed and no per-identifier salt is; an issuer who minted one identifier's credential learns nothing about the other from the proof
- **Construction 008** — against verifier: route 1 intends to hide a low-entropy taskContext from a verifier without the opening, assuming an independent uniformly random 128-bit secret blinding value and the commitment hash assumptions; a public or disclosed opening does not provide this protection
- **Construction 008** — against verifiers-colluding: verifiers colluding across contexts can link any repeated visible C in route 1. Route 2 cross-context unlinkability is a design objective, not established by this record; it depends on PRF key secrecy, domain separation and the absence of other stable presentation identifiers
- **Construction 008** — against issuer-verifier-colluding: the issuer that placed C and a verifier together can link C to the exchange (the issuer knows u) — stated, not hidden: issuer–verifier collusion is outside this record's protection
- **Construction 009** — against verifier, verifiers-colluding: the compared value is not disclosed and no value derived from it is emitted; two verifiers comparing proofs learn only what the enclosing records disclosed to each
- **Construction 009** — against issuer-verifier-colluding: the issuer of either credential learns nothing about the other credential from the proof — the equality is proven over openings the holder supplies, never over a value the issuer can recognise in a public signal
- **Construction 010** — against verifier, verifiers-colluding: P1/P2 — no pairwise-scope identifier of the edge, no counterparty identifier
- **Construction 010** — against verifiers-colluding: P4 — proposed cross-context proof unlinkability against colluding verifiers, conditional on the selected proof system and absence of correlatable disclosures; context nullifiers intentionally link reuse and registry/context metadata can also correlate presentations
- **Construction 010** — against registry-operator, issuer-verifier-colluding: C3 — currency check does not identify the presenter; holds only if rl_root/root_C are fetched without a per-holder query
- **Construction 011** — against verifier, verifiers-colluding: pairwise identifiers hidden; no cross-presentation correlator minted by the linkage itself
- **Construction 012** — against verifier, verifiers-colluding: no identifier beyond the disclosed set, and no cross-presentation handle: two verifiers shown different subsets cannot join them through this proof
- **Construction 012** — against issuer-verifier-colluding, registry-operator: the issuer of any one credential in the show learns nothing about the others from the proof; the revocation-state fetch must not be a per-holder query (record 006 C3)
- **Construction 013** — against verifier: the counterparty learns that this party's half has a form its policy accepts, not the half, its subject identifier or the membership behind it — holds while C is hiding (the salt u) and while the policy accepts more than one form; a policy that accepts exactly one form turns yes into the form
- **Construction 013** — against verifier: a party that answers first cannot be met by a counterparty that re-commits to a different half after seeing the answer — commitments are frozen once an answer exists (the formal model's commit_frozen)
- **Construction 020** — against verifier, verifiers-colluding: principal hidden under the selected proof assumptions and declared disclosure, against the verifier and colluding verifiers; hiding chain length additionally requires validated padding/fixed shape and metadata analysis, which are not established here
- **Construction 021** — against verifier, verifiers-colluding: no ancestor identifier is disclosed — the verifier learns the leaf's authority, not who equipped the presenter or through whom; against colluding verifiers the chain contributes no cross-presentation handle beyond what the presenter discloses of itself
- **Construction 021** — against registry-operator: the status check on links that carry `credentialStatus` does not identify the presenter or the chain when rl_root is fetched without a per-chain query; the root-status timing leak of item 14 is stated, not hidden
- **Construction 021** — against issuer-verifier-colluding: an issuer of one link learns from the proof nothing about the links below it — attenuations it never saw stay unseen, as the credential specification intends ('a governing party withdraws derivations it never saw')
- **Construction 022** — against verifier: a verifier shown the reference but not the referenced credential learns nothing about that credential's content that it could not have guessed without the reference — the salt removes the enumeration oracle
- **Construction 022** — against verifiers-colluding: route 2 only: two verifiers comparing what they were shown cannot link two presentations of the referencing credential through the reference, because neither saw a reference value
- **Construction 023** — against verifier: the issuing community learns nothing about which members vouched beyond the count reaching k — no voucher identifier, VRC-side or VMC-side, no path position, no leaf
- **Construction 023** — against verifiers-colluding: two join requests by one applicant to two communities, or a repeated request to one, cannot be linked through this proof beyond the disclosed admission identifier — the proof emits no identifier-derived value; the admission identifier is disclosed by the applicant's own act
- **Construction 023** — against registry-operator, issuer-verifier-colluding: fetching root_C and rl_root does not identify the applicant or the vouchers — holds only if roots are fetched without a per-applicant query
- **Construction 024** — against verifier, issuer-verifier-colluding: the community learns which of its eligible vetters attested no better than chance over the vetter set, less whatever the disclosed per-statement facts narrow that set
- **Construction 024** — against verifier: one vetter's attestations for two applicants cannot be linked through their tags or serials

### Negative space as recorded, per construction

*Generated from the `doesNotEstablish` field of every construction record (first three items each; the full list is in the record).*

- **Construction 001** does not establish: that the community's admission decision was correct (assurance boundary); that the leaf is current (see record 006); which member the holder is; …
- **Construction 002** does not establish: one natural person globally; one enrolment per issuer or ecosystem (second point of the trade curve — governance, not cryptography); cross-context uniqueness
- **Construction 003** does not establish: freshness beyond what the challenge carries; that the verifier's challenge was itself honest; correct JCS/SHA-256 evaluation inside the circuit merely because a supplied scalar is constrained; …
- **Construction 004** does not establish: non-transfer of the secret; absence of coercion or account sharing; agent authority or consent; …
- **Construction 005** does not establish: that the two parties are independent in the accreditation sense (declared, not proven — X8); that either is honest; distinct natural persons or independent key controllers merely from distinct leaves, keys or issuer identifiers
- **Construction 006** does not establish: that revocation is instantaneous — only that the handle was not revoked as of `epoch` (C4's published bound); that the registry's revocation decision was correct; that the verifier performed no live lookup — the construction makes the presentation self-carrying (public root + ZK proof; witness remains private); whether a deployment still phones home is a profile statement, not a proof property
- **Construction 007** does not establish: that the controller is one natural person — two agents or two people sharing a secret satisfy the clause (that is record 002's uniqueness, under its own declaration); that either credential is currently valid or unrevoked (record 006); that the holder intended the two identifiers to be correlated beyond this verifier — the proof is a disclosure to the party it is made to, not a widening of either identifier's declared scope; …
- **Construction 008** does not establish: unlinkability of presentations carrying the same visible commitment C; hiding plaintext alone does not prevent equality-based correlation; that the trust task completed, or what was done in it — completion evidence is a framework artifact outside any credential (the artifact gap, cred-tf #39/#40); that the binder's plaintext is not held elsewhere — the framework holds it in the Trust Task documents; this record blinds only the copy the credential carries; …
- **Construction 009** does not establish: common control: equal identifiers in two credentials say that the same DID appears in both, not that the presenter controls it — key control is record 004, and two different identifiers under one hand is record 007; that either credential is authentic, valid or unrevoked on its own — the enclosing record's signature-verify (or set-membership) and non-revocation clauses establish that; this clause compares two witnesses those clauses have already bound; that the two credentials were meant to be presented together — intentional correlation is the holder's declaration (record 012), and a shared subject widens no identifier's declared scope; …
- **Construction 010** does not establish: that the voucher endorses this request — a VRC is standing, not per-request; S5 binds the proof, not the relationship; that the presenter is one natural person (that is PR-UNQ in a different context, record 002 under its own declaration); that C's admission decision for either member was correct (assurance boundary — accreditation carries assurance); …
- **Construction 011** does not establish: any community-level assurance (that is record 010); that the personas are distinct natural persons; the relationship's content beyond what the statement discloses; …
- **Construction 012** does not establish: that the presenter is one natural person (k credentials, one secret: an agent holding a person's secret satisfies every clause — record 002 under its own declaration establishes uniqueness); anything about credentials not in the show: intentional correlation is declared per presentation and does not widen any identifier's declared scope; that the communities involved consented to be named together — the disclosure is the holder's; …
- **Construction 013** does not establish: that the edge will be accepted once published — admissibility under a policy commitment is not acceptance by a community's verifier; privacy of either policy — proving a form is in the counterparty's accepted set needs that set's path, so the policies are published (the request's step one); hiding a policy from prospective counterparties is not addressed; fairness — after both answers are yes, nothing compels a party to reveal its half; a party can learn the yes and walk away (the formal model's no_fairness); …
- **Construction 020** does not establish: that the principal authorised this specific act (grant ≠ invocation — the invocation is a trust-task artifact); the principal's identity; that the agent is not also acting for others; …
- **Construction 021** does not establish: that the presenter is someone the scope will deal with — a valid chain establishes narrowing by parties entitled to narrow, not that the leaf subject independently qualifies; that is the governing party's policy call (§Attenuation) and clause 7 is present only where the policy asks for it; delegation: the presenter acts as itself, and nothing here appoints it to act in anyone's name (§Authority is not delegation — that is record 020); that the governing party's own permission to govern S is current beyond 'accredited under root_G at the stated state'; …
- **Construction 022** does not establish: that the referenced credential is currently valid, unrevoked or accepted (record 006; the enclosing record's own clauses); that the party issuing the referencing credential was entitled to reference that credential — an acknowledgement by a non-member, an acceptance by the wrong delegate, a witness with no standing: governance and the enclosing record decide that, not the opening; unlinkability of presentations that show the same salted digest (route 1): hiding the plaintext behind a salt stops enumeration and nothing else; a stable visible reference still links every presentation of the referencing credential, exactly as record 008 says of a visible commitment C; …
- **Construction 023** does not establish: that any voucher consented to be counted toward this admission — a relationship credential is evidence of a relationship, and whether it is a vouch is the community's reading under its rules (general #31: 'vouching is social, not technical'); that the applicant is not already a member — a member can hold k VRCs; if the policy needs 'not a member', the statement gains a non-membership clause against root_C (record 006's gadget applied to the membership set), which this record does not include; that no voucher is the applicant — the statement has no clause excluding it, and an applicant who is already a member can vouch for itself through its own grant, so one of the k counted members is the applicant (the formal model's counter-deployment `self_vouch_counted`); the refusal is the community's admission check, which must refuse an applicant who is already a member (record 024 carries the key-level exclusion as its clause 7); …
- **Construction 024** does not establish: that the k attesters are k distinct people — distinct tags mean distinct vetter secrets; a vetter who hands its secret to another lets one person attest under two tags; that a transferable-token budget limits each vetter: vetters can pool tokens. A per-vetter quota requires a profile that binds authorized spends to a stable enrolled secret and bounds slots, or another construction demonstrating equivalent enforcement. Neither profile bounds distinct people or prevents sharing a secret (DG-024-4); that no attester is the applicant, in the Groth16 route — clause 7 is not built there; it holds only while applicants are outside the vetter set; …
<!-- generated-section:privacy:end -->

## Governance Considerations

This section is informative.

1. **Which half a membership proof reaches.** A membership is a mutually issued pair; a community-anchored proof establishes the community-issued grant half only. Communities whose governance requires member acknowledgement for a membership to count MUST say so, and verifiers MUST NOT read the grant half as the pair.
2. **Assurance is traceable to governance.** Whatever assurance a proof carries — personhood, accreditation, an assurance class — is inherited from the governance of the community that issued the membership credentials (ADR-001 G2). A construction record does not establish that a community's admission decision was correct.
3. **Declared scope and community disclosure.** A member's `pairwise` declaration constrains the member's own disclosure. A community that publishes a directory or presents member-issued credentials widens the exposure of that identifier; the Credentials Core Specification requires a community's governance framework to state its disclosure practice, and constructions over such identifiers inherit that statement.
4. **Publication of set roots.** A community or registry that publishes membership and revocation roots for proofs to rely on takes on an obligation of currency: the delay between a change and proofs reflecting it MUST be bounded and published (ADR-001 C4). This specification takes the bound as a horizon input and does not set it.
5. **Recommendations are evidence, not endorsement.** A proving system is RECOMMENDED here only when independent parties have reproduced a construction on it across architectures; the recommendation names the version and drops when the version changes. It is not an endorsement of a vendor.

## Internationalization Considerations

This section is informative.

Construction records, fixture families and rejection codes are identified by ASCII identifiers that are not localised. Human-readable statements in records are written in English and MAY be translated; the machine-readable record is authoritative. Canonical transcripts are digested over a canonical encoding chosen by the selected profile so that string ordering and Unicode normalisation do not vary by locale; [RFC8785] (JCS) is the proposed canonicalisation and the lab serializer approximates it, but full conformance is not established by the current fixtures and the decision is open under WG-06a. Whatever the profile selects, implementations canonicalise before digesting.

## Accessibility Considerations

This section is informative.

The presentation of a zero-knowledge proof imposes a proving-time and memory cost on the holder's device. Construction options record proving time on representative consumer devices, including low-memory devices, so that profiles can be chosen that do not exclude holders with older or constrained hardware. Mediated proving, where a holder delegates proving to an agent, is addressed by the requirements document's agent-mediated profile and by the mediator instrument of the evidence repository; it MUST NOT be the only path available to a holder.

## Conformance

This section is normative.

Conformance to this specification is claimed per **conformance target** and is demonstrated by **conformance tests** that are published as data — machine-readable records, fixtures and registry rows — rather than as prose. The tests below are the ones this Working Draft carries; they originate in the task force's evidence repository and are maintained in this repository's `conformance/` directory under Apache-2.0.

### Conformance Targets

1. **Construction record.** A construction record conforms when it validates against the construction-record schema and rules (§Conformance Tests, test 1). A record that does not validate MUST NOT appear in this specification and MUST NOT be cited as a DTG construction.
2. **Constructor.** A [[ref: constructor]] — a party that writes a construction record or a runtime for it — conforms when every clause of the record's relation is bound to a named gadget, every construction option states whether its cost is measured or conjectured and names its source, and every measured claim is backed by a runtime transcript or a verification-registry row. A constructor MUST NOT vet their own construction.
3. **Prover.** A prover implementation of a construction record conforms when, for every fixture family the record names, it produces the outcome the fixture expects — a valid proof for `accepts`, no proof (an unsatisfiable witness) for `rejects-unsat`, and the defined observable checks for the `unlinkable` fixture family, where declared (finite fixtures do not prove general unlinkability) — and when every proof it produces is bound to the canonical transcript digest of the presentation.
4. **Verifier.** A verifier implementation conforms when, for every fixture, it reaches the expected outcome **and emits the expected rejection-reason code** from the versioned register; when it rejects a proof presented against a different transcript; when it rejects a proof made against a registry state it does not accept; and when its outputs contain none of the prohibited claim patterns (test 3). A verifier MUST NOT infer from a proof anything the record's disclosure set does not contain.
5. **Issuer and registry.** An issuer conforms to a construction record when it satisfies the record's issuance requirements — for example, a ZK-openable commitment for each identifier that may be co-proven, where the record requires one. Those requirements are profiled by this specification (Integration, *Identifier commitment profile*), not by the credential specification's Conformance section: the credential specification's schema is unchanged unless the credential layer chooses to give a requirement a member of its own, and an issuer's conformance to that specification is neither claimed nor extended here. A registry conforms when it publishes the set roots the record names at stated registry states, with a published bound on the delay between a change and the roots reflecting it.
6. **Proving-system entry.** A proving-system entry conforms when it states its licence, setup, provenance and an audit statement, sources every published figure, and — to rise above the self-described rung — ships the pinned manifest and re-derivation script the verification registry needs to reproduce a construction on it.

### Conformance Tests

The following tests are the evidence a conformance claim rests on. Each is runnable; the first two run in this repository's continuous integration on every change.

1. **Record validation** (`conformance/validate.mjs`). Every construction record, request and proving-system entry is checked against its schema and against the rules that encode the task force's drafting rules. Refusals are register strings, never prose: `record-no-adversary`, `record-no-horizon`, `record-no-does-not-establish`, `record-clause-unbound`, `record-composed-disclosure-is-union` (a composed record whose disclosure set is the union of its parts), `record-composed-no-single-transcript`, `construct-no-measurement` (a record at state `constructed` with no measured option), `run-vectors-missing`, `vet-no-registry-row`, `history-not-monotone`, `request-construction-missing`, `stack-no-audit-statement`, `stack-benchmark-no-source`. The full list is in the validator. A record MUST validate before it is rendered into this specification.
2. **Generated text is current** (`conformance/test.mjs`). The rendered Construction Records, Requests Answered, Proving Systems and derived Privacy Considerations sections carry a digest of the records they were generated from. The test recomputes that digest, regenerates every marked section and every generated term using `conformance/generate.mjs` and `render-lib.mjs`, and compares their actual contents. Missing or duplicate boundaries, edited generated prose and missing or obsolete generated terms fail the check. Editor-written sections remain outside this comparison. The check establishes consistency with the renderer and records, not cryptographic correctness.
3. **Conformance fixtures** (evidence repository, `runtimes/fixtures/`; fixture schema `x1-fixtures/v0`). A fixture is a canonically encoded input — a context descriptor and a canonical transcript, never an opaque label — an expected outcome, and a named reason from a versioned **rejection-reason register** (v2: 66 exact codes and 29 parameterised families, append-only). Vectors come in three classes: **accept**, **reject**, and **lint**, the last testing that a verifier's output makes no claim the record does not license. The current suite is 39 vectors across nine predicate families with a manifest naming the register version and the vector count. A consumer harness re-derives every digest and re-runs the constructions, demanding the same outcome and the same reason code; a second consumer with no shared code, written in another language, has consumed all vectors with matching outcomes and byte-identical reason codes and re-derived every embedded canonical digest. Two implementations that pass the same suite have instantiated the same decisions, independently of language or hash.
4. **Independent reproduction** (the [[ref: verification registry]]; acceptance flow gates A–G). A [[ref: runner]] — a party other than the constructor — rebuilds a construction from published source on independent hardware, runs its suites, and files the pinned report sections. The acceptance checker re-derives a manifest-comparison verdict from submitted report fields. That verdict alone does not establish that the submitter executed the reported build or tests. Required compiled-artifact digests and constraint metadata match the selected manifest; setup-chain artifacts, including local verification keys, follow the manifest’s advisory policy because local contributions introduce randomness. A production proof is verified against its selected authorized verification key; advisory build-report handling does not permit arbitrary key substitution. Independent execution and provenance require their own evidence. Admission of a submitting party and publication of a row are the [[ref: maintainer]]'s acts and are never delegated to tooling; the acceptance flow itself is the [[ref: registry verifier]]. A [[ref: requester]] who asked for a construction takes no part in vetting it.
5. **The reproduction ladder.** A construction option's reproduction state is derived from registry rows, never asserted: `self-described` → `lab-measured` → `reproduced-once` (one row, party ≠ constructor) → `reproduced-cross-arch` (rows on two or more OS/architecture pairs) → `reproduced-multi-seat` (two or more parties, two or more architectures, fixtures green under the record's name). This specification uses RECOMMENDED of a proving system for a construction only at the top rung, names the version the rows were made against, and withdraws the word when the version changes until a new row lands.

### What conformance does not establish

Reproduction and behaviour are not review. A construction at the top of the ladder has been shown to build identically elsewhere and to behave as its fixtures require; it has not been shown free of under-constrained signals or of interface drift between an audited commit and a shipped one. Audit is a separate claim with its own evidence — a reviewed commit and file set — and is recorded as such in proving-system entries.

## References

This section is informative.

### Normative References

- **[DTG-CRED]** DTG Credentials Core Specification, Version 1.0, Document Status **Working Draft 0.4.0** — the minimum compatible Document Status for this draft (see Introduction, *Relationship to other specifications*). Read at `main` commit `994a3d63fe27d77ca6023f5a4aae8013272a6646` (2026-09-15: semantic versioning adopted, the VSC merged, the VDC, VAC and correlation-scope text). Sections are cited by title; the glossary is cross-referenced as the external specification `DTG_CRED`. The exact revision an implementation profile pins remains a profile decision. Trust over IP Foundation. <https://trustoverip.github.io/dtgwg-cred-spec/> · source: <https://github.com/trustoverip/dtgwg-cred-spec>
- **[DTG-ZKP-RULES]** Drafting rules of the DTG ZKP Task Force. <https://github.com/trustoverip/dtgwg-zkp-tf/blob/main/DRAFTING-RULES.md>
- **[RFC2119]** S. Bradner, "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997. <https://datatracker.ietf.org/doc/html/rfc2119>
- **[RFC8785]** A. Rundgren, B. Jordan, S. Erdtman, "JSON Canonicalization Scheme (JCS)", RFC 8785, June 2020. <https://datatracker.ietf.org/doc/html/rfc8785>
- **[VC-DI]** Verifiable Credential Data Integrity 1.0, W3C Recommendation — §2.6 (`digestMultibase`). <https://www.w3.org/TR/vc-data-integrity/>
- **[VC-DM]** Verifiable Credentials Data Model v2.0, W3C Recommendation. <https://www.w3.org/TR/vc-data-model-2.0/>

### Informative References

- **[DTG-ZKP-REQ]** Privacy-Preserving Proof of Liveness — Requirements, v0.4 (Proposed Task Force Working Draft). DTG ZKP Task Force. <https://github.com/trustoverip/dtgwg-zkp-tf/blob/main/proof-of-liveness-requirements.md> Source for the personhood/liveness use-case family and explicitly reused concepts; not a universal requirement for every DTG construction.

- **[AGENTPRIVACY]** M. Travers (privacymage), *agentprivacy* — the body of work from which this specification's Cryptographic Background is drawn and its root of understanding: the Zero Knowledge Spellbook (thirty expository chapters on zero-knowledge proof systems and their frontier addenda, 2026), the privacymage grimoire and explanatory material on privacy-preserving delegation. <https://agentprivacy.ai> · <https://github.com/mitchuski>
- **[ADR-001]** G. Gore, "Community-Anchored Proof — Choosing the first zero-knowledge use case to implement against DTG credentials", ADR-001, Proposed 2026-08-25. <https://docs.fpp.storm.ws/>
- **[PoP-2026]** A. R. Choudhuri, S. Garg, K. Lee, H. Montgomery, G. V. Policharla, R. Sinha, "A Cryptographic Framework for Proof of Personhood", IACR ePrint 2026/333. <https://eprint.iacr.org/2026/333>
- **[Groth16]** J. Groth, "On the Size of Pairing-based Non-interactive Arguments", EUROCRYPT 2016.
- **[Semaphore]** Semaphore protocol and zk-kit (Privacy & Scaling Explorations). <https://semaphore.pse.dev/> · <https://github.com/zk-kit>
- **[Flock]** R. Rothblum, B. Bünz, W. Wang, "Flock" — a SNARK for batches of Boolean computation (Succinct / Espresso Systems, June 2026). <https://blog.succinct.xyz/introducing-flock/> · <https://github.com/succinctlabs/flock>
- **[ProveKit]** ProveKit — client-side zero-knowledge proving toolkit (World Foundation, Atheon, Reilabs, Nethermind). <https://provekit.org/> · <https://github.com/worldfnd/provekit>
- **[SIROS-CAT]** SIROS Foundation ZK Circuit Catalog. <https://circuits.siros.org/> · <https://github.com/sirosfoundation/go-zk-circuits>
- **[Longfellow]** Google Longfellow ZK (libzk). <https://github.com/google/longfellow-zk>
- **[Circomspect]** Trail of Bits, Circomspect — static analyzer for Circom. <https://github.com/trailofbits/circomspect>
- **[DTG-ZKP-EVIDENCE]** DTG ZKP evidence repository — reference runtimes, conformance fixtures, verification registry, construction records. Apache-2.0 code, CC BY 4.0 documents. <https://github.com/mitchuski/dtgwg-zkp-mage> · the commit this revision was exported from is named in Appendix B.
- **[DTG-CRED-TF-39]** "Privacy: Appropriately supporting unlinkability, ZKP and selective disclosure", dtgwg-cred-tf discussion #39. <https://github.com/trustoverip/dtgwg-cred-tf/discussions/39>
- **[DTG-CRED-TF-40]** "Delegation as a case study in the design-time window", dtgwg-cred-tf discussion #40. <https://github.com/trustoverip/dtgwg-cred-tf/discussions/40>
- **[DTG-CRED-31]** "WD02 merge plan: sequencing the five outstanding PRs into a consistent whole", dtgwg-cred-spec issue #31 (closed 2026-09-10). <https://github.com/trustoverip/dtgwg-cred-spec/issues/31>
- **[DTG-CRED-9]** "Define the identity linkages required by the ZKP constructions", dtgwg-cred-spec issue #9 — the common-control requirement, the subject-or-issuer widening and the chain-predicate distinction (2026-09-10). <https://github.com/trustoverip/dtgwg-cred-spec/issues/9>
- **[DTG-CRED-38]** "Digest-valued binders are unsalted and enumerable; blinding is deferred from WD02", dtgwg-cred-spec issue #38. <https://github.com/trustoverip/dtgwg-cred-spec/issues/38>
- **[DTG-CRED-42]** "docs: say plainly what the ZK predicates are waiting on", dtgwg-cred-spec PR #42 (merged 2026-09-10) — the editor's note in §Zero-Knowledge and Selective Disclosure. <https://github.com/trustoverip/dtgwg-cred-spec/pull/42>
- **[DTG-TT]** DTG Core Trust Task Protocols — the Trust Tasks framework specification (draft; the revision a profile pins is a profile decision). <https://github.com/trustoverip/dtgwg-trust-tasks-tf/blob/main/SPEC.md>
- **[OPENVTC]** OpenVTC verifiable trust infrastructure — a candidate integration target, revision to be pinned. <https://github.com/OpenVTC/verifiable-trust-infrastructure>
- **[AKITA]** Lattice Jolt / Akita — a Module-SIS-based polynomial commitment scheme announced 2026-09-09 as a post-quantum proving route; research software, zero knowledge in a forthcoming companion paper. <https://a16zcrypto.com/posts/article/lattice-snarks-jolt-post-quantum-faster> · <https://github.com/LayerZero-Labs/akita>
- **[LONGFELLOW-PQCA]** Google, "Donating the Longfellow ZKP library to the Post-Quantum Cryptography Alliance" (Linux Foundation Europe), 2026-09-02 — library stewardship and provenance, distinct from the SIROS catalogue's artefact provenance. <https://blog.google/products-and-platforms/platforms/google-pay/zero-knowledge-proof-library-linux-foundation/>
- **[DTG-ZKP-TF-CALL-2026-09-08]** DTG ZKP Task Force meeting notes, 2026-09-08 (ToIP Confluence) — ADR-001 confirmed as the first proof; the blind-signature vouch alternative; the same-context pseudonym caveat; the credential signature scheme as the non-swappable choice. <https://lf-toip.atlassian.net/wiki/spaces/HOME/pages/1132953601>
