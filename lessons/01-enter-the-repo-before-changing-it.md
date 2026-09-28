# Enter the repo before changing it

The fastest way to waste time in open source is to solve the wrong problem correctly.

Before touching code, learn the local rules.

## The repo is a social system, not just a codebase

Every mature repository has several architectures at once:

- the software architecture;
- the ownership architecture;
- the review architecture;
- the release architecture;
- the unwritten social architecture.

A technically correct patch can still be wrong for the repository if it ignores one of these.

## First-pass checklist

Before proposing work:

1. Read `CONTRIBUTING.md`, issue templates, PR templates, `CODEOWNERS`, and local agent/AI guidance.
2. Search open and recently closed issues and PRs for the same problem.
3. Identify who owns the subsystem and how they prefer changes to arrive.
4. Read a few accepted PRs in the same area.
5. Check whether somebody is already assigned or actively working on it.
6. Learn the expected evidence: unit test, integration test, benchmark, reproducer, design discussion, or all of the above.
7. Check what must be human-authored or disclosed.

## Why contributor style matters

Different repositories optimize for different risks.

A fast-moving application may value quick, narrow iteration.

A compiler, kernel, scheduler, inference runtime, robotics stack, or database may value compatibility and strong evidence more than speed.

Do not import one repo's habits into another.

The universal rule is not "always be fast" or "always be cautious."

The universal rule is:

> Match the rigor of the contribution to the blast radius of being wrong.

## Search before build

A large fraction of useful OSS work is archaeology.

Before implementing a fix, answer:

- Has this happened before?
- Was there an older fix?
- Did a migration reintroduce it?
- Is a newer release already fixed?
- Is there another subsystem with a pattern we should reuse?
- Is somebody already implementing the same thing?

This often saves more maintainer time than writing code.

## Ownership is part of correctness

If another contributor owns the task, do not race them with a competing PR unless maintainers explicitly ask.

A better contribution may be:

- reproducing the bug;
- testing their patch;
- finding an edge case;
- adding missing regression coverage;
- benchmarking;
- reviewing compatibility;
- documenting a failure mode.

Improving existing work is often more valuable than replacing it.

## Human prose is a real interface

Some repositories prohibit AI-written public communication. Others permit assistance but require disclosure, verification, or personal authorship.

Treat public prose like code:

- know what every sentence means;
- narrow every claim to evidence;
- remove jargon you cannot explain;
- never submit generated text you could not defend in a review conversation.

## Simple mental model

Before changing a repository, learn:

**What does this project promise? Who protects that promise? What evidence convinces them?**
