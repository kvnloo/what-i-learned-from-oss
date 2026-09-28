# Developer velocity without sloppiness

Velocity is not "write more code."

Velocity is:

> reduce the time from uncertainty to a verified outcome.

That changes how work should be organized.

## Optimize the whole loop

A useful contribution loop is:

**scan → reproduce → deduplicate → model the subsystem → test → implement → verify → review → upstream → monitor → clean up**

Skipping early steps often creates more work later.

## Smallest mergeable slice

Prefer a change that:

- fixes one invariant;
- has one clear owner;
- includes focused evidence;
- avoids unrelated cleanup;
- can be reviewed independently.

Small changes reduce:

- merge conflicts;
- review load;
- regression surface;
- time spent debugging multiple causes at once.

A small patch is not automatically good, but it is easier to reason about.

## Fast downstream, conservative upstream

Experiment aggressively in your own fork.

Use it for:

- probes;
- instrumentation;
- benchmark harnesses;
- temporary assertions;
- alternative implementations;
- failure injection;
- ugly diagnostic code.

Then clean the upstream contribution down to the minimum durable change.

Your fork is a laboratory.

Upstream is shared infrastructure.

## Draft is a state, not a shame

A PR should stay draft when:

- the core claim is still unverified;
- important tests have not run;
- the design is still changing;
- you know a substantive gap remains;
- reviewer attention would be premature.

"Ready for review" should mean:

> I believe the evidence is strong enough that maintainer time is now the bottleneck.

## Finish live commitments before expanding the backlog

Before starting another wave, sweep existing work:

- maintainer replies;
- requested changes;
- CI failures;
- merge conflicts;
- stale branches;
- outdated approvals;
- missing tests;
- policy failures.

New contributions are exciting. Existing review feedback is usually higher priority.

## CI is a signal, not an oracle

Green CI tells you that the checks that ran passed.

It does not prove:

- the right tests exist;
- the failing environment was covered;
- runtime behavior is correct;
- browser/native/provider behavior was tested;
- a benchmark did not regress;
- an approval still applies to the current commit.

Likewise, red CI may reflect infrastructure rather than your patch.

Learn what each check actually covers.

## Preserve fast paths

When fixing a fallback or edge case, avoid degrading the healthy common path.

A strong patch often looks like:

- retain the existing optimized behavior;
- add a narrow guard;
- route only the unsupported case elsewhere;
- test both.

This is especially important in runtimes, schedulers, networking, rendering, and databases.

## DevOps lesson: reproducibility compounds

A good local harness becomes reusable infrastructure.

Keep:

- deterministic reproductions;
- benchmark scripts;
- environment captures;
- failure-injection fixtures;
- regression inputs.

They reduce future diagnosis time across many issues.

The goal is not merely CI/CD automation.

It is **shortening the distance between a claim and trustworthy evidence**.

## Simple mental model

**Move fast by shrinking uncertainty, not by skipping verification.**
