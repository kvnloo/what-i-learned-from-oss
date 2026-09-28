# Review and maintainer attention

A reviewer is not there to demonstrate cleverness.

The job is to reduce the probability that a change harms users or creates future maintenance cost.

## Review the invariant, not the diff

A line-by-line review can miss the important question.

Start with:

> What property did the old system protect, and does the new implementation still protect it?

Then inspect the code.

This catches failures such as:

- preserving output while breaking ordering;
- fixing starvation while increasing total resource consumption;
- adding a fast path that silently skips unsupported plugins;
- passing a test through timing luck;
- correcting one backend while breaking another.

## Strong counterexamples beat long comments

A small concrete scenario is often the best review artifact.

Examples:

- three operations whose order matters;
- two clients that unexpectedly share mutable state;
- a buffer surrounded by canary values;
- one unsupported plugin in an otherwise optimized set;
- one normal workload alongside the optimized workload.

A counterexample forces the discussion onto observable behavior.

## Ask before prescribing

When you are not certain about an invariant, phrase the review around the uncertainty.

Useful:

> "Is this ordering no longer required because the relevant state is fully captured per item?"

Less useful:

> "This is wrong. Restore the old order."

Maintainers may know constraints you do not.

The goal is to converge, not win.

## Preserve credit

If your investigation builds on another contributor's report or patch:

- link it;
- test it;
- extend it;
- avoid presenting the idea as newly discovered.

OSS is cumulative work.

Credit is part of healthy collaboration.

## Respect attention budgets

Before posting, ask:

- Is this new information?
- Is it actionable?
- Is the evidence strong enough for the confidence of the wording?
- Is this the right thread?
- Can it be shorter?
- Would a test or patch communicate this better than prose?

Sometimes the best comment is no comment.

## Treat maintainer feedback as training data

When a maintainer corrects:

- scope;
- terminology;
- preferred tests;
- ownership;
- tone;
- architecture;
- release expectations;

turn that into a local repository rule.

The next contribution should demonstrate that the lesson was learned.

## Simple mental model

**A good review leaves the maintainer with less uncertainty than before they read it.**
