# Shape is not memory layout

Two arrays can contain the same number of values and still require different code to traverse safely.

This is because there are two separate questions:

1. **What is the logical shape?**
2. **How are those values physically arranged in memory?**

Many low-level bugs come from answering only the first.

## Contiguous memory

The simplest case is a packed array:

```text
A B C D E F
```

The next logical value is also the next physical value.

This makes flat loops safe.

## Non-contiguous views

A tensor can present the same logical data through a different view:

```text
logical order: A B C D

physical storage:
A _ C _ B _ D
```

The underscores represent gaps or values belonging to something else.

A view can also reorder dimensions without copying the underlying data.

The logical shape may look ordinary while the physical access pattern is not.

## Strides describe the arrangement

A stride tells the program how far to move in memory to reach the next value along a dimension.

A contiguous tensor has the simplest strides.

A transposed, sliced, broadcast, or otherwise transformed view may not.

So:

> shape tells you how many values exist; strides tell you how to reach them.

## Same size does not imply safe flattening

A dangerous assumption is:

> "The allocation contains N elements, therefore I can process the next N memory locations."

That is only valid when the representation guarantees a gap-free layout suitable for that operation.

The correct condition depends on the algorithm.

Some operations only need:

- all values to occupy one gap-free allocation.

Others need:

- each row contiguous.

Others need:

- a specific dimension contiguous.

Others can handle arbitrary strides.

Do not use a stronger or weaker condition than the implementation actually requires.

## Match capability checks to implementation invariants

A runtime often has two layers:

1. a dispatch check that decides whether a backend can handle an operation;
2. the implementation itself, which assumes certain layout properties.

These must agree.

If the dispatcher is too strict, valid workloads fall back unnecessarily.

If the dispatcher is too permissive, the kernel may read incorrect memory or crash.

A useful review question is:

> What layout invariant does the implementation truly require?

Then make the capability check express exactly that invariant.

## Preserve the fast path

Supporting strided inputs does not mean slowing down contiguous inputs.

A common pattern is:

```text
if contiguous:
    use simple fast loop
else:
    use stride-aware loop
```

This adds correctness for unusual layouts while preserving performance for the common case.

## Reference implementations must support the test case

Accelerator tests often compare GPU output against a CPU reference.

If the CPU reference crashes on the same unusual layout, the GPU path cannot be meaningfully validated.

The test oracle is part of the test architecture.

A reference implementation does not need to be the fastest path, but it must correctly represent the supported semantics.

## Order-independent operations can permit broader layouts

Some operations, such as summation, do not care about element order.

That can make certain gap-free but reordered layouts safe for a flat reduction even when row-based operations would not be.

Do not generalize this property to operations where order or neighborhood matters.

## Simple mental model

**Never infer memory arrangement from shape alone. Ask how the next logical value is reached, and make the dispatch rule match exactly what the implementation can safely consume.**
