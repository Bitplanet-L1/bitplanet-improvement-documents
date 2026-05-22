# Contributing to BIPD

This repository exists to document Bitplanet protocol and process specifications.
For foundational ecosystem/economic policy, refer to
[Bitplanet canonical decisions](https://github.com/Bitplanet-L1/neweco/blob/main/CANONICAL-DECISIONS-LOG.md).

## How to Propose a BIPD

1. Fork this repository.
2. Copy [proposals/0000-bipd-template.md](./proposals/0000-bipd-template.md) to a new file:
   `proposals/XXXX-short-title.md`.
3. Fill out all required sections in the template.
4. Open a pull request against `main`.
5. Update `XXXX` to the final number assigned during merge preparation.

Before opening a PR, start with a discussion issue using
`.github/ISSUE_TEMPLATE/bipd-discussion.md` when the design is still early.

## Lifecycle Stages

- **Draft**
  Early proposal under active author iteration and community feedback.
- **Review**
  Proposal is complete enough for focused maintainer + domain expert review.
- **Accepted**
  Proposal is approved as the intended Bitplanet specification direction.
- **Implemented**
  Implementation is complete in the relevant Bitplanet codebases.
- **Rejected**
  Proposal is explicitly not adopted.

## Who Can Move State

Current authority model (D-089 Phase 1):

- Bitplanet Foundation maintainers control repository merges and status transitions.
- Proposal authors and contributors provide revisions and technical evidence.
- Domain reviewers (validator/client/network/runtime) provide review input and objections.

Future phases are expected to transition authority to validator + cores governance.
Until then, maintainers apply final status changes on behalf of the current proxy-vote authority.

## Review Expectations and SLAs

Target review timelines (best effort):

- Initial maintainer triage for new Draft PRs: within 5 business days.
- Initial technical review after status is set to Review: within 10 business days.
- Follow-up review after substantive author updates: within 7 business days.

These are targets, not guarantees. If timeline pressure exists, include rationale and
blocking dependencies in the PR description.

## Quality Bar

A high-quality BIPD should:

- Clearly describe motivation and affected Bitplanet components.
- Reference relevant D-* canonical decisions instead of duplicating policy values.
- Cover backwards compatibility, security considerations, and alternatives.
- Be implementable by Bitplanet maintainers without hidden assumptions.
