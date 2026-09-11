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

## Section 11 - Relationship to POCI Conformance Levels

| POCI Level | Proof Protocol Mapping |
|---|---|
| Level 1: Self-Declared | Structurally excluded. A Valid Proof Event requires independent-witness attestation per Section 7. Self-declaration cannot satisfy structural independence. |
| Level 2: Third-Party Assessed | Satisfied at T3–T4. Point-in-time, independently witnessed assessment. |
| Level 3: Continuously Monitored | Satisfied by a Valid Proof Stream (Section 4) with per-AEU attestation in real time, live-queryable, continuously chained. |

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

- Repository: `proofprotocol/Proof-Protocol-Specification` *(or wherever this is committed — currently the generic "Proof Protocol Specification" slot in the Specification Suite table points at the DKP repo; that link needs to be repointed here once this is committed)*
- Commit timestamp: logged by GitHub at moment of push
- Permanent archive: Zenodo DOI to be minted on next version publication

### NIST Randomness Beacon Anchor

> ⚠️ **Do not fill this in from memory or reuse a prior pulse.** Retrieve a live pulse at https://beacon.nist.gov/beacon/2.0/pulse/last at the moment you're ready to commit this document, and paste the actual JSON response here — the same way every other PP-SPEC document does it. A fabricated or reused pulse value defeats the entire evidentiary purpose this section exists to serve.

```json
{
  "pulse" : {
    "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1936066",
    "version" : "2.0",
    "cipherSuite" : 0,
    "period" : 60000,
    "certificateId" : "87f27f431da3f584af6007fe045df13aaa81d831f335b6ee73f6334768f32d3ae10491e669b93a43e548b20370a6526c3def99643c25d8ad7bf5df95a3c2b45d",
    "chainIndex" : 2,
    "pulseIndex" : 1936066,
    "timeStamp" : "2026-09-11T01:50:00.000Z",
    "localRandomValue" : "2AF9EBF7E68FAB3624BA5177C09B848A9D099B12FDB303CDEE1979BA6783CAE0238A0EE4325A614B705DDD94DF9E5F7034A8FDBD7E21669E7A556A2675C0C9A1",
    "external" : {
      "sourceId" : "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "statusCode" : 0,
      "value" : "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
    },
    "listValues" : [ {
      "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1936065",
      "type" : "previous",
      "value" : "10C1179B2DEAA0CAE9A881363F6C802E542393B6D973122AB90E43C2C88EF560701A22CDF0CAF66316586D8150073A795C217547BD314088FE7821C454A8F0EA"
    }, {
      "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1936023",
      "type" : "hour",
      "value" : "A6B4BC002C7D9D6EE670E0BA71CD29BC583EBAD9CFBF4EF823560A8F9C5F7CDC39CDED55203016C27E54E20E483BDF5E6749B7B06109924EB190898775BC7522"
    }, {
      "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1935986",
      "type" : "day",
      "value" : "144252ABE0F724691F601EEFDF385ADA072228D82FA62B5062B610ECD20B0B70D7F355FA19A417831591696566337617BC2FBABDFB19BA0219C77BA3358B5C88"
    }, {
      "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1921596",
      "type" : "month",
      "value" : "77EEA287E6D82ED376169156E6B5BCF8AA1D65E33806C13C79A6B180D9A833EACC2935FF7977C970D2436334ED923E8AEAEBB83362F0B6177CB08DD3A1F11AD3"
    }, {
      "uri" : "https://beacon.nist.gov/beacon/2.0/chain/2/pulse/1595005",
      "type" : "year",
      "value" : "A5FD82C3D2D3BD40D828416E16786CB12040BE747E0558CB834430D356760749B4DE671A660D6A4F16BBEBF1219A4376C14030F3D6A15CF26884B3244675159C"
    } ],
    "precommitmentValue" : "AD2E21A8102CE8F841AFC074F1D3730748A55AE76A1FEA82DB18D97080848B45E75DDCA6241C2DD6DC4ED6EECFD083505BDD7E238B7E1C63BB1A9E087A2D4C74",
    "statusCode" : 0,
    "signatureValue" : "4AD8B16C64D14662159D662A2AE2FE54D33D29B7240ACA43C6B1C5DF057F2FE88A9633937EF8DE409F9EB9E3B71EC6A6DE0CF245AD1F51E518B6928A58670258543675D3B3EE90BF4387784163EF51AC7D07BB2330E67A5D8D9A68F1E3E32E835BB460C70B3E89770BDEB5524566A133142C296F2F0C3ABF9F9E6BA09702991AB5732D02B567E7CC5A136275E9081EC91FC009B023312CD43D9E38E7A2E84DF48BDC83522C71945FFAD411D9DBAD9917D6AE6DA6526A7167DF689098326559836238F4122AF6253A980E392C33DAA31340A12311A3AECCD1A060516A4A69C968A95A7AB38CCF76D1441ABFA5632965422D1183DA09974FF7D62A25ABC054F921E1DB043A8B4AC0EE55EAF88FF8ABDE11240680CD15F6CCCBBBA381FE53073123C91297747220F07730786B5E429283A465A0D2632F2DD06A781A9DEDD5FA6019E44C0A5E84FAB011F336EBE035DE8743F3F1DAB36988DF8707C55A9BAE4FC645A43A81F5DA8D43B2AEABCE141FE58496CF180AD656EDD02DF4CA3809882F9A5556667DD8B183E2CDB202D22731F0AF9D2E9F544972A30B5AE85FE1F48D486D112E9E8119C3F87E0D4CB23DBAEFB88B793B2F88144447F14A6E0DFCB7998988BC8EA700205D06C0A617C93D1EFAABFE7A39DDDCA56CD2AE191216F6FAAA348286964E29AB064D67F8607EBCAC7C82FCA9E2B260DC5719DE1400EB07B6A3627AE6",
    "outputValue" : "BAB4D5CF0426DB44F15E84C2BA1969F2CA49225F5BE93AE5C8B41BED1AA586EB8E0F37B7691A8F8F191A5F31CA9BE11E67D6A4DDA296922202DC179A6C09BD7C"
  }
}
```

## Authorship

This specification was authored solely by Craig Ellrod, Inventor of the Proof Economy, Founder and CEO of Nebulonium, Inc. (d/b/a HACKERverse), drawing on thirty years of professional practice in offensive cybersecurity, adversarial system evaluation, and technical standards development. No external party contributed to, reviewed, or approved this specification prior to publication.

---

*This document may be freely shared, referenced, and built upon with attribution. Certification rights are retained by HACKERverse®.*
