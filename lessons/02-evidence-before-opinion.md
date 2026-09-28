# Evidence before opinion

The most reusable lesson from reviewing bugs and performance work is that observations, explanations, and fixes are three different things.

Do not collapse them.

## Start with the observable failure

Write down what can actually be seen:

- wrong output;
- crash;
- slowdown;
- memory increase;
- timeout;
- race;
- inconsistent behavior;
- test flake;
- confusing error;
- missing callback.

Then separate that from the hypothesis about why it happens.

A useful review often sounds like:

> "This result is real. I am less certain about the proposed explanation."

That distinction prevents premature redesign.

## The five-question method

For almost any issue:

1. **What does the system promise?**
2. **What shortcut, optimization, or state transition is involved?**
3. **What hidden assumption makes that shortcut safe?**
4. **What is the smallest counterexample to that assumption?**
5. **What is the smallest test that proves the invariant?**

This works across inference engines, schedulers, compilers, robotics, databases, UI systems, and libraries.

## Correct output is not enough

A program can produce the right visible output while still:

- writing outside its buffer;
- mutating shared configuration;
- leaking memory;
- using the wrong fallback path;
- consuming twice the intended resource budget;
- changing behavior only under compilation;
- breaking another backend;
- relying on timing luck.

The test should target the property that must remain true, not merely today's output.

## RED/GREEN is stronger than "test passes"

A good regression test should:

1. fail on the known-bad code;
2. pass on the proposed fix;
3. isolate the intended invariant;
4. avoid depending on unrelated timing or implementation details.

A test that only passes after a fix may not prove anything if it also passed before.

## Use negative controls

When measuring a fix, include a case that should *not* change.

Examples:

- a flat surface when fixing tilted-surface bias;
- ordinary pods while optimizing grouped scheduling;
- an unaffected backend while changing device-specific logic;
- an unmodified fast path while introducing a fallback;
- a small-context workload while studying long-context scaling.

A negative control answers:

> Did we fix the target, or merely perturb the whole system?

## Evidence must match the claim

If the claim is about correctness, provide a reproducer and regression test.

If the claim is about performance, provide before/after measurements.

If the claim is about a race, exercise chronology and repeated interleavings.

If the claim is about compatibility, test the old supported path.

If the claim is about end-to-end behavior, a unit test alone is insufficient.

## Never overstate test coverage

Say exactly what ran.

Good:

> "The focused CPU regression passes. CUDA and SYCL were not validated."

Bad:

> "The fix is verified."

Precision builds trust.

## Measurement receipts

For performance and systems work, record enough context to reproduce the result:

- commit;
- hardware;
- software/runtime versions;
- model or workload;
- configuration;
- warmup;
- sample count;
- correctness check;
- latency/throughput;
- memory;
- relevant counters.

A benchmark without its environment is often just a story.

## Simple mental model

**Separate what happened, why you think it happened, and what your experiment actually proves.**
