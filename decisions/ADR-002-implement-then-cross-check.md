# ADR-002 — Implement the model, then cross-check it

**Date:** 2026-04 · **Status:** Accepted

## Context

Capture–mark–recapture under Pollock's Robust Design is a solved problem with mature
software. Established R packages fit it; general probabilistic programming frameworks
express it in a few dozen lines and sample it efficiently.

Writing the sampler by hand is, on its face, the worse engineering choice: more code, more
opportunity for error, and no novelty.

Three things argued the other way.

**The assumptions become code.** Using a package, the design's assumptions are
configuration — a model string, an argument. Implementing the likelihood forces each one to
be written explicitly, and an assumption you had to type is an assumption you can defend to
a reviewer.

**The model is small.** Three primary sessions, session-specific detection, two survival
intervals. This is tractable to implement and check, not a hierarchical model with
thousands of latent states.

**The extensions were the point.** Prior sensitivity, posterior predictive checks tailored
to this design, and a simulation of within-session emigration bias
([ADR-004](ADR-004-simulate-your-own-bias.md)) all needed direct access to the likelihood
and the sampler. Bolting them onto a package's interface would have been harder than
writing the sampler.

The risk is obvious: a hand-written sampler can be subtly wrong and still converge nicely
and produce plausible posteriors.

## Decision

**Implement the sampler, then validate it against an established independent
implementation.**

The implementation is adaptive Metropolis–Hastings, four chains, explicit warmup and
retained draws, with R̂ and effective sample size reported per parameter.

The validation is the load-bearing half: the fit is cross-checked against the
**maximum-likelihood equivalent from an established R package**, with the exact call
recorded in the script so anybody can repeat the comparison.

That comparison is strong evidence precisely because the two are independent — a different
language, a different inferential paradigm, a different author, a different decade. Two
implementations agreeing by coincidence is far less likely than one implementation being
wrong.

Convergence diagnostics are reported alongside, because they answer a different question:
R̂ says the chains agree with each other, and says nothing about whether they are sampling
the right posterior.

## Consequences

**What was gained**

- Every assumption of the design is explicit in code and defensible in review.
- The diagnostics and simulations that make the analysis strong were possible to write.
- Correctness has positive evidence, not just absence of complaint.
- Complete control over the sampler, so its behaviour can be explained rather than cited.

**What it cost**

- **Substantially more work** than calling a package, for a model that a package already
  fits.
- **Adaptive Metropolis–Hastings is less efficient** than modern gradient-based samplers.
  It is adequate at this scale and would not be for a larger model.
- **The cross-check validates agreement at a point**, not correctness in general. The
  reference is a maximum-likelihood fit; agreement between a posterior median and an MLE is
  reassuring but is not a proof, and it would not necessarily catch an error that only
  manifests in the tails.
- **A sampler I maintain.** A package's bugs get found by thousands of users; mine get
  found by me.
- **It is the wrong default.** For a model of any real complexity, a probabilistic
  programming framework is the correct choice, and choosing to hand-roll would be
  self-indulgent rather than rigorous.

The general judgement: **implement when implementing teaches you something you need, and
then prove it against something you did not write.** The first half alone is
overconfidence; the second half is what converts the exercise into evidence.
