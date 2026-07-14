# BIPD-0002: BIPD Process

- **Number**: 0002
- **Authors**: Bitplanet Foundation
- **Type**: Meta
- **Status**: Draft
- **Created**: 2026-05-22
- **Updated**: 2026-07-02

## Summary

This document defines the Bitplanet Improvement Proposals & Documents (BIPD) process,
including scope, proposal types, lifecycle states, and governance authority during
D-089 Phase 1.

## Motivation

Bitplanet needs a durable, transparent, and reviewable specification process for protocol
and interface changes. BIPDs provide:

- A shared technical artifact for protocol and validator changes.
- Early review of compatibility and security implications.
- Historical traceability for why a change was made.

BIPDs complement, but do not replace, D-* canonical decisions in neweco.

## Specification

### What a BIPD Is

A BIPD is a design/specification document describing a protocol, process, or informational
change relevant to Bitplanet.

### Proposal Types

- **Standard**: Normative technical changes to protocol, runtime, validator behavior,
  networking, transaction validity, RPC interfaces, or interoperability.
- **Meta**: Process and governance changes for how BIPDs are created, reviewed,
  and maintained.
- **Informational**: Non-normative technical guidance, clarifications, and context.

### Lifecycle States

- **Draft**: Proposal is authored and iterated in public.
- **Review**: Proposal is complete enough for focused technical and governance review.
- **Accepted**: Proposal is approved as intended specification direction.
- **Implemented**: Corresponding implementation work is complete.
- **Rejected**: Proposal is not accepted.

### Submission Flow

1. Start with a BIPD discussion issue for early feedback.
2. Copy `proposals/0000-bipd-template.md` to `proposals/XXXX-short-title.md`.
3. Submit a PR against `main`.
4. Update the proposal number from `XXXX` to the assigned final number.
5. Iterate with reviewers until transition decision.

### Authority and Transitions (Phase 1)

Under D-089 Phase 1, Bitplanet Foundation maintainers hold BIPD status transition authority
under single proxy-vote governance.

- Authors can propose status changes in PR text.
- Maintainers apply status changes by merge and follow-up edits.
- Reviewers provide technical accept/reject recommendations.

Future phases are expected to move authority to validator + cores governance.

### Relationship to Canonical Decisions

- D-* canonical decisions are the source of truth for foundational ecosystem/economic policy.
- BIPDs are the source of truth for technical protocol/process specifications.
- BIPDs should reference canonical decisions where policy parameters are relevant.

### Implementation Path

Accepted Standard BIPDs are expected to map to implementation plans in relevant Bitplanet
repositories. On-chain protocol behavior ultimately ships through the
Bitplanet-SolanaAgave-v3 validator client.

## Rationale

A lightweight, explicit lifecycle improves coordination between protocol authors,
validator implementers, and ecosystem stakeholders. Keeping policy-level decisions in
neweco and technical specs in BIPD avoids duplication and ambiguity.

## Backwards Compatibility

This process introduces a new repository and does not alter existing deployed protocol
behavior by itself.

## Security Considerations

Process ambiguity can lead to unsafe implementations. Requiring explicit specification,
review, and status transitions reduces risk of accidental consensus divergence.

## Drawbacks

- Additional documentation and review overhead.
- Possible latency between proposal and implementation.

## Alternatives

- Ad hoc design discussions only: insufficient traceability and poor long-term discoverability.
- Tracking all technical design in canonical decisions: mixes policy and protocol concerns.

## References

- Bitplanet canonical decisions:
  https://github.com/Bitplanet-L1/neweco/blob/main/CANONICAL-DECISIONS-LOG.md
- SIMD reference repository:
  https://github.com/solana-foundation/solana-improvement-documents
