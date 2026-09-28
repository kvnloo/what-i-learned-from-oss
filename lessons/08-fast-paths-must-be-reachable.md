# A fast path only matters if real workloads reach it

A microbenchmark can prove that a kernel, function, or algorithm is faster in isolation.

It cannot prove that the application becomes faster.

This distinction matters in mature systems because optimized code is usually guarded by dispatch conditions.

## The real performance pipeline

A production workload often flows through:

1. application configuration;
2. model or data shape;
3. feature flags;
4. dispatch logic;
5. fast-path eligibility checks;
6. the optimized kernel;
7. surrounding operations;
8. end-to-end output.

An optimization at step 6 has zero practical value if ordinary workloads fail at step 5.

## Reachability before speed

Before benchmarking an optimized path, prove:

- the workload can reach it;
- the relevant conditions are satisfied;
- no earlier operation transforms the input so the path becomes irrelevant;
- the benchmark actually includes the operation being optimized.

A benchmark that bypasses the production path can answer the wrong question extremely precisely.

## Microbenchmarks and end-to-end benchmarks answer different questions

A microbenchmark asks:

> Is this component faster when I force it to run?

An end-to-end benchmark asks:

> Does a real user-visible workload become meaningfully faster?

Both are useful.

Neither substitutes for the other.

The sequence should usually be:

1. microbenchmark to establish local improvement;
2. instrumentation to prove real-path execution;
3. end-to-end benchmark to establish practical value.

## Amdahl's law is a prioritization tool

If a component consumes 1% of total runtime, making it twice as fast cannot halve total latency.

The theoretical maximum improvement is bounded by how much time the workload spends there.

This is why a dramatic kernel benchmark can produce an invisible application-level result.

Before investing heavily, estimate:

> What fraction of total runtime can this optimization possibly affect?

## Dispatch logic is part of architecture

Fast paths often require conditions such as:

- specific tensor shapes;
- no mask;
- a particular scale;
- supported data types;
- supported hardware;
- a feature being enabled or disabled;
- a sufficiently large problem size.

These conditions are not incidental details.

They define the actual performance envelope of the system.

Understanding dispatch is often more important than understanding the optimized kernel itself.

## Benchmark the workload that preserves the target operation

A seemingly realistic benchmark can accidentally remove the expensive work.

Examples:

- an early top-k shrinks a vocabulary before a large softmax;
- Flash Attention replaces a non-fused attention path;
- caching eliminates the computation under study;
- batching changes which implementation is selected;
- a benchmark excludes sampling entirely.

Always trace the workload before trusting its relevance.

## Instrumentation beats assumption

When possible, verify execution directly with:

- counters;
- profiler traces;
- temporary logging;
- kernel names;
- branch counters;
- backend traces.

Do not infer "the fast path ran" merely because the configuration sounds right.

## Optimize opportunity cost too

Maintainer review time is finite.

Before asking for review of a performance patch, provide evidence that:

- the path is reachable;
- a real workload exercises it;
- the workload-level gain is measurable enough to justify complexity.

A technically valid optimization can still be a poor trade if it creates permanent maintenance cost for negligible real benefit.

## Simple mental model

**First prove the road is used. Then prove making the road faster matters to the trip.**
