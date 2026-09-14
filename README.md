> **Zenodo DOI:** [10.5281/zenodo.21379780](https://doi.org/10.5281/zenodo.21379780) — pending re-publication under this scope

# PP-SPEC-001 · Proof Protocol™ Specification v1.0

**Domain-Agnostic Architecture for Machine-Verifiable, Independently Witnessed Proof**

**Version:** 1.0 - Public Specification
**Issued by:** HACKERverse® / Nebulonium, Inc.
**Author:** Craig Ellrod, Founder & CEO
**License:** Creative Commons Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)
**Document ID:** PP-SPEC-001
**Status:** Published
**Maintained by:** Proof Economy™ Standards Alliance (PESA)
**Specification URI:** proofprotocol.io

> Proof Protocol™ is not a blockchain. No token. No wallet. No chain. No gas fees. Proof is anchored to the NIST Randomness Beacon. The stamp is earned, not minted.

> Agents and humans do not trust agents. They trust proof.

---
## Canonical Status

Proof Protocol™ is the canonical standard for the Proof Economy™. It defines
the domain-agnostic architecture that all conformant domain implementations —
including DKP — are built against. Where any domain protocol's terminology
or framing differs from this specification, this document is authoritative.

---
## Scope and Relationship to Domain Protocols

This specification defines the domain-agnostic architecture that any Proof Protocol-conformant domain implementation must satisfy. It does not itself specify how any particular domain — agentic AI security, financial risk, clinical research, or any other — produces proof. Domain protocols implement this specification's architecture against domain-specific evidence.

**DKP (Defensible Knowledge Proof)** — cryptographic proof for agentic AI and cybersecurity execution — is the reference implementation of this specification. DKP-SPEC-001 defines DKP's domain-specific schema, roles, and mechanics. Where this document and DKP-SPEC-001 differ in domain-agnostic terminology, this document is authoritative; DKP-SPEC-001's domain-specific extensions govern within their own scope.

Future domain protocols (financial risk and audit, AI safety and alignment, software supply chain, clinical research, and others identified in the Proof Economy™ Universal Framework) implement this same architecture against their own domain evidence and are peers to DKP, not subordinate to it.

## Changelog

- v1.0 - original release. Extracted and formalized from the domain-agnostic architecture section of the Proof Economy™ Universal Framework (PE-SPEC-001 draft) to stand as its own citable, conformance-bearing specification.

## Contents

- [Section 1 - Overview](#section-1---overview)
- [Section 2 - The Self-Attestation Problem](#section-2---the-self-attestation-problem)
- [Section 3 - The Valid Proof Test](#section-3---the-valid-proof-test)
- [Section 4 - Definitions](#section-4---definitions)
- [Section 5 - The Five-Tier Corroboration Model](#section-5---the-five-tier-corroboration-model)
- [Section 6 - Pre-Execution Commitment](#section-6---pre-execution-commitment)
- [Section 7 - Independent Witnessing](#section-7---independent-witnessing)
- [Section 8 - Ledger Anchoring](#section-8---ledger-anchoring)
- [Section 9 - Cryptographic Integrity vs. Structural Independence](#section-9---cryptographic-integrity-vs-structural-independence)
- [Section 10 - Conformance Requirements for Domain Protocols](#section-10---conformance-requirements-for-domain-protocols)
- [Section 11 - Relationship to POCI Conformance Levels](#section-11---relationship-to-poci-conformance-levels)
- [Section 12 - Prior Art and Category Provenance](#section-12---prior-art-and-category-provenance)
- [Section 13 - Certification](#section-13---certification)

## Section 1 - Overview

The Proof Protocol™ is a domain-agnostic specification for machine-verifiable, independently witnessed, tamper-evident proof of system and agent behavior. It defines what valid proof is, the minimum architectural properties any conformant implementation must exhibit, and the test by which a proof artifact is distinguished from a mere attestation.

This specification does not prescribe a schema, wire format, or implementation. It prescribes the architectural properties a domain protocol's schema and implementation must satisfy to be Proof Protocol-conformant.

## Section 2 - The Self-Attestation Problem

Every vendor, system, and AI agent attests to the efficacy of its own behavior. The party that acts, tests, interprets results, and authors the report is the same party. There is no structural mechanism by which a relying party can independently verify whether a claim reflects what actually happened.

This is the Self-Attestation Problem: the party with the greatest interest in a favorable outcome is the sole author of the evidence for that outcome.

The problem is structurally worse for autonomous AI agents than for human-operated systems. An agent can generate plausible, internally consistent, cryptographically sealable records of events that never occurred — at machine speed, without human authorship, without detectable markers of fabrication, and without any party outside the agent's trust boundary being aware that fabrication has taken place. Cryptographic sealing authenticates the seal. It does not authenticate what is underneath it.

## Section 3 - The Valid Proof Test

A record satisfies the **Valid Proof Test** if and only if it was originated by a structurally independent witness before any account of the event was constructed.

**Structural independence** means the witness sits outside the monitored system's trust boundary: it has no administrative authority over, and did not participate in constructing, the execution being witnessed.

A record that fails the Valid Proof Test — however cryptographically signed, hashed, or sealed — is an **attestation**, not a **proof**. Attestation is a declaration. Proof is evidence originated outside the interested party's control. The distinction is structural, not a matter of degree.

## Section 4 - Definitions

| Term | Definition |
|---|---|
| Atomic Execution Unit (AEU) | The minimum observable unit of behavior for proof purposes within a given domain. Domain-agnostic: a security execution, a financial transaction, a clinical observation, a model decision. The irreducible atom of the proof record. |
| Valid Proof Event (VPE) | A structured, machine-readable capture of a single AEU execution, including pre-execution commitment, execution parameters, observed outcome, and cryptographic hash. The basic unit of evidence. |
| Valid Proof Stream (VPS) | An ordered, append-only sequence of VPEs constituting a complete proof session. |
| Pre-Execution Commitment | A cryptographic hash of execution parameters, generated and signed before execution begins, anchored to an independent randomness or timestamp source. Establishes that proof parameters were fixed before the outcome was known. |
| Proof Record | A sealed, structured artifact containing the VPS, the pre-execution commitment reference, cryptographic hashes of inputs and outputs, executing-party identity, and a ledger anchor. The canonical evidence artifact. |
| Proof Correlation Identifier (PCID) | A globally unique identifier binding a Proof Record to its execution. Domain protocols define their own concrete derivation; this specification requires only that the identifier be deterministically derivable from execution-time values the prover does not fully control. |
| ProofRegister™ | A public, append-only ledger of Proof Records, indexed by PCID, queryable by any party without dependence on the originating system's infrastructure. |
| Independent Witness | A party or system that captures or attests evidence from outside the monitored system's trust boundary, satisfying the structural independence requirement of Section 3. |

## Section 5 - The Five-Tier Corroboration Model

Every Proof Record passes through five corroboration states before sealing. The tiers are sequential and non-skippable; each is a necessary condition for the next.

| Tier | Label | Requirement |
|---|---|---|
| T1 | Activated | Pre-execution commitment generated and anchored to an independent randomness source. |
| T2 | Committed | Execution environment and parameters locked and signed. |
| T3 | Witnessed | Execution telemetry captured by at least one independent witness outside the executing system's trust boundary. |
| T4 | Analyzed | Outcome evaluated against the pre-execution commitment by an independent process. |
| T5 | Sealed | All evidence aggregated, hashed, cryptographically signed, and committed to a public ledger. |

A claim is only valid against a Sealed (T5) Proof Record. A Proof Record that has not received independent attestation is not Sealed regardless of internal completeness.

## Section 6 - Pre-Execution Commitment

Before any execution begins, the parameters of that execution are committed to an independent, publicly verifiable randomness or timestamp source. This eliminates retroactive modification: the record of what was intended to happen exists before the outcome is known, and cannot be authored after the fact to match whatever actually occurred.

The reference implementation anchors to the NIST Randomness Beacon — federal infrastructure operated by the National Institute of Standards and Technology, publishing a new cryptographically signed random value at fixed intervals. A commitment referencing a Beacon pulse cannot have been constructed before that pulse was published, which makes the commitment's timing independently verifiable by any party.

## Section 7 - Independent Witnessing

A witness satisfies the structural independence requirement of Section 3 only if all of the following hold:

- **Operational independence** — the witness is operated by a party with no financial or reputational interest in the proof outcome.
- **Infrastructure independence** — the witness runs on infrastructure the monitored system cannot access, modify, or terminate.
- **Epistemic independence** — the witness has no knowledge of the monitored system's intended outcome before execution.
- **Instructional independence** — the witness operates under instructions authored before the proof event that the monitored system cannot modify.

Failure of operational or infrastructure independence is a permanent disqualification for that witness. Failure of epistemic or instructional independence is remediable by resetting with a new pre-execution commitment.

## Section 8 - Ledger Anchoring

A Proof Record is anchored to a public, append-only ledger such that its existence and integrity are verifiable by any party without access to the originating system's infrastructure. This specification does not mandate a specific ledger technology. The reference implementation, ProofRegister™, is not a blockchain: no token, no consensus mechanism, no transaction fees. Its anchoring guarantee derives from the pre-execution commitment (Section 6) and independent witnessing (Section 7), not from distributed consensus.

## Section 9 - Cryptographic Integrity vs. Structural Independence

Cryptographic signing, hashing, or enclave-based attestation establishes that an artifact was not altered after creation. It does not establish that the artifact is an honest account of what occurred, because a system that both executes an action and signs the record of that action remains within its own trust boundary.

A record can be fully valid cryptographically and still fail the Valid Proof Test. Known failure modes that cryptographic-integrity-only models do not address include:

- **Replay** of a previously valid signed artifact as evidence of a new event.
- **Prompt injection or intent drift** producing an honestly-signed but improperly-induced action.
- **Compromised orchestration** signing compromised telemetry.

A conformant implementation MUST distinguish, in its own terminology and conformance claims, between records that satisfy the Valid Proof Test ("proof") and records that carry cryptographic integrity alone ("attestation"). A conformance program that treats the two as equivalent is not Proof Protocol-conformant.

## Section 10 - Conformance Requirements for Domain Protocols

A domain protocol claiming conformance with this specification MUST:

1. Define its own Atomic Execution Unit appropriate to its domain.
2. Implement pre-execution commitment per Section 6, anchored to an independently verifiable, publicly retrievable source.
3. Require at least one independent witness per Section 7 for any record claiming T3 (Witnessed) or higher.
4. Implement the five-tier corroboration model per Section 5, or an equivalent tier structure that preserves the same sequential, non-skippable property.
5. Distinguish attestation from proof in its own conformance language, per Section 9.
6. Anchor sealed records to a publicly queryable ledger per Section 8.

A domain protocol MAY define additional domain-specific schema, roles, and mechanics beyond this specification's scope, provided the above requirements are satisfied.

## Section 11 – Structural Independence Requirements

Proof Protocol conformance is defined independently of any external
framework's tiering:

| Requirement | Proof Protocol Standard |
|---|---|
| Self-declared claims | Structurally excluded. A Valid Proof Event requires independent-witness attestation per Section 7. Self-declaration cannot satisfy structural independence. |
| Point-in-time third-party assessment | Satisfied at T3–T4. Point-in-time, independently witnessed assessment. |
| Continuous, real-time attestation | Satisfied by a Valid Proof Stream (Section 4) with per-AEU attestation in real time, live-queryable, continuously chained. |
 
## Section 12 - Prior Art and Category Provenance

Craig Ellrod has been proving system and device behavior under defined test conditions since 2004. The System Under Test (SUT) and Device Under Test (DUT) methodology — documented in *Technical Marketing* (2004) — is the foundational prior art from which this specification's Atomic Execution Unit and pre-execution commitment architecture emerge.

- **2004:** SUT/DUT test methodology developed and documented.
- **2004–2025:** Thirty-plus years of adversarial validation practice across Cisco, Armor Defense, JupiterOne, and Cequence Security.
- **May 2025:** HACKERverse® formalizes the Proof Economy™ category and the Continuous Adversarial Evaluation™ (CAE) category.
- **March 24, 2026:** Gartner releases the Adversarial Exposure Validation (AEV) category — approximately ten months after Proof Economy™ and CAE coinage.
- **2026:** DKP published as the reference domain implementation of this specification (see DKP-SPEC-001).

## Section 13 - Certification

This specification is published under Creative Commons Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0). Any party may implement, reference, or build upon it with attribution. The canonical text itself may not be redistributed in modified form, in order to preserve a single, unfragmented reference standard — this does not restrict the right to implement, build products against, or certify conformance to the architecture it defines.

Certification of a domain protocol's conformance to this specification — including the right to carry the ProofStamp™ mark — is retained by Nebulonium, Inc., dba HACKERverse®. The protocol is open. The stamp is not.

## Anchoring and Verification

### GitHub Commit Anchor

- Repository: `proofprotocol/Proof-Protocol-Specification`
- Commit timestamp: logged by GitHub at moment of push
- Permanent archive: Zenodo DOI to be minted on next version publication

### NIST Randomness Beacon Anchor

> ⚠️ **Do not fill this in from memory or reuse a prior pulse.** Retrieve a live pulse at https://beacon.nist.gov/beacon/2.0/pulse/last at the moment you're ready to commit this document, and paste the actual JSON response here — the same way every other PP-SPEC document does it. A fabricated or reused pulse value defeats the entire evidentiary purpose this section exists to serve.

```json
[PASTE LIVE NIST BEACON PULSE JSON HERE AT COMMIT TIME]
```

## Authorship

This specification was authored solely by Craig Ellrod, Founder and CEO of Nebulonium, Inc. (d/b/a HACKERverse), drawing on thirty years of professional practice in offensive cybersecurity, adversarial system evaluation, and technical standards development. No external party contributed to, reviewed, or approved this specification prior to publication.

---

*This document may be freely shared, referenced, and built upon with attribution. Certification rights are retained by HACKERverse®.*
