# BIPD-0458: Stop using static SimpleVote transaction cost

- **Number**: 0458
- **Authors**: Bitplanet Core (ported)
- **Type**: Standard
- **Status**: Draft
- **Created**: 2026-06-21
- **Updated**: 2026-06-21

## Summary

**Ported from Solana SIMD-0458.**

This proposal removes the use of statically defined compute unit (CU) costs for
SimpleVote transactions in the Bitplanet runtime. SimpleVote transactions MUST
be costed and accounted for through the same transaction cost model path as
normal transactions when enforcing block CU limits. As a result, the separate
vote CU limit becomes obsolete and MUST no longer be used once this change is
activated.

Because transaction cost accounting affects consensus behavior, this change
MUST be gated behind a feature flag.

## Motivation

SimpleVote transactions have historically reserved a fixed, statically defined
number of CUs for block cost tracking. That special case depended on the
assumption that the Vote program, as a builtin program, had a static execution
CU cost.

That assumption is no longer appropriate when Vote program execution is removed
from builtin cost modeling. SimpleVote transactions should instead be measured
and accounted for the same way as other transactions. Keeping a separate static
cost path would preserve an inaccurate special case, would make cost accounting
less uniform, and would require implementations to maintain a vote-specific
block limit that is no longer justified by the cost model.

## Specification

For the purposes of this BIPD, a SimpleVote transaction is a transaction that
the Bitplanet runtime classifies as a simple vote according to the existing
validator/runtime rules.

When the feature gate for this BIPD is inactive, validators MUST preserve the
existing SimpleVote cost accounting behavior.

When the feature gate for this BIPD is active, Bitplanet validators MUST apply
the following behavior:

- The runtime MUST stop using statically defined CU values for SimpleVote
  transaction cost tracking.
- The runtime MUST calculate SimpleVote transaction cost using the same cost
  model and accounting path used for normal transactions.
- Block CU limit enforcement MUST account for SimpleVote transactions using
  the calculated transaction cost, not the previous static SimpleVote cost.
- The separate vote CU limit MUST be removed from active block cost
  enforcement. SimpleVote transactions MUST be admitted or rejected by the same
  block CU accounting rules as other transactions.

The expected impact on SimpleVote CU consumption and reservation is:

| Behavior | Total consumed CUs | Total reserved CUs |
|:--|:--|:--|
| Current | 3,428 | 3,428 |
| Proposed | 3,428* | 19,812** |

\* The current statically defined SimpleVote cost is composed of one signature
at 720 CU, two write locks at 600 CU, one vote instruction at 2,100 CU, and
8 CU to load small accounts, for a total of 3,428 CU. Under the proposed model,
those components remain effectively the same for the vast majority of
SimpleVote transactions. The loaded account data size component may vary, and
therefore unusual SimpleVote transactions, such as transactions with two
signatures, additional writable accounts, or unusually large vote accounts, may
consume a different number of CUs.

\** Under the current cost model, SimpleVote transactions may have higher
estimated reserved CUs because default account data loading costs are included.
This may add about 16,000 CUs to the initial reservation, resulting in a total
reservation of about 19,812 CUs. Block-packing strategies that reserve CUs
upfront and refund unused CUs after execution may therefore reserve more CUs for
SimpleVote transactions before later refunding the unused portion. This effect
is specific to reserve-then-refund packing strategies.

This BIPD does not change the SimpleVote transaction format, Vote program
semantics, signature verification, lock acquisition, or transaction execution
rules. It changes only how SimpleVote transaction costs are calculated and
accounted for when enforcing block CU limits.

## Rationale

Using the normal transaction cost model for SimpleVote transactions makes cost
accounting more consistent and removes a vote-specific special case whose
underlying assumption no longer holds.

The expected executed CU consumption remains unchanged for ordinary SimpleVote
transactions because the prior static value was already an approximation of the
same signature, write-lock, vote-instruction, and small account-load costs.
Using the regular cost path is more accurate for less common SimpleVote
transactions whose account data size, writable account set, or signature count
differs from the assumptions embedded in the static value.

The separate vote CU limit is removed because SimpleVote transactions are no
longer accounted for through a separate static-cost path. A single block CU
accounting mechanism is easier for validators to implement consistently and
reduces the risk of divergence between vote and non-vote transaction handling.

Feature gating is required because block cost accounting affects which
transactions can be included in a block. Activating the change without a feature
gate could cause validators running different software versions to disagree on
block validity.

## Backwards Compatibility

This proposal is backwards compatible while the feature gate is inactive because
validators continue to use the existing SimpleVote static-cost behavior.

After activation, validators that have not implemented this BIPD may calculate
different block costs for blocks containing SimpleVote transactions and may
therefore disagree with upgraded validators about block validity. For that
reason, activation MUST follow Bitplanet's normal feature-gated rollout process
for consensus-affecting runtime changes.

No transaction wire format, RPC interface, account layout, Vote program API, or
client-side transaction construction behavior is changed by this BIPD.

## Security Considerations

The primary security consideration is consensus safety. Implementations MUST
ensure that all validators use identical SimpleVote classification, cost
calculation, reservation, refund, and block-limit enforcement behavior once the
feature gate is active.

Validators and block producers should account for the higher initial reservation
that can occur under reserve-then-refund block-packing strategies. If a block
producer does not handle refunds correctly, it may pack blocks less efficiently
than expected. Incorrect refund handling or inconsistent application of the
removed vote CU limit could cause valid blocks to be rejected or invalid blocks
to be produced.

This change does not alter authorization, vote signing, or Vote program state
transition rules. It also does not introduce new economic or governance
parameters.

## Drawbacks

Some block producers may initially reserve more CUs for SimpleVote transactions
than they did under the static-cost special case. In reserve-then-refund block
packing, this can reduce packing efficiency unless the producer correctly
refunds unused CUs after execution.

The change also removes an established special case, which requires careful
implementation and testing across the runtime, cost model, and banking stage
paths that currently distinguish SimpleVote transactions from other
transactions.

## Alternatives

One alternative is to keep the static SimpleVote cost. This preserves existing
behavior but retains an inaccurate special case after Vote program cost is no
longer modeled as a builtin static cost.

Another alternative is to update the static SimpleVote cost to a new constant.
That would avoid the immediate reservation increase for common SimpleVote
transactions, but it would continue to be inaccurate for SimpleVote transactions
whose account data, writable accounts, or signatures differ from the constant's
assumptions.

A third alternative is to keep a separate vote CU limit while also using the
normal transaction cost model. This would retain vote-specific block accounting
despite removing the static vote-cost path, adding complexity without improving
the accuracy of transaction cost calculation.

## References

- Solana SIMD-0458, "Stop special-casing of Vote CU cost":
  https://raw.githubusercontent.com/solana-foundation/solana-improvement-documents/main/proposals/0458-stop-using-static-simplevote-transaction-cost.md
- Solana improvement documents repository:
  https://github.com/solana-foundation/solana-improvement-documents
