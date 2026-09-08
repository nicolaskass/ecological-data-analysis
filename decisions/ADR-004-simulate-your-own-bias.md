# ADR-004 — Simulate the estimator's own bias

**Date:** 2026-05 · **Status:** Accepted

## Context

Pollock's Robust Design assumes the population is **closed within** a primary session — no
births, deaths, immigration or emigration across the few consecutive days of a session.
That assumption is what allows abundance and detection probability to be separated.

For this species it is doubtful in a specific way. The animal spends most of its life
underground and surfaces in bursts tied to weather rather than to a fixed calendar. An
individual can plausibly return to a refuge partway through a session — which is
emigration, temporary but indistinguishable from permanent within the session.

If that happens, the estimator does not merely become uncertain. It becomes **biased in a
particular direction**: animals that leave partway through appear to have low detection
probability, and low estimated detection inflates estimated abundance.

The conventional treatment is a sentence in the discussion: *"the closure assumption may be
violated, and estimates should be interpreted with caution."* It is honest and it is nearly
useless. The reader cannot tell whether the concern moves the estimate by two percent or
doubles it, and neither can the author.

## Decision

**Simulate the violation and measure the bias.**

A simulation generates capture data under varying degrees of within-session emigration,
runs the same estimator on each, and produces a curve: bias in estimated abundance as a
function of how badly the assumption fails. It runs with a fixed seed and its output ships
with the reproducibility package as a figure and as recorded values.

The caveat becomes a magnitude. The discussion can now state the direction of the bias, and
how large the violation would have to be before the conclusion changes.

Alongside it, a **closure test** provides external evidence about whether the assumption is
violated at all, and a **capture-frequency test** checks for individual heterogeneity in
catchability — a different assumption whose violation biases in the same direction, and
which would otherwise be confounded with this one.

The point is not to prove the assumption holds. It is to know what follows if it does not.

## Consequences

**What was gained**

- The claim can be calibrated: not "estimates may be biased" but how much, in which
  direction, and under what magnitude of violation.
- A reviewer raising the closure assumption is answered with a figure rather than a
  concession.
- The conclusion's robustness becomes a stated property.
- Two related assumptions are separated instead of confounded.

**What it cost**

- **Real work for a result that weakens your own paper.** The output of this exercise is,
  at best, a number that qualifies your headline estimate. There is no incentive to do it,
  which is exactly why it is uncommon.
- **The simulation embeds its own assumptions** about how emigration occurs — a rate, a
  pattern. Reality may violate closure in a way the simulation does not model, so the bias
  curve is *a* quantification, not *the* quantification.
- **It invites the objection it answers.** Publishing a bias analysis draws a reviewer's
  attention to a limitation they might not have raised. That is the right trade and it is
  not a comfortable one.
- **Compute and code** for a diagnostic that is not the analysis.

The general principle, and the one I keep arriving at from different directions: **a stated
limitation constrains nothing; a measured one constrains what you may claim.** The same
reasoning turned a single optimistic uncertainty figure into two honest bounds in my
[geospatial work](https://github.com/nicolaskass/geospatial-data-processing). Writing the
caveat is the cheap half. Measuring it is what makes the rest of the paper trustworthy.
