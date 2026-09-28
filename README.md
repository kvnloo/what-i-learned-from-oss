# what-i-learned-from-oss

A compact, public-safe vault of lessons learned by contributing across mature open-source projects.

The goal is not to collect fixes. It is to preserve the reasoning patterns that transfer between repositories: how to enter an unfamiliar codebase, find the real invariant, produce trustworthy evidence, move quickly without wasting maintainer time, automate the mechanical parts, and turn every contribution into durable technical understanding.

## Lessons

1. [Enter the repo before changing it](lessons/01-enter-the-repo-before-changing-it.md)  
   Repository culture, ownership, contribution rules, collision avoidance, and human-authorship boundaries.

2. [Evidence before opinion](lessons/02-evidence-before-opinion.md)  
   Reproducers, RED/GREEN tests, negative controls, benchmarks, measurement receipts, and claim discipline.

3. [Developer velocity without sloppiness](lessons/03-dev-velocity-without-sloppiness.md)  
   Small mergeable slices, downstream experimentation, draft discipline, CI/CD interpretation, and fast feedback loops.

4. [Automation and the human gate](lessons/04-automation-and-the-human-gate.md)  
   What to automate, what requires judgment, preventing duplicate writes, protecting maintainer attention, and keeping receipts.

5. [Open source as a learning system](lessons/05-open-source-as-a-learning-system.md)  
   Repo lessons, architecture lessons, plain-English understanding, invariant libraries, and turning contributions into expertise.

6. [Review and maintainer attention](lessons/06-review-and-maintainer-attention.md)  
   Reviewing invariants instead of diffs, strong counterexamples, preserving credit, asking before prescribing, and learning from maintainer feedback.

7. [Scaling curves reveal hidden work](lessons/07-scaling-curves-reveal-hidden-work.md)  
   Complexity contracts, hidden O(n) work, control architectures, profiling categories, and staged performance fixes.


## Core loop

```text
scan
  ↓
read local rules
  ↓
dedupe / check ownership
  ↓
understand the subsystem
  ↓
reproduce
  ↓
identify the invariant
  ↓
design the smallest decisive test
  ↓
experiment downstream
  ↓
verify + capture evidence
  ↓
human review
  ↓
upstream
  ↓
monitor feedback / CI
  ↓
clean up + record the lesson
```

## North star

> Move fast by shrinking uncertainty, not by skipping verification.

A useful contribution should leave both the codebase and the contributor with a smaller unknown than before.
