# Bitplanet Improvement Proposals & Documents (BIPD)

BIPD is the public specification repository for Bitplanet protocol and ecosystem changes.
It mirrors the role of SIMDs in the Solana ecosystem, with Bitplanet-specific governance,
terminology, and implementation flow.

This repository tracks technical proposals in the form of Bitplanet Improvement Proposals
and Documents (BIPDs). The BIPD process is governed by
[BIPD-0002](./proposals/0002-bipd-process.md).

## Scope and Relationship to Canonical Decisions

Bitplanet keeps two complementary decision layers:

- **D-* Canonical Decisions**:
  Foundational ecosystem and economic decisions, maintained in the neweco repository:
  [CANONICAL-DECISIONS-LOG.md](https://github.com/Bitplanet-L1/neweco/blob/main/CANONICAL-DECISIONS-LOG.md).
- **BIPDs**:
  Technical protocol and interface specifications for Bitplanet.

When a proposal depends on policy, issuance, economics, or governance parameters,
it should reference the canonical decisions log instead of restating or inventing those values.
Bitplanet-native token semantics (BIP) should be described consistently with referenced
canonical decisions.

## BIPD Types

BIPDs can be one of:

- **Standard**:
  Protocol, validator, networking, runtime, transaction, RPC, or interoperability changes
  that affect most or all Bitplanet implementations.
- **Meta**:
  Changes to BIPD process, governance workflow, and specification tooling.
- **Informational**:
  Clarifications, guidance, and supporting technical documentation that does not define
  a normative protocol change by itself.

## Lifecycle

BIPDs move through the following statuses:

- **Draft**: Proposed and open for technical iteration.
- **Review**: Under focused review by maintainers and relevant contributors.
- **Accepted**: Decision made to include in Bitplanet protocol or process plan.
- **Implemented**: Required implementation work is complete.
- **Rejected**: Not adopted in current form.

See [CONTRIBUTING.md](./CONTRIBUTING.md) and
[BIPD-0002](./proposals/0002-bipd-process.md) for lifecycle details and transition rules.

## Governance Authority (Current Phase)

During D-089 Phase 1, BIPD state authority is held by the Bitplanet Foundation under
single proxy-vote authority. Future phases are expected to hand authority to
validator + cores governance as defined by canonical decisions.

## Specification Layer vs Client Implementation

BIPDs are the specification layer above client code.

On-chain protocol changes ultimately flow through the
Bitplanet-SolanaAgave-v3 validator client (Bitplanet's Agave fork), while BIPDs define
and document the intended behavior, rationale, and compatibility boundaries.

## Getting Started

- Read [BIPD-0002](./proposals/0002-bipd-process.md).
- Start discussion via issue using the BIPD discussion template.
- Copy the template: [proposals/0000-bipd-template.md](./proposals/0000-bipd-template.md).
- Submit a pull request with your proposed BIPD.
