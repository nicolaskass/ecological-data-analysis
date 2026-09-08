# ADR-003 — Priors are justified from literature and tested for influence

**Date:** 2026-05 · **Status:** Accepted

## Context

The standard objection to Bayesian analysis in ecology is that the prior lets the analyst
choose the answer. It is usually unfair and occasionally correct, and the reason it keeps
landing is that priors are so often reported as a bare distribution with no justification
and no sensitivity analysis.

With a small dataset the objection has teeth. When data is abundant the prior washes out;
with a few hundred individuals over three sessions it does not necessarily, and detection
probability in particular is weakly identified.

The available external information was awkward. There is a published apparent-survival
estimate for a **congeneric** species — informative in kind, but:

- it is **annual**, while the intervals here are seven and ten weeks — an incommensurable
  timescale;
- the species differ in life history, and the animal here spends most of its time
  underground, which changes what "apparent survival" even measures
  ([ADR-007](ADR-007-apparent-survival-is-not-survival.md)).

Using it directly would import a number that does not transfer. Ignoring it would discard
the only relevant published information.

## Decision

**Justify each prior in writing, at the point of definition, and then measure how much it
matters.**

**Justification in the code.** Each prior carries a comment naming the published estimate
it derives from, and stating why it is deliberately broad: incommensurable timescale,
different life history. The centre reflects available knowledge; the width reflects how
badly that knowledge transfers. A reviewer can disagree with the specific reasoning rather
than objecting to priors in general.

**Weakly informative, not flat.** A flat prior on a probability is not neutral — it is a
strong statement on the logit scale, and it can be sharply informative about quantities
nobody intended to constrain. Weakly informative priors on the logit scale are the honest
choice.

**Sensitivity is measured, not asserted.** The entire model is re-fitted under a
deliberately different prior family, and the estimates are compared. If the conclusions
survive, that is reportable evidence; if they do not, that is a finding about the data's
weakness which the reader is entitled to.

**Shrinkage is quantified per parameter.** A prior–posterior shrinkage diagnostic on the
logit scale states how far the data moved each parameter from its prior — separating the
parameters the data actually determined from those still substantially reflecting the
prior.

Together these convert *"how much of this is your prior?"* from a rhetorical question into
a number.

## Consequences

**What was gained**

- The most predictable objection to the analysis is answered before it is raised, with
  evidence.
- Weakly-identified parameters are identified as such rather than presented with false
  confidence.
- The prior becomes a documented modelling decision, open to specific critique.
- Re-fitting infrastructure exists, which makes responding to a reviewer's alternative
  cheap.

**What it cost**

- **Every fit runs multiple times**, which is compute and complexity for a result that
  frequently just confirms the main analysis.
- **Two prior families is not a sensitivity analysis in general.** It tests one alternative,
  chosen by me. A reviewer could reasonably propose a third that behaves differently, and
  the honest description is "tested against a deliberately different alternative", not
  "shown robust to prior choice".
- **The justification is an argument, not a derivation.** "Broad because the timescale is
  incommensurable" is a judgement. It is written down so it can be argued with, which is the
  most that can be offered.
- **More manuscript space** spent on the prior than most reviewers want to read.

The transferable principle: **when a choice is contestable, do not defend it — measure its
consequences.** The same reasoning produced two uncertainty bounds instead of one in my
[geospatial work](https://github.com/nicolaskass/geospatial-data-processing). An assumption
whose influence is quantified stops being a vulnerability and becomes part of the result.
