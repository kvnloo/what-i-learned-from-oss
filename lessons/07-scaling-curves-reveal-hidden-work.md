# Scaling curves reveal hidden work

A single benchmark tells you how fast a system is at one point.

A scaling curve tells you **how the system works**.

This matters because many systems are designed around a promise such as:

- bounded work per request;
- constant-size lookup;
- fixed top-k selection;
- incremental updates;
- cached reuse;
- sparse computation;
- amortized work.

If runtime grows with input size when the architecture says it should remain roughly flat, that is a clue that some supposedly bounded path has quietly become proportional to total state.

## Read the shape before reading the code

Suppose generation speed degrades as context grows:

- short context: fast;
- medium context: slower;
- long context: much slower.

Do not begin with "which line is slow?"

Begin with:

> What operation is growing with context length that theoretically should not be?

That question narrows the search dramatically.

## Complexity is an architectural contract

If a subsystem claims to select a fixed number of relevant items, then downstream work should usually scale with that fixed number, not the entire history.

For example:

- a top-k attention path should behave roughly like O(k), not O(n);
- an incremental cache should update changed entries, not rebuild all entries;
- a scheduler optimization should avoid rescanning every candidate when a filtered subset is sufficient.

The implementation can produce correct answers while still violating the intended complexity contract.

That is a performance bug, not merely an optimization opportunity.

## Compare against a control architecture

A powerful experiment is to compare the suspect subsystem against a nearby implementation that does not contain it.

If both degrade, the problem may be shared infrastructure.

If only one degrades, the search space becomes much smaller.

Controls can be:

- another model architecture;
- another backend;
- another data type;
- sparse vs dense;
- cached vs uncached;
- fused vs unfused.

## Profile categories, not just functions

When performance scales badly, separate costs into broad buckets:

- compute;
- memory movement;
- allocation;
- synchronization;
- host-to-device transfer;
- device-to-device transfer;
- repeated preprocessing;
- sorting/search;
- cache maintenance.

A flame graph or profiler trace is most useful when connected back to an architectural hypothesis.

## Repeated preprocessing is a common hidden O(n)

Caches often fail this way.

A design may cache the final data but still recompute some metadata over the entire cache on every step.

Examples:

- rebuilding an index;
- re-pooling all cached blocks;
- re-normalizing unchanged values;
- regenerating masks;
- sorting an entire candidate set;
- copying the full lookup table back to the device.

The data is cached, but the work around the cache is not.

## Stage fixes by leverage and risk

When several sources of scaling cost are found, prefer an ordered series:

1. enable an already-intended fast path;
2. remove obviously unnecessary full-state work;
3. shrink the working set;
4. cache derived state;
5. move repeatedly transferred metadata closer to the consumer;
6. redesign APIs only if the remaining bottleneck requires it.

This keeps each claim testable and reviewable.

## Validate semantics after performance changes

Performance fixes often alter:

- ordering;
- masking;
- cache invalidation;
- sparse selection;
- reuse;
- concurrency.

A fast result is meaningless if it silently changes behavior.

Use targeted correctness tests that stress the exact risk introduced by the optimization.

## Simple mental model

**When runtime grows with input size, compare that growth against the complexity the architecture promised. The mismatch often tells you where the hidden work lives.**
