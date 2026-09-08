<!-- generated-from: records-sha256=e97af739f33e9ce365926d3ea669462f1aa1033598c51fe755e2109b1ff1e738 — the Requests Answered, Construction Records, Proving Systems and derived Privacy Considerations sections are generated from conformance/{records,requests,stacks}; conformance/test.mjs compares this stamp, the marked generated sections and generated terms with fresh generation from these files. Edit the records, not the generated text. -->

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
| source | https://docs.fpp.storm.ws/ (ADR-001) · named on the board: zkp-tf #18 — ScottJeezey 2026-08-27: 'a natural first one to seed it with' |
| answered by | [Construction 010 · Community-Anchored Proof (ADR-001)](#construction-010-community-anchored-proof-adr-001) |

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
| P3 | privacy | reveals no identifier of the holder beyond attributes the holder deliberately discloses | 010 yield (deliberate disclosure line) | **covered** |  |
| P4 | privacy | two proofs from the same credentials cannot be linked — by one verifier or by verifiers comparing notes | 010 method (nullifier clause, parameterised) · adversary (verifiers-colluding) | **refined** | full show-unlinkability cannot coexist with reuse detection in the same context (ePrint 2026/333 §5.3); parameterise by context: unlinkable across contexts, the declared nullifier the only link within a reuse-detecting one |
| P5 | privacy | a verifier learns the outcome and deliberately disclosed attributes, nothing further | 010 yield | **covered** |  |
| S1 | soundness | a party lacking the required credentials cannot produce a verifying proof | 010 method 1–3 · conformance fixtures rejects-unsat / rejects-verify | **covered** |  |
| S2 | soundness | clause 3 cannot be satisfied unless the counterparty genuinely holds a membership credential from the same community | 010 method (set-membership on the voucher's grant leaf, card 001) | **refined** | proves the community-issued grant half only (cred-spec PR #12 pair; PR #26 carve-out) — the acknowledgement is not in the presenter's hands |
| S3 | soundness | the counterparty need not be online, consulted, or aware | 010 witness (proven from root_C, not from the voucher) | **refined** | under WD02 pairwise identifiers the voucher's linkage (VRC-side ↔ VMC-side identifier) must have been supplied at issuance or avoided by one directed identifier — card 007 / HR-2; otherwise clause 3 is unprovable offline |
| S4 | soundness | the presenter proves they are the subject of the credentials, not merely a holder of copies | 010 method (key-binding, card 004) | **covered** |  |
| S5 | soundness | bound to a verifier challenge; not replayable to another verifier or time | 010 method (transcript-bind, card 003) · public inputs transcriptDigest | **covered** |  |
| S6 | soundness | (added by the ZKP TF) the voucher is not the holder — a self-vouch is unsatisfiable | 010 method (distinctness, card 005) | **added** | without it a member with two identifiers vouches for themselves and clauses 1–3 verify |
| S7 | soundness | (added, WD02) the identifiers a party used in the VRC and in their VMC are controlled by one secret | 010 method (key-binding, card 007) | **added** | cred-spec PR #30 §Community-Anchored: 'the proof must additionally establish common control' |
| C1 | currency | does not verify if any relied-on credential is revoked or suspended | 010 method (non-revocation, card 006) | **covered** |  |
| C2 | currency | states the registry state it was made against; the verifier judges recency | 010 public inputs (root_C, rl_root, epoch) · yield | **covered** |  |
| C3 | currency | establishing currency must not itself identify the holder | 010 adversary (registry-operator · issuer-verifier-colluding) | **covered** | holds only if roots are fetched without a per-holder query — stated as the condition |
| C4 | currency | the delay between a change and proofs reflecting it is bounded and published | 010 horizon (status freshness, C4 bound) | **open** | the bound is a registry/governance publication, not a proof property — construction carries it as a horizon input |
| T1 | registry | a verifier confirms the community is one it recognises without learning which member | 010 public inputs (root_C at a stated registry state) | **covered** |  |
| T2 | registry | whatever the proof relies on from a registry is independently checkable by a party that did not create it | 006 set-root primitive route · public inputs set roots | **refined** | the set-root primitive (cred-tf #40): signed published root + zero-knowledge membership/status proof in the presentation; paths and openings stay private; root/witness retrieval requires an explicit privacy policy |
| T3 | registry | two verifiers checking against the same registry state reach the same verdict | 010 conformance fixtures (current family) · fixtures determinism | **covered** |  |
| T4 | registry | a registry's obligations are stated as an interface so any conformant registry can serve these proofs | — (Trust Registry TF) | **open** |  |
| D1 | deployment | proving is feasible on the devices that hold credentials — a phone or an agent — within a stated time and memory budget | 010 substitutions (Groth16 est. ≈35–45k constraints ~2 s; blackbox 0.03 s/vouch) · stacks layer (ProveKit phone numbers) | **partial** | budgets not yet stated by the TF; the options layer supplies measured numbers per stack for the gate to set them |
| D2 | deployment | verification is cheap enough to run inline | lab: ~8–10 ms verify (Groth16) | **covered** |  |
| D3 | deployment | a proof fits the transports DTG credentials already travel over | stacks layer: proof size column (721 B Groth16 vs ~716 KB WHIR) | **open** | a profile decision; the options table shows both numbers |
| X1 | conformance | published test vectors covering proofs that must verify and proofs that must fail | 010 conformance fixtures families · runtimes/fixtures format (accept/reject/lint vectors, rejection-reason register) | **partial** | format exists and is consumed cross-language; 010's own vector family not yet cut |
| X2 | conformance | an independent implementation can verify another's proofs | registry (independent reproduction) · consumer-py (zero-shared-code consumer) | **partial** | reproduced for cards 001/002/003/005; 010 composition not yet built |
| X3 | conformance | any requirement on how credentials are issued is stated explicitly and early | 010 issuance (ZK-friendly signature or published commitment; issuer linkage MAY) · cred-spec #17 | **covered** |  |
| G1 | governance | a community declares whether private presentation is required, default, or optional | — (Governance; cred-spec §Governance Considerations) | **open** |  |
| G2 | governance | the assurance a proof carries is traceable to the governance of the issuing community | 010 yield (assurance class via C's governance) · doesNotEstablish (admission correctness) | **covered** |  |

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

A presentation exposes the accepted root and a zero-knowledge proof of the relevant membership statement. Paths, openings and non-membership witnesses remain private proof inputs unless disclosure is explicitly declared. Self-contained evidence can avoid per-holder verifier lookups, but obtaining and refreshing roots and witnesses still requires a deployment policy describing its correlation surface.

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

Under the credentials specification's Working Draft 02, an identifier carries a holder-declared [correlation scope](https://github.com/trustoverip/dtgwg-cred-spec/pull/30) — `pairwise | directed | public`, monotonic — and roles come from credentials. The scope is a public input where a construction's disclosure depends on it: a `pairwise` identifier appears in a proof only behind a commitment; a `directed` persona identifier may be shown on purpose (construction 011); and whether a proof of common control is needed at all is decided by the declaration (constructions 007 and 012: no proof where one `directed` or `public` identifier was deliberately reused). Where a declaration is carried — credential or DID document — remains a profile dependency: the verifier needs an authenticated source for the declaration, and its encoding can change the construction inputs.

*Source: cred-spec #22, PR #30 §Correlation Scope and §Choosing a scope; cred-tf #41.*

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

This section is informative in this Working Draft: every record below is at state `carded` or `constructed`. Evidence maturity is printed at the head of each record. Normative adoption is a separate task-force decision; reproduction or publication alone does not confer it.

This section is generated from the machine-readable records in `conformance/records/`. Changes are made to a record, never to this text; a record that fails validation does not render. Each record states its adversary, its horizon and what it does not establish, and labels conjecture as conjecture, because the validator refuses records that do not.

### Index of constructions

**Primitive constructions** — one gadget each.

| # | construction | state | priority | gadget |
|---|---|---|---|---|
| [001](#construction-001-set-membership-over-an-accredited-root) | Set membership over an accredited root | `constructed` | P1 | set-membership |
| [002](#construction-002-scoped-nullifier-reuse-detection) | Scoped nullifier (reuse detection) | `constructed` | P1 | nullifier |
| [003](#construction-003-transcript-binding) | Transcript binding | `constructed` | P1 | transcript-bind |
| [004](#construction-004-holder-binding-key-from-secret) | Holder binding (key from secret) | `carded` | P2 | key-binding |
| [005](#construction-005-distinct-member-distinct-issuer) | Distinct member / distinct issuer | `constructed` | P1 | distinctness |
| [006](#construction-006-non-revocation-against-a-status-root) | Non-revocation against a status root | `carded` | P1 | non-revocation |
| [007](#construction-007-common-control-across-identifiers) | Common control across identifiers | `carded` | P1 | key-binding |
| [008](#construction-008-blinded-binder-taskcontext-hiding-presentation-correlation-unresolved) | Blinded binder (taskContext hiding; presentation correlation unresolved) | `carded` | P2 | commitment-open |

**Composed constructions** — a named conjunction under one transcript and one disclosure set.

| # | construction | state | priority | composes |
|---|---|---|---|---|
| [010](#construction-010-community-anchored-proof-adr-001) | Community-Anchored Proof (ADR-001) | `carded` | P1 | 001 ∧ 002 ∧ 003 ∧ 004 ∧ 005 ∧ 006 ∧ 007 |
| [011](#construction-011-pairwise-edge-vrc-possession-directed-personas-shown-pairwise-identifiers-hidden) | Pairwise edge (VRC possession, directed personas shown, pairwise identifiers hidden) | `carded` | P2 | 003 ∧ 004 ∧ 006 ∧ 007 |
| [012](#construction-012-intentional-correlation-one-controller-across-k-credentials) | Intentional correlation — one controller across k credentials | `carded` | P2 | 003 ∧ 006 ∧ 007 |
| [020](#construction-020-delegation-chain-vdc-agent-acts-for-a-member) | Delegation chain (VDC) — agent acts for a member | `carded` | P2 | 001 ∧ 003 ∧ 004 ∧ 006 |

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

#### Method

1. the committed leaf is in the tree at `root` — [[ref: set-membership]] · runtime `runtimes/circom-gadget/circuits/nullifier_membership.circom`

#### Disclosure set

- root
- context

#### Does not establish

- that the community's admission decision was correct (assurance boundary)
- that the leaf is current (see card 006)
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

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| Groth16 / BN254 / Poseidon Merkle (lab) | 11,522 constraints · ~680 ms prove · ~8 ms verify · 721 B (with card 003 bound) | **measured** | CIRCUITS.md · registry 0002–0006 |
| KZG / accumulator membership (paper §3.7 hiding KZG) | constant-size opening; pairing verify | unmeasured | ePrint 2026/333 §3.7, §8 |
| Semaphore v4 tree (structurally conformant, byte-incompatible) | see cross-check | unmeasured | explorations/SEMAPHORE-V4-CROSSCHECK.md |
| Flock-class binary-field prover over a standard-hash (BLAKE3 or SHA-256) Merkle tree — the path is a batch of compressions, Flock’s native workload; removes the Poseidon requirement on the registry side and gives a post-quantum path (LIV-ALG-07) | unmeasured — conjecture: depth-20 path ≈ 20–40 compressions ≈ well under a millisecond of prover work per the published 82,100 compressions/s single-core figure; proof size in the hundreds of kB class | unmeasured | board/stacks/flock.json (blog.succinct.xyz/introducing-flock) |

#### Issuance requirements

- issuer publishes a ZK-friendly commitment per member (Poseidon leaf) or an accumulator

#### Provenance

- cred-spec VMC
- liveness reqs v0.4 §13 (PR-UNQ membership leg)
- cred-tf #39 (ScottJeezey 2026-08-25): issuer-as-predicate named as a ZKP TF work item — this card
- registry: 0002–0006 (circuit reproduced, card-level run pending)
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `carded` | mitchuski | runtimes/01-uniqueness-nullifier NOTES + decision §13 |
| 2026-08-28 | `constructed` | mitchuski | circom-gadget nullifier_membership 10/10, numbers in CIRCUITS.md |


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

#### Method

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

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| Poseidon nullifier in the membership circuit (lab) | included in card 001's 11,523 | **measured** | CIRCUITS.md |
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
| 2026-08-28 | `carded` | mitchuski | decision §13 + O2 PHC-by-nullifier |
| 2026-08-28 | `constructed` | mitchuski | same circuit as 001; scoped preimage tested 9/9 in runtime 01 |


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

#### Method

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

#### Adversary, per claim

- **verifier · verifiers-colluding** — changing the constrained transcript scalar invalidates the proof; audience/time replay protection additionally assumes the verifier checks the authenticated request and its validity window

#### Horizon

- the transcript's own validity window

#### Conformance fixtures

Families: `accepts` · `rejects-verify`

Vectors: `runtimes/fixtures/vectors`

Rejection codes: `transcript-digest-mismatch`, `bare-nonce-insufficient`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
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
- registry: 0002–0006
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `carded` | mitchuski | X2 context legibility + canonical runtime 11/11 |
| 2026-08-28 | `constructed` | mitchuski | +1 constraint measured, CIRCUITS.md |


### Construction 004 · Holder binding (key from secret)

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `carded` |
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

#### Method

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

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
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
| 2026-08-28 | `carded` | mitchuski | runtimes/04-holder-binding STUB.md + paper §7.2 languages |


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

#### Method

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

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
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
| 2026-08-28 | `carded` | mitchuski | X8 + dual_issuer design |
| 2026-08-28 | `constructed` | mitchuski | dual_issuer 7/7, guardian 8/8; duplicate = no witness |


### Construction 006 · Non-revocation against a status root

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `carded` |
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

#### Method

1. non-membership of the handle in the set committed by rl_root (sorted-leaf neighbours or accumulator non-witness) — [[ref: non-revocation]]

#### Disclosure set

- rl_root
- epoch

#### Does not establish

- that revocation is instantaneous — only that the handle was not revoked as of `epoch` (C4's published bound)
- that the registry's revocation decision was correct
- that the verifier performed no live lookup — the card makes the presentation self-carrying (public root + ZK proof; witness remains private); whether a deployment still phones home is a profile statement, not a proof property

#### Adversary, per claim

- **registry-operator · issuer-verifier-colluding** — the status check does not identify the holder — requires bulk/anonymous root fetch, never a per-holder query

#### Horizon

- status freshness (C4 bound)
- epoch rollover

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `current`

Rejection codes: `rl-root-stale`, `handle-revoked`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| sorted-leaf non-membership Merkle (indexed tree) | ≈ 2× card 001 | unmeasured | explorations/O4-registry-zk-revocation.md |
| RL membership check inside f with nullifier as PHC attribute (paper §5.4) — carries the paper's own linkability caveat | depends on RL representation | unmeasured | ePrint 2026/333 §5.4 |
| set-root primitive (cred-tf #40 unification): a signed, published set root + a membership or non-membership proof carried in the presentation — accumulator non-membership witness as a private proof input; the same public-input object serves anchoring (card 001), revocation status (this card) and registry membership | unmeasured — ScottJeezey: "ours to pressure-test", priority | unmeasured | cred-tf #40 (stormer78 08-22; ScottJeezey 08-24) · cred-tf #39 (ScottJeezey 08-25) |
| Flock-class prover over an indexed (sorted-leaf) non-membership tree built with the registry’s existing standard hash — the set-root primitive without a hash migration | unmeasured — conjecture ≈ 2× the standard-hash membership path | unmeasured | board/stacks/flock.json · cred-tf #40 (set-root primitive) |

#### Issuance requirements

- registry publishes rl_root per epoch, fetchable without identifying the fetcher (X4)

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
| 2026-08-28 | `carded` | mitchuski | O4 + X6 explorations; ADR-001 C-clauses |


### Construction 007 · Common control across identifiers

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `carded` |
| priority | P1 |
| constructor | mitchuski |
| requested by | sankarshanmukhopadhyay / geoffturk / stormer78 (spec side) — carded by the ZKP TF co-chair |
| request | cred-spec #9 (Sankarshan: identity linkages the ZKP constructions require) · cred-spec #31 (geoffturk 09-02: '#9 stays open — and gains weight'; four things lean on the unencoded common-control linkage) · cred-spec PR #30 §Community-Anchored ZKP ('the proof must establish common control across them') |

**Kind:** [[ref: primitive construction]] — binds the [[ref: key-binding]] gadget and nothing else.

#### Statement

A verifier checks common control through the selected shared-secret derivation relation, while the secret remains private under the construction assumptions. Any cross-presentation correlation depends on the enclosing disclosure set and context policy.

**Need.** under WD02's three correlation scopes, two `pairwise` identifiers differ by construction, so any proof that reads one party out of two credentials must first prove one holder controls both identifiers — without a field that says so

#### Witness

*Never leaves the holder.*

- the holder secret s
- per-identifier derivation material: for each identifier, the salt or key-derivation path under which it was minted from s
- the two credentials that name the identifiers (their bytes stay with the holder; only what the enclosing card discloses is shown)

#### Public inputs

- the two identifiers exactly as the credentials carry them — or their ZK-openable commitments, when the identifiers themselves are hidden by the enclosing card
- transcriptDigest — the presentation transcript this proof is bound to

#### Method

1. identifier A's public key or commitment opens to (s, salt_A) and identifier B's opens to (s, salt_B) for one and the same s — a different secret behind either identifier is unsatisfiable — [[ref: key-binding]]

#### Disclosure set

- the outcome (one controller / not shown)
- transcriptDigest
- the identifiers only as far as the enclosing card already discloses them — this card adds no identifier to the disclosure set

#### Does not establish

- that the controller is one natural person — two agents or two people sharing a secret satisfy the clause (that is card 002's uniqueness, under its own declaration)
- that either credential is currently valid or unrevoked (card 006)
- that the holder intended the two identifiers to be correlated beyond this verifier — the proof is a disclosure to the party it is made to, not a widening of either identifier's declared scope
- the counterparty's common control: a presenter can prove only what is derived from a secret in the presenter's hands; a counterparty's linkage needs the counterparty's witness or the counterparty's own attestation (see card 010)
- that arbitrary independently generated or hardware-protected keys derive from a shared available scalar; derivation and custody are profile requirements

#### Adversary, per claim

- **verifier · verifiers-colluding** — no cross-presentation handle: the proof is transcript-bound and emits no identifier-derived value; two verifiers comparing proofs learn only what the enclosing cards disclosed to each
- **issuer-verifier-colluding** — the secret s is never revealed and no per-identifier salt is; an issuer who minted one identifier's credential learns nothing about the other from the proof

#### Horizon

- the shorter of the two identifiers' key-validity periods — after a rotation the old key no longer opens to s under the recorded path and the clause must be re-proven against the rotated material
- the hash/commitment cryptoperiod of the derivation

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable`

Rejection codes: `co-control-unproven (unsat: distinct secrets)`, `identifier-not-zk-openable (verify-fail: identifier carries no openable commitment — an issuance failure, X3)`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| no proof — the holder declares one `directed` identifier and uses it in both credentials (WD02 §Choosing a scope: correlation evident on the face of the credentials) | zero constraints; the cost is the declaration itself | unmeasured | cred-spec PR #30 §Choosing a scope / §Community-Anchored ZKP |
| Groth16/Poseidon: two Poseidon commitment openings sharing the secret input (the circom-gadget leaf commitment, twice) — conjecture ~500–600 constraints total (~65%), unmeasured; one compile settles it | unmeasured (conjecture ≈ 2 × the lab's Poseidon leaf commitment) | unmeasured | runtimes/circom-gadget (Poseidon commitment + nullifier already bind a leaf to a secret) |
| blackbox commit-and-prove: the same opening under the paper's hiding commitments (Construction II show, N=1) | paper Table 1 class | unmeasured | ePrint 2026/333 §7.2 |

#### Issuance requirements

- each identifier that may need to be proven co-controlled must be, or carry, a ZK-openable commitment to the holder secret: a SNARK-native key (e.g. BabyJubJub did:key) or a published Poseidon/KZG commitment beside an Ed25519 key — the X3 requirement of card 010, now applied to identifiers rather than signatures (cred-spec #17)
- the credential layer carries the requirement to be able to prove co-control, never a field that states the link (cred-spec #9, the 08-25 position)

#### Provenance

- cred-spec PR #30 §Correlation Scope / §Choosing a scope / §Community-Anchored Zero-Knowledge Proof (WD02 draft, 2026-09-02)
- cred-spec #9 (identity linkages required by the ZKP constructions)
- cred-spec #31 disposition table (#9 stays open and gains weight; cross-TF work with the ZKP TF)
- ADR-001 S4 (holder binding) — generalised to two identifiers

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-09-02 | `requested` | geoffturk / stormer78 (cred-spec #31, PR #30) · sankarshanmukhopadhyay (cred-spec #9) | cred-spec #31 disposition row for #9: 'four things lean on the unencoded common-control linkage … the resolution is cross-TF work with the ZKP task force' |
| 2026-09-05 | `carded` | mitchuski | carded from PR #30's §Community-Anchored text + #9 + the lab's key-binding gadget shape; cost line labelled conjecture per drafting rule 4 |


### Construction 008 · Blinded binder (taskContext hiding; presentation correlation unresolved)

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | primitive |
| state | `carded` |
| priority | P2 |
| constructor | mitchuski (card) · ScottJeezey (named the work item) |
| requested by | ScottJeezey for the ZKP TF · bmiller59 (#39 postulate) · sankarshanmukhopadhyay |
| request | cred-tf #39 (ScottJeezey 08-25: 'a blinded, non-correlating form of the binder (taskContext), with salted commitments available now and PRF-derived per-context pseudonyms as the fuller construction') · cred-spec §Trust Task Context Binding · cred-spec PR #18 (parked) · cred-tf #40 (the artifact gap) |

**Kind:** [[ref: primitive construction]] — binds the [[ref: commitment-open]] gadget and nothing else.

#### Statement

A verifier that holds a trust-task context learns that the presented credential was issued within that exchange, while the credential at rest and every other presentation of it carry no plaintext binder that recognises the exchange or the holder.

**Need.** a credential bound to the trust-task exchange it was issued in currently carries the binder in the clear; the binder (id/threadId pairing) is then a durable correlator across every presentation of that credential

#### Witness

*Never leaves the holder.*

- the taskContext value (the id/threadId pairing the framework assigned to the exchange)
- the blinding salt u the issuer used when committing to it
- the credential carrying the commitment

#### Public inputs

- Route 1 currently treats commitment C as visible (digestMultibase-encoded). Repeated C values can correlate presentations; hiding C in a proof or a verifiable rerandomization route remains an unresolved design requirement.
- what the verifier already holds of the exchange: the taskContext digest it expects (route 1) or the context descriptor for the per-context pseudonym (route 2)
- transcriptDigest

#### Method

1. Route 1: prove that C opens to (taskContext, secret blinding value) for the expected exchange. Route 2 (proposed): the holder proves correct derivation of a context pseudonym from its secret key and context descriptor; the verifier checks that proof without learning or recomputing with the holder secret. Both routes still require binding to the issuer-authenticated credential. — [[ref: commitment-open]]

#### Disclosure set

- the outcome (bound to this exchange / not shown)
- transcriptDigest
- route 2 only: the per-context pseudonym, which is by construction the same value every time this holder presents in this context — a declared, context-scoped link and nothing wider
- route 1 as currently specified: visible C, which is stable for this credential and can correlate presentations

#### Does not establish

- unlinkability of presentations carrying the same visible commitment C; hiding plaintext alone does not prevent equality-based correlation
- that the trust task completed, or what was done in it — completion evidence is a framework artifact outside any credential (the artifact gap, cred-tf #39/#40)
- that the binder's plaintext is not held elsewhere — the framework holds it in the Trust Task documents; this card blinds only the copy the credential carries
- durable-versus-task-dependent status of the claim (Outcome Interpretability is the credential layer's statement, not this proof's)

#### Adversary, per claim

- **verifier** — route 1 intends to hide a low-entropy taskContext from a verifier without the opening, assuming an independent uniformly random 128-bit secret blinding value and the commitment hash assumptions; a public or disclosed opening does not provide this protection
- **verifiers-colluding** — verifiers colluding across contexts can link any repeated visible C in route 1. Route 2 cross-context unlinkability is a design objective, not established by this card; it depends on PRF key secrecy, domain separation and the absence of other stable presentation identifiers
- **issuer-verifier-colluding** — the issuer that placed C and a verifier together can link C to the exchange (the issuer knows u) — stated, not hidden: issuer–verifier collusion is outside this card's protection

#### Horizon

- route 1 plaintext hiding lasts only while the opening remains secret from the named verifier and the hash assumptions hold. Closing a thread does not erase retained openings or prevent correlation through a retained visible C.
- route 2: the context descriptor's epoch; the pseudonym rotates with it
- the commitment's hash cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable`

Rejection codes: `binder-mismatch (unsat: C does not open to the supplied taskContext)`, `binder-plaintext-present (lint: credential carries taskContext in the clear beside C)`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| route 1 — proposed salted-commitment opening using available primitives; no record-specific measured implementation | unmeasured — one Poseidon opening, conjecture ≈ 250–300 constraints (~70%) | unmeasured | runtimes/canonical + runtimes/circom-gadget |
| route 2 — PRF-derived per-context pseudonym (the card-002 nullifier construction with the context descriptor as domain) | the lab's domain-tagged nullifier: measured inside the 11,523-constraint gadget; standalone unmeasured | unmeasured | runtimes/circom-gadget (nullifier binds context; card 002) |

#### Issuance requirements

- issuers place the commitment C in `taskContext` (or beside it) instead of the plaintext pairing — a change to cred-spec §The `taskContext` Property, and the one member this card asks the credential layer for
- C is digestMultibase-encoded (WD02 D-A) so both layers agree on the encoding
- the framework (Trust Tasks) commits to the taskContext in a form the proof can open — 'we can only blind what the framework gives us a committed form of' (ScottJeezey, cred-tf #39)
- Specify generation, distribution and retention of the secret blinding value; do not publish it beside a low-entropy plaintext-hiding commitment.

#### Provenance

- cred-tf #39 (ScottJeezey 2026-08-25 — ZKP TF work items on the record)
- cred-tf #40 (the artifact gap; delegation as a design-time case)
- cred-spec §Trust Task Context Binding / §The `taskContext` Property (WD01)
- cred-spec #31 D-A (digestMultibase settled) · trustoverip/dtgwg-trust-tasks-tf#236 (§4.9.3)

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-25 | `requested` | ScottJeezey (ZKP TF co-chair, cred-tf #39) | cred-tf #39 comment 2026-08-25T14:41Z: 'we are treating these as work items: a blinded, non-correlating form of the binder (taskContext)…' |
| 2026-09-05 | `carded` | mitchuski | carded from Scott's two routes + cred-spec §Trust Task Context Binding + the lab's descriptor-digest and nullifier shapes; costs labelled conjecture |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-07 | reviewer (Codex; local editorial review) | Narrowed plaintext-hiding claims, made visible-C correlation explicit, corrected PRF verification and retention assumptions. Evidence state unchanged; design and implementation questions remain open. |


### Construction 010 · Community-Anchored Proof (ADR-001)

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `carded` |
| priority | P1 |
| constructor | mitchuski + DenisPopov15 (construction) · stormer78 (record) |
| requested by | stormer78 — ADR-001 Proposed 2026-08-25, docs.fpp.storm.ws |
| request | zkp-tf #18 (Scott 08-27: 'a natural first one to seed it with') |

**Composes:** [001](#construction-001-set-membership-over-an-accredited-root) ∧ [002](#construction-002-scoped-nullifier-reuse-detection) ∧ [003](#construction-003-transcript-binding) ∧ [004](#construction-004-holder-binding-key-from-secret) ∧ [005](#construction-005-distinct-member-distinct-issuer) ∧ [006](#construction-006-non-revocation-against-a-status-root) ∧ [007](#construction-007-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A maintainer checks authenticated evidence of a relationship between two distinct member credentials of community C, under the declared holder-linkage, validity and status assumptions. Hidden identifiers remain private against the verifier and colluding verifiers only under the stated construction assumptions, disclosure set and horizon; community roots, context, optional nullifier and other disclosed metadata remain visible.

**Need.** the first ZK use case against DTG credentials: a relationship exists inside a shared community, without revealing who is in it

#### Witness

*Never leaves the holder.*

- the VRC (the voucher → the presenter): the vouch, its statement, and the pairwise-scope identifier pair it was issued between
- the presenter's VMC from C — the community-issued grant (and the presenter’s acknowledgement half)
- the voucher's VMC grant from C as it sits in C's membership root (the leaf and its path — no copy of the voucher's acknowledgement exists on the presenter's side)
- the presenter's holder secret, and the derivation material linking the presenter’s VRC-side identifier to the presenter’s VMC-side identifier (card 007) — unless the presenter declared one `directed` identifier for both
- the voucher's linkage: either one `directed` identifier used in both the voucher’s VMC and the VRC (WD02's honest default for intra-community edges), or a co-control attestation the voucher issued alongside the VRC (card 007 run by the voucher at issuance — the vouch-under-community-credential shape of ePrint 2026/333); the presenter cannot derive this from the presenter’s own secret
- non-revocation witnesses for the VRC and both VMC handles

#### Public inputs

- context descriptor (scope, purpose, epoch)
- root_C — C's membership root at a stated registry state
- rl_root and epoch — revocation state
- nullifier (only if this context declares reuse detection; otherwise absent)
- transcriptDigest — one transcript for the whole show, including the maintainer's challenge

#### Method

1. ADR clause 1 — the VRC verifies as a vouch made by the holder of the voucher's credential over the presenter's key — [[ref: signature-verify]]
2. ADR clause 2 — the presenter's VMC grant is a leaf of root_C — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-set-membership-over-an-accredited-root))
3. ADR clause 3 — the VRC issuer's VMC grant is a leaf of root_C (offline: proven from the root, not from the voucher) — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-set-membership-over-an-accredited-root))
4. S7 (WD02, PR #30) — the identifier the presenter used in the VRC and the identifier the presenter’s VMC grant names are controlled by one secret; likewise the voucher's VRC-issuing identifier and the voucher’s VMC-grant identifier (from the voucher’s linkage artifact, or trivially if the voucher used one `directed` identifier) — otherwise clauses 1–3 are about four unrelated identifiers — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-common-control-across-identifiers))
5. S6 — the two authenticated member leaves are distinct; this rejects reuse of one leaf, but does not by itself reject one controller with multiple memberships — [[ref: distinctness]] ([[ref: construction record]] 005, [Distinct member / distinct issuer](#construction-005-distinct-member-distinct-issuer))
6. S4 — the presenter's presentation key derives from the secret the presenter’s VMC/VRC bind to — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-holder-binding-key-from-secret))
7. C1–C3 — neither VMC handle nor the VRC handle is in the set under rl_root at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-non-revocation-against-a-status-root))
8. P4 (parameterised) — if the context declares reuse detection, emit the scoped nullifier; else emit none — [[ref: nullifier]] ([[ref: construction record]] 002, [Scoped nullifier (reuse detection)](#construction-002-scoped-nullifier-reuse-detection))
9. S5 — the whole show is bound to transcriptDigest — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-transcript-binding))

#### Disclosure set

- the outcome (verifies / does not)
- root_C, rl_root, epoch (registry state the show was made against — ADR C2)
- context descriptor
- transcriptDigest
- nullifier — only in contexts that declare reuse detection
- anything the presenter deliberately discloses (P3), e.g. an assurance class carried by C's governance (G2)

#### Does not establish

- that the voucher endorses this request — a VRC is standing, not per-request; S5 binds the proof, not the relationship
- that the presenter is one natural person (that is PR-UNQ in a different context, card 002 under its own declaration)
- that C's admission decision for either member was correct (assurance boundary — accreditation carries assurance)
- the voucher's consent to this disclosure — the VRC's effective disclosure is the wider of its two halves (cred-spec PR #27)
- that the voucher's membership was consented in the PR #12 sense — clause 3 proves the community-issued grant only; the acknowledgement half is not in the presenter's hands
- key non-transfer, absence of coercion, agent authority
- that the voucher is still a member in any sense stronger than 'not revoked as of epoch'
- that the voucher's two identifiers are co-controlled when the voucher supplied no linkage and used pairwise identifiers for both — then clause 3 is unprovable by the presenter, and the card says so rather than reading a link out of a field (cred-spec #9)
- distinct humans or controllers merely from unequal member leaves
- a complete implementation from the existence of component runtimes
- unconditional anonymity against network observers, hosted provers or unique disclosed context

#### Adversary, per claim

- **verifier · verifiers-colluding** — P1/P2 — no pairwise-scope identifier of the edge, no counterparty identifier
- **verifiers-colluding** — P4 — proposed cross-context proof unlinkability against colluding verifiers, conditional on the selected proof system and absence of correlatable disclosures; context nullifiers intentionally link reuse and registry/context metadata can also correlate presentations
- **registry-operator · issuer-verifier-colluding** — C3 — currency check does not identify the presenter; holds only if rl_root/root_C are fetched without a per-holder query

#### Horizon

- earliest of: VRC validity · either VMC validity · epoch rollover · status freshness (C4 bound) · root_C cryptoperiod

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `voucher-not-member (unsat)`, `self-vouch (unsat)`, `vrc-signature-invalid (verify)`, `transcript-digest-mismatch (verify)`, `handle-revoked (unsat at epoch)`, `rl-root-stale`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| Groth16 / BN254 / Poseidon — candidate composition, with credential authenticity, holder linkage and non-revocation still requiring implementation | unmeasured for the complete statement; component figures cannot be added into a validated end-to-end estimate | unmeasured | CIRCUITS.md numbers per component |
| blackbox: Gro15 SPS credentials + hiding KZG + Groth–Sahai for the algebraic part, commit-and-prove SNARK for f (paper §8–9) | paper-reported benchmark pointer only; exact revision, workload and applicability to ADR-001 require verification | unmeasured | ePrint 2026/333 §10 |
| legacy rails: ECDSA/Ed25519 credentials proven as-signed (Longfellow / Crescent → vouchable, paper App. A) | unmeasured for the selected credential format and complete ADR-001 statement | unmeasured | zkp-tf #17 (SIROS catalog), ePrint 2026/333 App. A |
| post-quantum route: Flock-class binary-field prover for the hash side (membership, non-revocation, transcript) — signature clauses over curve-based credentials remain the open cost | unmeasured; proof size hundreds of kB vs ~1 kB Groth16 — a profile trade (ADR-001 D3) | unmeasured | board/stacks/flock.json |

#### Issuance requirements

- X3, concretely: the VMC and VRC signatures or a published commitment must be ZK-friendly — either SPS on BLS12-381 (blackbox), a SNARK-native signature, or an additional Poseidon/KZG commitment alongside `eddsa-jcs-2022` (cred-spec #17)
- C publishes root_C and rl_root per epoch, fetchable anonymously (T2, C3)
- membership leaf = the community-issued grant (PR #12 pair): the proof covers the grant half
- a VRC issued from a pairwise-scope identifier by a member who wants it usable in community-anchored proofs carries the issuer's co-control attestation to their VMC-side identifier (card 007 at issuance) — or the member declares `directed` and uses one identifier; the credential layer names the option, not the link (cred-spec #9)

#### Provenance

- ADR-001 Community-Anchored Proof (Proposed 2026-08-25)
- cred-spec construction 2 (community-anchored ZKP)
- cred-spec #21 → PR #26 (edge verifiability w.r.t. a verifier)
- cred-spec #8 → PR #12 (VMC pair)
- ePrint 2026/333 §2.3–2.4, §5.3–5.4, §7.2, §8–10
- cred-spec PR #30 §Community-Anchored Zero-Knowledge Proof (WD02 draft): "the proof must additionally establish common control" · cred-spec #31 row #9
- commit: github.com/mitchuski/dtgwg-zkp-mage

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `carded` | mitchuski | ZKP_TF_RUN-2026-08-28.md §3 (ten refinements) + this card; method fully bound to gadgets; composed yield and does-not written fresh |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-05 | mitchuski | WD02 three-scope vocabulary (PR #30); S7 common-control clause via card 007; voucher-side linkage stated as ingredient + issuance option — re-carded, state unchanged |


### Construction 011 · Pairwise edge (VRC possession, directed personas shown, pairwise identifiers hidden)

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `carded` |
| priority | P2 |
| constructor | mitchuski |
| requested by | cred-spec construction 1 |
| request | zkp-tf #18 |

**Composes:** [003](#construction-003-transcript-binding) ∧ [004](#construction-004-holder-binding-key-from-secret) ∧ [006](#construction-006-non-revocation-against-a-status-root) ∧ [007](#construction-007-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

#### Statement

A verifier learns that two disclosed persona identifiers (declared `directed`) hold a valid relationship credential between them, without learning the pairwise-scope identifiers under it and without a handle that correlates this presentation with any other.

**Need.** prove two known personas have a relationship without exposing the private pairwise channel (cred-spec §Pairwise Zero-Knowledge Proof, WD02 wording: disclose the parties’ `directed` persona identifiers while hiding the underlying `pairwise` ones)

#### Witness

*Never leaves the holder.*

- the VRC and the pairwise-scope identifier pair it was issued between
- the co-control witnesses linking each disclosed `directed` persona identifier to its hidden pairwise identifier (card 007; cred-spec #9: co-control proven in ZK, never a field) — the counterparty’s half is theirs to supply
- the presenter’s holder secret

#### Public inputs

- the two `directed` persona identifiers (disclosed on purpose)
- rl_root, epoch
- transcriptDigest

#### Method

1. the VRC verifies under the issuing pairwise identifier’s key — [[ref: signature-verify]]
2. each disclosed persona identifier is co-controlled with its hidden pairwise identifier (card 007) — the presenter’s from their own secret, the counterparty’s from the counterparty’s attestation — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-common-control-across-identifiers))
3. the presenter’s presentation key derives from the secret behind their pairwise identifier — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-holder-binding-key-from-secret))
4. the VRC handle is not revoked at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-non-revocation-against-a-status-root))
5. bound to one transcript — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-transcript-binding))

#### Disclosure set

- the two `directed` persona identifiers
- rl_root, epoch
- transcriptDigest

#### Does not establish

- any community-level assurance (that is card 010)
- that the personas are distinct natural persons
- the relationship's content beyond what the statement discloses
- the counterparty’s persona↔pairwise linkage without the counterparty’s attestation (card 007 negative space)

#### Adversary, per claim

- **verifier · verifiers-colluding** — pairwise identifiers hidden; no cross-presentation correlator minted by the linkage itself

#### Horizon

- VRC validity
- status freshness

#### Conformance fixtures

Families: `accepts` · `rejects-verify` · `unlinkable`

Rejection codes: `co-control-unproven`, `vrc-signature-invalid`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| Groth16 composition of 004+006+003 with an in-circuit signature check | unmeasured — dominated by the signature gadget (X3 again) | unmeasured | board/README |
| paper Construction II show with N=1 vouch (§7.2) | paper Table 1 | unmeasured | ePrint 2026/333 §7.2 |

#### Issuance requirements

- as card 010's X3 line

#### Provenance

- cred-spec §Pairwise Zero-Knowledge Proof (WD02 wording, PR #30)
- cred-spec #9 (F post: co-control as requirement, not field)
- cred-spec PR #30 §Correlation Scope

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-08-28 | `carded` | mitchuski | cred-spec construction 1 + #9 F post |

Revisions within a state:

| date | by | note |
|---|---|---|
| 2026-09-05 | mitchuski | WD02 vocabulary; co-control routed through card 007 — re-carded, state unchanged |


### Construction 012 · Intentional correlation — one controller across k credentials

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `carded` |
| priority | P2 |
| constructor | mitchuski |
| requested by | talltree / geoffturk / stormer78 |
| request | cred-spec #22 (talltree 08-29: 'the ZK proof simply needs to prove the same person controls the DIDs') · cred-spec PR #30 §Choosing a scope |

**Composes:** [003](#construction-003-transcript-binding) ∧ [006](#construction-006-non-revocation-against-a-status-root) ∧ [007](#construction-007-common-control-across-identifiers) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

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

#### Method

1. for each pair (identifier_1, identifier_i), i = 2..k: both open to the same s — k−1 common-control clauses sharing one witness — [[ref: key-binding]] ([[ref: construction record]] 007, [Common control across identifiers](#construction-007-common-control-across-identifiers))
2. no credential handle is in the set under rl_root at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-non-revocation-against-a-status-root))
3. the whole show is bound to transcriptDigest — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-transcript-binding))

#### Disclosure set

- the outcome (one controller / not shown)
- the holder-declared set of credentials shown to share a controller — a per-presentation choice, the disclosure this card exists to make
- the identifiers the holder chose to disclose, and no others
- rl_root, epoch, transcriptDigest

#### Does not establish

- that the presenter is one natural person (k credentials, one secret: an agent holding a person's secret satisfies every clause — card 002 under its own declaration establishes uniqueness)
- anything about credentials not in the show: intentional correlation is declared per presentation and does not widen any identifier's declared scope
- that the communities involved consented to be named together — the disclosure is the holder's
- what any of the credentials asserts beyond existence and non-revocation (a VPC's persona content, a VRC's statement) unless disclosed

#### Adversary, per claim

- **verifier · verifiers-colluding** — no identifier beyond the disclosed set, and no cross-presentation handle: two verifiers shown different subsets cannot join them through this proof
- **issuer-verifier-colluding · registry-operator** — the issuer of any one credential in the show learns nothing about the others from the proof; the revocation-state fetch must not be a per-holder query (card 006 C3)

#### Horizon

- earliest of: any shown credential's validity · epoch rollover · the shortest identifier key-validity among the k (card 007)

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify` · `unlinkable` · `current`

Rejection codes: `co-control-unproven`, `handle-revoked`, `show-not-declared (lint: a credential in the witness set has no disclosure declaration)`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| no proof — where the holder deliberately reused one `directed` or `public` identifier across the credentials, the correlation is on their face (WD02 §Choosing a scope) | zero | unmeasured | cred-spec PR #30 |
| Groth16 composition: (k−1) × card-007 openings + k non-revocation legs + 1 transcript constraint | unmeasured; conjecture linear in k with the 007 and 006 per-leg costs | unmeasured | board/cards/007.json, 006.json |

#### Issuance requirements

- as card 007: every identifier that may later be co-proven is, or carries, a ZK-openable commitment to s (X3 applied to identifiers)

#### Provenance

- cred-spec #22 (talltree 2026-08-29T22:40Z: the three-scope ZK observation)
- cred-spec PR #30 §Choosing a scope (WD02 draft) · Privacy Consideration 2 (intentional correlation via personas)
- cred-spec §VPC (Verifiable Persona Credential) — the credential-layer instrument for the same intent

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-29 | `requested` | talltree (cred-spec #22) | cred-spec #22 comment 2026-08-29T22:40Z: 'it reduces the set of ZK proofs needed for intentional correlation … the ZK proof simply needs to prove the same person controls the DIDs' |
| 2026-09-05 | `carded` | mitchuski | composed from 007 + 006 + 003 under one transcript; yield and negative space written fresh (composition rule) |


### Construction 020 · Delegation chain (VDC) — agent acts for a member

*This record is at state `carded`: it is written and validates; no runtime has measured it. Costs marked conjecture are conjecture (drafting rule 4). Informative.*

| | |
|---|---|
| kind | composed |
| state | `carded` |
| priority | P2 |
| constructor | construction: sankarshanmukhopadhyay · DenisPopov15 · mitchuski (per ScottJeezey, cred-tf #40) · record: stormer78 (PR #19) |
| requested by | stormer78 / sankarshanmukhopadhyay |
| request | cred-spec PR #19 open question 6; ADR-001 §05 'deserves its own record once this one is proven' · cred-tf #40 (stormer78 08-22 design note; ScottJeezey 08-24: "on our list alongside Q2") · cred-spec #31 pre-merge list for #19 |

**Composes:** [001](#construction-001-set-membership-over-an-accredited-root) ∧ [003](#construction-003-transcript-binding) ∧ [004](#construction-004-holder-binding-key-from-secret) ∧ [006](#construction-006-non-revocation-against-a-status-root) — a [[ref: composed construction]]: one transcript, one [[ref: disclosure set]], written fresh.

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

#### Method

1. act ∈ scope_n ⊆ … ⊆ scope_root, depth ≤ maxDepth, validUntil monotone along the chain, root hop signed by the principal (Scott’s predicate shape, cred-tf #40) — [[ref: chain-resolve]]
2. each hop’s delegate countersigned the grant: `accepts` matches the grant digest (digestMultibase, WD02 D-A) and verifies under the delegate’s key — [[ref: signature-verify]]
3. principal is a leaf of root_C — [[ref: set-membership]] ([[ref: construction record]] 001, [Set membership over an accredited root](#construction-001-set-membership-over-an-accredited-root))
4. the agent's presentation key derives from the leaf-hop delegate secret — [[ref: key-binding]] ([[ref: construction record]] 004, [Holder binding (key from secret)](#construction-004-holder-binding-key-from-secret))
5. no hop revoked at epoch — [[ref: non-revocation]] ([[ref: construction record]] 006, [Non-revocation against a status root](#construction-006-non-revocation-against-a-status-root))
6. bound to one transcript — [[ref: transcript-bind]] ([[ref: construction record]] 003, [Transcript binding](#construction-003-transcript-binding))

#### Disclosure set

- the invoked scope term
- root_C, rl_root, epoch
- transcriptDigest

#### Does not establish

- that the principal authorised this specific act (grant ≠ invocation — the invocation is a trust-task artifact)
- the principal's identity
- that the agent is not also acting for others
- that the principal has not declined renewal — in the core, revocation is non-renewal within one validUntil; the profile’s credentialStatus re-adds a live lookup and this card’s non-revocation leg is what lets the presentation carry it instead
- what the delegate actually did in the principal’s name — the invocation record lives on the framework side (the artifact gap, cred-tf #40 Q8)
- chain-length hiding without a validated fixed-shape or padded profile
- compatibility with a current merged credential revision until its grant, acceptance, status and chaining semantics are pinned and reconciled

#### Adversary, per claim

- **verifier · verifiers-colluding** — principal hidden under the selected proof assumptions and declared disclosure, against the verifier and colluding verifiers; hiding chain length additionally requires validated padding/fixed shape and metadata analysis, which are not established here

#### Horizon

- the shortest validUntil in the chain
- status freshness

#### Conformance fixtures

Families: `accepts` · `rejects-unsat` · `rejects-verify`

Rejection codes: `scope-escalation (unsat)`, `depth-exceeded`, `hop-revoked`

#### Construction options

*Routes through the construction-selection gate ([DTG-ZKP-REQ] §16.1), each with its cost as measured or as conjectured.*

| route | cost | status | source |
|---|---|---|---|
| bounded monolithic proof of a fixed maximum chain depth; padding semantics and authenticated hop checks to be defined | unmeasured | unmeasured | editorial alternative for review, 2026-09-08 |
| recursive/folding proof per hop | unmeasured | unmeasured | PATH-MAP P4 (PLONKish/folding counter-proposal welcome) |

#### Issuance requirements

- VDC as an edge credential type (cred-spec PR #19, rebased over WD02 vocabulary) with ZK-friendly signatures — X3 applies
- the delegator’s identifier is `directed`, a context-scoped identifier per delegation, not `pairwise` (cred-spec #31 pre-merge note for #19)
- grant and acceptance digests are digestMultibase (WD02 D-A); the acceptance is REQUIRED (cred-tf #40; review feedback folded per #31)
- chaining (`parent`, `maxDepth`, `credentialStatus`) exists only in the opt-in profile, visible to the verifier, costs stated (cred-tf #40 §1)

#### Provenance

- cred-spec PR #19 (VDC draft)
- liveness reqs v0.4 delegation evidence
- ADR-001 §05
- ePrint 2026/333 App. B.1
- cred-tf #40 (delegation as a design-time case; the chain-resolution boundary) · cred-spec #31 (#19 pre-merge checklist)

#### Record history

| date | to | by | evidence |
|---|---|---|---|
| 2026-08-28 | `requested` | mitchuski | zkp-tf #18 (talltree 08-26 / Scott 08-27): seed set for the board |
| 2026-09-05 | `carded` | mitchuski | ScottJeezey accepted delegation-chain validity as a ZKP TF target (cred-tf #40, 2026-08-24); predicate shape + acceptance clause + core/profile split from stormer78’s note; issuance lines from cred-spec #31 |
<!-- generated-section:constructions:end -->

<!-- generated-section:stacks:start -->
## Proving Systems

This section is informative.

A proving-system entry records facts a reader can check — proof system, field, setup, licence, audit statement, published figures with their source and a verification date — and never a recommendation. Recommendations require separate task-force review informed by the reproduction ladder over verification-registry rows; a figure in this section is the proving system's own or the evidence repository's, and says so. Entries of different kinds are not comparable rows: a catalog of circuits over credentials as already signed and a general-purpose prover answer different questions, and the kind is stated first. This section is generated from `conformance/stacks/`.

| kind | entries | what the kind means |
|---|---|---|
| general-stack | [flock](#stack-flock-binary-field-snark-for-batched-boolean-computation-standard-hashes) · [provekit](#stack-provekit-world-noir-whir-client-side-proving) | general-purpose proving systems — any statement the frontend expresses; issuer-agnostic |
| hand-rolled | [lab-groth16-circom](#stack-lab-groth16-circom-2-snarkjs-bn254-poseidon) | constructions written and measured in the evidence repository as reference implementations |
| as-signed-catalog | [siros-longfellow](#stack-siros-circuit-catalog-longfellow-libzk-v1-mdl-eudi-pid-as-signed) | catalogs of circuits that prove over credentials exactly as already signed — substrate for the legacy-rails route, not DTG construction routes |

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

#### Published figures (the stack's own, or the lab's — never this book's)

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

#### Published figures (the stack's own, or the lab's — never this book's)

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

#### Published figures (the stack's own, or the lab's — never this book's)

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

#### Published figures (the stack's own, or the lab's — never this book's)

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

#### Notes

- The only stack with registry rows today: the reproduction ladder places it at reproduced-cross-arch for constructions 001/002/003/005.
- Chosen for toolchain maturity so the selection conversation happens against measured numbers; a PLONKish or folding counter-proposal through the same gate is invited (PATH-MAP P4).
<!-- generated-section:stacks:end -->

## Security Considerations

This section is informative.

1. **Soundness is a property of a construction, not of this document.** A construction record states clauses; only a runtime realises them, and only an independent reproduction shows that the realisation behaves. A record at state `carded` has not been shown sound by anyone. Readers MUST NOT read a record's presence in this specification as a claim that its construction is secure.
2. **Reproduction is not audit.** A registry entry records a reproduction claim and its supporting artifacts. The evidence-repository build-report acceptance checker compares submitted metadata with a manifest; it does not itself execute the suites, verify a proof or authenticate the independence of the submitter. Evaluate those forms of evidence separately. It does not establish that the circuit is free of under-constrained signals, unchecked booleanity or interface drift between an audited commit and a shipped one. An audit claim in a proving-system entry that names no reviewed commit and file set is printed as "claimed; not located".
3. **Trusted setup.** Constructions on pairing-based proving systems depend on a setup whose entropy must be destroyed. The reference constructions' setup is a fixed-entropy laboratory setup, unusable in production and marked as such; production deployment requires a ceremony, and the registry treats setup-chain digests as advisory because a real ceremony is machine-local by design.
4. **Composition.** Two constructions that are individually sound may leak jointly. Composed construction records are required to declare their own disclosure set and negative space rather than inherit the union of their parts, and are bound to one presentation transcript. Implementations MUST NOT present the components of a composed construction as separate proofs and claim the composed record's properties.
5. **Common control.** Under the Credentials Core Specification's three correlation scopes, two `pairwise` identifiers of one party differ by construction. A construction that reads one party out of two credentials must prove common control of both identifiers (construction 007) or rely on the party having declared one `directed` identifier; a presenter cannot prove a counterparty's common control without the counterparty's witness or attestation. Records that need it say so; implementations that assume it silently are unsound.
6. **Self-vouching.** A community-anchored proof without a distinctness clause (S6) accepts a member vouching for themselves under two identifiers. The clause is added in construction 010 and is required of any implementation of that record.
7. **Replay and transcript binding.** Every construction is bound to one canonical transcript digest. A proof presented against a different transcript is a different proof and MUST fail verification.
8. **Post-quantum horizon.** Pairing-based constructions have no post-quantum path; hash-based proving systems do, at a cost in proof size. Each record's horizon and each proving system's entry state which applies. Suite agility requirements are those of [DTG-ZKP-REQ] §9.9.

## Privacy Considerations

This section is informative. Items 1–6 are written by the editors; the numbered list that follows them is generated from the construction records' adversary and negative-space fields and is regenerated whenever a record changes.

1. **A privacy claim without an adversary is not a claim.** Every privacy property in a construction record names the party it holds against — the verifier, verifiers colluding, issuer and verifier colluding, or the registry operator — and the horizon over which it holds. Properties not named are not claimed.
2. **Nullifiers are declared links.** A scoped nullifier is emitted only in contexts that declare reuse detection, and within such a context it is the only link between presentations by one holder. Full unlinkability and reuse detection cannot coexist in one context ([PoP-2026] §5.3); records parameterise the choice by context descriptor rather than promising both.
3. **Registry state fetches.** Establishing that a credential is current must not itself identify the holder. Constructions expose accepted roots and proofs while retaining membership paths and openings as private witnesses. A deployment describes how root and witness retrieval can correlate the holder, including any live state fetches. "No live lookups" is a profile default, not an absolute.
4. **Proof size as a correlator.** Proof size, timing and error surface can distinguish constructions and therefore holders. Records list proof size per option; profiles should fix one option per context so that the choice of construction does not itself disclose.
5. **What the credential layer holds.** A construction can blind only what the credential or framework gives it in committed form. Durable correlators that live in Trust Task artefacts — identifiers, thread identifiers, the task-context pairing — are outside any construction's protection until the framework commits to them (construction 008, [DTG-CRED-TF-39]).
6. **Intentional correlation is the holder's act.** A proof of common control across identifiers (constructions 007, 012) discloses to the party it is made to and widens no identifier's declared scope. Verifiers MUST NOT infer from such a proof that the identifiers may be correlated elsewhere.

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
- **Construction 007** — against verifier, verifiers-colluding: no cross-presentation handle: the proof is transcript-bound and emits no identifier-derived value; two verifiers comparing proofs learn only what the enclosing cards disclosed to each
- **Construction 007** — against issuer-verifier-colluding: the secret s is never revealed and no per-identifier salt is; an issuer who minted one identifier's credential learns nothing about the other from the proof
- **Construction 008** — against verifier: route 1 intends to hide a low-entropy taskContext from a verifier without the opening, assuming an independent uniformly random 128-bit secret blinding value and the commitment hash assumptions; a public or disclosed opening does not provide this protection
- **Construction 008** — against verifiers-colluding: verifiers colluding across contexts can link any repeated visible C in route 1. Route 2 cross-context unlinkability is a design objective, not established by this card; it depends on PRF key secrecy, domain separation and the absence of other stable presentation identifiers
- **Construction 008** — against issuer-verifier-colluding: the issuer that placed C and a verifier together can link C to the exchange (the issuer knows u) — stated, not hidden: issuer–verifier collusion is outside this card's protection
- **Construction 010** — against verifier, verifiers-colluding: P1/P2 — no pairwise-scope identifier of the edge, no counterparty identifier
- **Construction 010** — against verifiers-colluding: P4 — proposed cross-context proof unlinkability against colluding verifiers, conditional on the selected proof system and absence of correlatable disclosures; context nullifiers intentionally link reuse and registry/context metadata can also correlate presentations
- **Construction 010** — against registry-operator, issuer-verifier-colluding: C3 — currency check does not identify the presenter; holds only if rl_root/root_C are fetched without a per-holder query
- **Construction 011** — against verifier, verifiers-colluding: pairwise identifiers hidden; no cross-presentation correlator minted by the linkage itself
- **Construction 012** — against verifier, verifiers-colluding: no identifier beyond the disclosed set, and no cross-presentation handle: two verifiers shown different subsets cannot join them through this proof
- **Construction 012** — against issuer-verifier-colluding, registry-operator: the issuer of any one credential in the show learns nothing about the others from the proof; the revocation-state fetch must not be a per-holder query (card 006 C3)
- **Construction 020** — against verifier, verifiers-colluding: principal hidden under the selected proof assumptions and declared disclosure, against the verifier and colluding verifiers; hiding chain length additionally requires validated padding/fixed shape and metadata analysis, which are not established here

### Negative space as recorded, per construction

*Generated from the `doesNotEstablish` field of every construction record (first three items each; the full list is in the record).*

- **Construction 001** does not establish: that the community's admission decision was correct (assurance boundary); that the leaf is current (see card 006); which member the holder is; …
- **Construction 002** does not establish: one natural person globally; one enrolment per issuer or ecosystem (second point of the trade curve — governance, not cryptography); cross-context uniqueness
- **Construction 003** does not establish: freshness beyond what the challenge carries; that the verifier's challenge was itself honest; correct JCS/SHA-256 evaluation inside the circuit merely because a supplied scalar is constrained; …
- **Construction 004** does not establish: non-transfer of the secret; absence of coercion or account sharing; agent authority or consent; …
- **Construction 005** does not establish: that the two parties are independent in the accreditation sense (declared, not proven — X8); that either is honest; distinct natural persons or independent key controllers merely from distinct leaves, keys or issuer identifiers
- **Construction 006** does not establish: that revocation is instantaneous — only that the handle was not revoked as of `epoch` (C4's published bound); that the registry's revocation decision was correct; that the verifier performed no live lookup — the card makes the presentation self-carrying (public root + ZK proof; witness remains private); whether a deployment still phones home is a profile statement, not a proof property
- **Construction 007** does not establish: that the controller is one natural person — two agents or two people sharing a secret satisfy the clause (that is card 002's uniqueness, under its own declaration); that either credential is currently valid or unrevoked (card 006); that the holder intended the two identifiers to be correlated beyond this verifier — the proof is a disclosure to the party it is made to, not a widening of either identifier's declared scope; …
- **Construction 008** does not establish: unlinkability of presentations carrying the same visible commitment C; hiding plaintext alone does not prevent equality-based correlation; that the trust task completed, or what was done in it — completion evidence is a framework artifact outside any credential (the artifact gap, cred-tf #39/#40); that the binder's plaintext is not held elsewhere — the framework holds it in the Trust Task documents; this card blinds only the copy the credential carries; …
- **Construction 010** does not establish: that the voucher endorses this request — a VRC is standing, not per-request; S5 binds the proof, not the relationship; that the presenter is one natural person (that is PR-UNQ in a different context, card 002 under its own declaration); that C's admission decision for either member was correct (assurance boundary — accreditation carries assurance); …
- **Construction 011** does not establish: any community-level assurance (that is card 010); that the personas are distinct natural persons; the relationship's content beyond what the statement discloses; …
- **Construction 012** does not establish: that the presenter is one natural person (k credentials, one secret: an agent holding a person's secret satisfies every clause — card 002 under its own declaration establishes uniqueness); anything about credentials not in the show: intentional correlation is declared per presentation and does not widen any identifier's declared scope; that the communities involved consented to be named together — the disclosure is the holder's; …
- **Construction 020** does not establish: that the principal authorised this specific act (grant ≠ invocation — the invocation is a trust-task artifact); the principal's identity; that the agent is not also acting for others; …
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

Construction records, fixture families and rejection codes are identified by ASCII identifiers that are not localised. Human-readable statements in records are written in English and MAY be translated; the machine-readable record is authoritative. Canonical transcripts are encoded under [RFC8785] so that string ordering and Unicode normalisation do not vary by locale; implementations MUST canonicalise before digesting.

## Accessibility Considerations

This section is informative.

The presentation of a zero-knowledge proof imposes a proving-time and memory cost on the holder's device. Construction options record proving time on representative consumer devices, including low-memory devices, so that profiles can be chosen that do not exclude holders with older or constrained hardware. Mediated proving, where a holder delegates proving to an agent, is addressed by the requirements document's agent-mediated profile and by the mediator instrument of the evidence repository; it MUST NOT be the only path available to a holder.

## Conformance

This section is normative.

Conformance to this specification is claimed per **conformance target** and is demonstrated by **conformance tests** that are published as data — machine-readable records, fixtures and registry rows — rather than as prose. The tests below are the ones this Working Draft carries; they originate in the task force's evidence repository and are maintained in this repository's `conformance/` directory under Apache-2.0.

### Conformance Targets

1. **Construction record.** A construction record conforms when it validates against the construction-record schema and rules (§Conformance Tests, test 1). A record that does not validate MUST NOT appear in this specification and MUST NOT be cited as a DTG construction.
2. **Constructor.** A [[ref: constructor]] — a party that writes a construction record or a runtime for it — conforms when every clause of the record's method is bound to a named gadget, every construction option states whether its cost is measured or conjectured and names its source, and every measured claim is backed by a runtime transcript or a verification-registry row. A constructor MUST NOT vet their own construction.
3. **Prover.** A prover implementation of a construction record conforms when, for every fixture family the record names, it produces the outcome the fixture expects — a valid proof for `accepts`, no proof (an unsatisfiable witness) for `rejects-unsat`, and the defined observable checks for the `unlinkable` fixture family, where declared (finite fixtures do not prove general unlinkability) — and when every proof it produces is bound to the canonical transcript digest of the presentation.
4. **Verifier.** A verifier implementation conforms when, for every fixture, it reaches the expected outcome **and emits the expected rejection-reason code** from the versioned register; when it rejects a proof presented against a different transcript; when it rejects a proof made against a registry state it does not accept; and when its outputs contain none of the prohibited claim patterns (test 3). A verifier MUST NOT infer from a proof anything the record's disclosure set does not contain.
5. **Issuer and registry.** An issuer conforms to a construction record when it satisfies the record's issuance requirements — for example, a ZK-openable commitment beside the credential signature where the record requires one. A registry conforms when it publishes the set roots the record names at stated registry states, with a published bound on the delay between a change and the roots reflecting it.
6. **Proving-system entry.** A proving-system entry conforms when it states its licence, setup, provenance and an audit statement, sources every published figure, and — to rise above the self-described rung — ships the pinned manifest and re-derivation script the verification registry needs to reproduce a construction on it.

### Conformance Tests

The following tests are the evidence a conformance claim rests on. Each is runnable; the first two run in this repository's continuous integration on every change.

1. **Record validation** (`conformance/validate.mjs`). Every construction record, request and proving-system entry is checked against its schema and against the rules that encode the task force's drafting rules. Refusals are register strings, never prose: `card-no-adversary`, `card-no-horizon`, `card-no-does-not-establish`, `card-clause-unbound`, `card-composed-yield-is-union` (a composed record whose disclosure set is the union of its parts), `card-composed-no-single-transcript`, `construct-no-measurement` (a record at state `constructed` with no measured option), `run-vectors-missing`, `vet-no-registry-row`, `history-not-monotone`, `request-construction-missing`, `stack-no-audit-statement`, `stack-benchmark-no-source`. The full list is in the validator. A record MUST validate before it is rendered into this specification.
2. **Generated text is current** (`conformance/test.mjs`). The rendered Construction Records, Requests Answered, Proving Systems and derived Privacy Considerations sections carry a digest of the records they were generated from. The test recomputes that digest, regenerates every marked section and every generated term using `conformance/generate.mjs` and `render-lib.mjs`, and compares their actual contents. Missing or duplicate boundaries, edited generated prose and missing or obsolete generated terms fail the check. Editor-written sections remain outside this comparison. The check establishes consistency with the renderer and records, not cryptographic correctness.
3. **Conformance fixtures** (evidence repository, `runtimes/fixtures/`; fixture schema `x1-fixtures/v0`). A fixture is a canonically encoded input — a context descriptor and a canonical transcript, never an opaque label — an expected outcome, and a named reason from a versioned **rejection-reason register** (v2: 66 exact codes and 29 parameterised families, append-only). Vectors come in three classes: **accept**, **reject**, and **lint**, the last testing that a verifier's output makes no claim the record does not license. The current suite is 40 vectors across nine predicate families with a manifest naming the register version. A consumer harness re-derives every digest and re-runs the constructions, demanding the same outcome and the same reason code; a second consumer with no shared code, written in another language, has consumed all vectors with matching outcomes and byte-identical reason codes and re-derived every embedded canonical digest. Two implementations that pass the same suite have instantiated the same decisions, independently of language or hash.
4. **Independent reproduction** (the [[ref: verification registry]]; acceptance flow gates A–G). A [[ref: runner]] — a party other than the constructor — rebuilds a construction from published source on independent hardware, runs its suites, and files the pinned report sections. The acceptance checker re-derives a manifest-comparison verdict from submitted report fields. That verdict alone does not establish that the submitter executed the reported build or tests. Required compiled-artifact digests and constraint metadata match the selected manifest; setup-chain artifacts, including local verification keys, follow the manifest’s advisory policy because local contributions introduce randomness. A production proof is verified against its selected authorized verification key; advisory build-report handling does not permit arbitrary key substitution. Independent execution and provenance require their own evidence. Admission of a submitting party and publication of a row are the [[ref: maintainer]]'s acts and are never delegated to tooling; the acceptance flow itself is the [[ref: registry verifier]]. A [[ref: requester]] who asked for a construction takes no part in vetting it.
5. **The reproduction ladder.** A construction option's reproduction state is derived from registry rows, never asserted: `self-described` → `lab-measured` → `reproduced-once` (one row, party ≠ constructor) → `reproduced-cross-arch` (rows on two or more OS/architecture pairs) → `reproduced-multi-seat` (two or more parties, two or more architectures, fixtures green under the record's name). This specification uses RECOMMENDED of a proving system for a construction only at the top rung, names the version the rows were made against, and withdraws the word when the version changes until a new row lands.

### What conformance does not establish

Reproduction and behaviour are not review. A construction at the top of the ladder has been shown to build identically elsewhere and to behave as its fixtures require; it has not been shown free of under-constrained signals or of interface drift between an audited commit and a shipped one. Audit is a separate claim with its own evidence — a reviewed commit and file set — and is recorded as such in proving-system entries.

## References

This section is informative.

### Normative References

- **[DTG-CRED]** DTG Credentials Core Specification. This draft uses WD02 vocabulary; the selected revision and its implementation compatibility remain to be pinned. Trust over IP Foundation. <https://trustoverip.github.io/dtgwg-cred-spec/>
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
- **[DTG-ZKP-EVIDENCE]** DTG ZKP evidence repository — reference runtimes, conformance fixtures, verification registry, construction records. <https://github.com/mitchuski/dtgwg-zkp-mage>
- **[DTG-CRED-TF-39]** "Privacy: Appropriately supporting unlinkability, ZKP and selective disclosure", dtgwg-cred-tf discussion #39. <https://github.com/trustoverip/dtgwg-cred-tf/discussions/39>
- **[DTG-CRED-TF-40]** "Delegation as a case study in the design-time window", dtgwg-cred-tf discussion #40. <https://github.com/trustoverip/dtgwg-cred-tf/discussions/40>
- **[DTG-CRED-31]** "WD02 merge plan: sequencing the five outstanding PRs into a consistent whole", dtgwg-cred-spec issue #31. <https://github.com/trustoverip/dtgwg-cred-spec/issues/31>
