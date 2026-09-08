# ADR-007 — Apparent survival is not survival

**Date:** 2026-05 · **Status:** Accepted

## Context

Capture–mark–recapture estimates a parameter conventionally written φ. It is the
probability that an animal marked in one session is available for capture in the next.

An animal fails that condition in two ways: it died, or it left. The model cannot separate
them, because both produce identical data — the animal is never seen again. The parameter
is therefore **apparent survival**, and the qualifier is not a technicality.

For most study species the gap is small and the shorthand is harmless. Here it is neither.

The animal spends most of its life underground. The literature going back to its original
description proposes that surface activity is episodic and tied to rainfall rather than to
a fixed breeding season. An individual absent from an entire primary session is at least as
likely to be in a refuge as to be dead.

So a low φ between sessions has two readings that are biologically opposite:

- **high mortality** — a population in trouble, a conservation alarm;
- **animals underground** — a population behaving exactly as its natural history predicts.

The data alone cannot distinguish them. And the site is an endorheic basin, geographically
closed, so permanent emigration out of the system is implausible — which makes temporary
absence the most likely explanation, and makes the mortality reading the *less* supported
one.

The temptation is straightforward. "Survival" is shorter, reads more strongly, and a low
survival estimate in a threatened species is a more publishable and more fundable finding.

## Decision

**Name the parameter for what the model can identify, and state the confound where the
parameter is defined.**

- The parameter is **apparent survival** in the code, in the outputs, and in the
  manuscript. Never "survival" unqualified.
- The analysis script's header carries the biological context: the underground life
  history, the closed basin, the published hypothesis about episodic surface activity, and
  the explicit statement that low apparent survival is interpreted as animals returning to
  refugia rather than as mortality.
- The interpretation is stated as an interpretation, with its basis, rather than presented
  as a finding.

The reasoning lives beside the model definition ([ADR-005](ADR-005-data-decisions-live-in-the-code.md))
because the parameter and its meaning must not be edited independently.

## Consequences

**What was gained**

- The paper claims what the data supports and no more.
- The confound is visible to readers who might otherwise cite an estimate as a mortality
  rate — a real risk in conservation literature, where numbers get reused far from their
  original context.
- A conservation recommendation built on this work rests on a correctly-labelled quantity.
- Naming the confound explicitly is what made simulating within-session emigration
  ([ADR-004](ADR-004-simulate-your-own-bias.md)) an obvious next step rather than an
  afterthought.

**What it cost**

- **A weaker-sounding result.** "Low apparent survival, interpreted as refuge use" is less
  striking than "low survival", and in a threatened species the striking version is more
  publishable and more fundable. That is a real cost and it is the entire reason the
  shorthand is common.
- **More text**, in a manuscript with a word limit, spent on what a parameter does not mean.
- **The interpretation is still an argument.** Refuge use is better supported than mortality
  given a closed basin and the natural history, but it is inference from context, not
  demonstration. Distinguishing them properly would need a different study design —
  telemetry, or year-round sampling.
- **It constrains future claims**, since having named the confound, it cannot be quietly
  dropped in a later paper.

The transferable point is about the boundary between a computation and a claim. The model
returns a number; converting that number into a statement about the world is a separate
step that the software does not perform and cannot check. **Most serious analytical errors
are not arithmetic — they are a quantity being described as something it is not**, which is
the same failure as reporting reprojection error as accuracy in
[survey work](https://github.com/nicolaskass/geospatial-data-processing). The arithmetic is
rarely the hard part. Knowing what the answer is an answer to is.
