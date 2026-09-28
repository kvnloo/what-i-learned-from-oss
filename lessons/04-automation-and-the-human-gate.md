# Automation and the human gate

Automation is valuable when it removes mechanical work.

It becomes harmful when it automates judgment that the system cannot reliably make.

## Good things to automate

Machines are excellent at:

- repository inventory;
- issue and PR search;
- duplicate detection;
- dependency and ownership mapping;
- CI status collection;
- reproducible test execution;
- benchmark matrices;
- formatting and linting;
- branch cleanup;
- evidence capture;
- stale-head detection;
- change summaries;
- checking whether a known invariant still holds.

These are repeatable and externally verifiable.

## Keep humans on high-consequence boundaries

Slow down for:

- public claims;
- architecture changes;
- security/privacy implications;
- backwards compatibility;
- maintainer disagreement;
- ambiguous ownership;
- destructive operations;
- migrations;
- policy interpretation;
- performance conclusions;
- deciding whether evidence is sufficient.

AI can help construct the experiment.

A human should own what the experiment means.

## Do not automate noise

A contribution factory can become a spam factory.

A useful gate is:

> If this action adds no new information for the maintainer, do not publish it.

Avoid:

- comments that merely restate the issue;
- speculative solution dumps;
- duplicate PRs;
- "any update?" messages without a reason;
- automated reviews on repositories that do not want them;
- publishing ten weak ideas instead of one tested result.

Maintainer attention is a scarce resource.

Treat it as such.

## Reconcile external state before retrying

When a tool call, post, or workflow appears to fail, do not immediately repeat it.

First verify the actual external state.

The write may have succeeded even if the acknowledgement was lost.

This matters for:

- comments;
- issue creation;
- PR creation;
- merges;
- deploys;
- package publishing;
- infrastructure operations.

Missing confirmation means **unknown**, not necessarily **failed**.

Blind retries create duplicates.

## Automate receipts

For every automated action, retain enough information to answer:

- what was attempted?
- against which revision?
- what ran?
- what changed?
- what evidence was produced?
- was anything posted upstream?
- what remains unverified?

Automation without receipts is difficult to trust.

## Separate discovery from publication

A scalable system can autonomously:

- scan;
- reproduce;
- experiment;
- compare;
- draft;
- rank confidence.

Publication should pass through repo-specific policy and evidence gates.

For strict human-authorship repositories, the system should stop before prose generation for upstream submission and instead provide a plain-English lesson plus raw evidence.

## Simple mental model

**Automate repetition. Gate interpretation. Protect maintainer attention.**
