![Ecological Data Analysis — Methods and Decision Record. Bayesian CMR, reproducibility packaging, numbers that cannot drift. Python, NumPy, SciPy.](assets/banner.png)

# Ecological Data Analysis — Methods & Decision Record

Statistical analysis of field data from a threatened Patagonian amphibian: mark–recapture
population estimation, morphometrics, and microhabitat selection. Four manuscripts, four
public reproducibility packages, one methodological standard.

**This repository contains no results.** The manuscripts are in review or in preparation,
and their findings belong in the journals. What is documented here is the *method*: how
the analyses are built, validated, packaged and defended.

---

## The problem

Field ecology data has properties that most data work does not.

**It is small and it cannot be regrown.** A capture–mark–recapture dataset is a few
hundred individuals gathered over two field seasons, in a place that takes days to reach,
for a species that spends most of its life underground and surfaces unpredictably. There
is no larger sample. Re-running the experiment means another year and another grant.

**It is expensive per observation.** Every row cost travel, permits, and a person kneeling
on a rocky shoreline at dusk. That changes the economics of analysis completely: throwing
away an inconvenient observation is not cheap, and neither is a method that needs more
data than exists.

**The parameters are not directly observable.** You never see the population. You see
captures, and captures confound abundance with the probability of detecting an animal that
was there. Every quantity of interest is inferred through a model, which means the model's
assumptions are doing real work and must be defensible.

**And it will be reviewed by people paid to find the flaw.** A number that cannot be
justified in detail is not a finding.

That combination — small, precious, indirect, adversarially reviewed — is a demanding
setting for statistical work, and it is where the habits in this repository come from.

## Scale

Measured from the four public packages.

| | |
|---|---|
| Analysis code | ~3,740 LOC Python across 4 packages |
| Largest single analysis | ~1,040 LOC (microhabitat selection) |
| Core stack | NumPy · SciPy · pandas · Matplotlib |
| Inference | Bayesian MCMC, hand-implemented adaptive Metropolis–Hastings |
| Validation | posterior predictive checks, prior sensitivity, WAIC with SE, bias simulation, cross-check against an established R implementation |
| Study design | 3 primary sessions, 14 secondary occasions, 275 individuals |

## The four packages

Public reproducibility repositories, each with data, scripts, pre-computed outputs, pinned
dependencies and an MIT licence.

| Package | Analysis |
|---|---|
| [`atelognathus-reverberii-cmr`](https://github.com/nicolaskass/atelognathus-reverberii-cmr) | Bayesian Pollock's Robust Design capture–mark–recapture, with an extended diagnostics suite |
| [`atelognathus-reverberii-microhabitat`](https://github.com/nicolaskass/atelognathus-reverberii-microhabitat) | Microhabitat selection across three zones, quadrant sampling |
| [`atelognathus-reverberii-morphometrics`](https://github.com/nicolaskass/atelognathus-reverberii-morphometrics) | Morphometric analysis, including recaptured individuals |
| [`atelognathus-reverberii-population-dynamics`](https://github.com/nicolaskass/atelognathus-reverberii-population-dynamics) | Population dynamics from capture histories |

## What's in this repository

| File | Contents |
|---|---|
| [METHODS.md](METHODS.md) | The statistical workflow and why each step is in it |
| [REPRODUCIBILITY.md](REPRODUCIBILITY.md) | The packaging standard applied across all four |
| [decisions/](decisions/) | Seven methodological decision records |

---

## Seven things worth a look

**Every reported number is generated, never transcribed.**
A dedicated script compiles every quantity the manuscript reports into a single JSON, and
the manuscript source reads its numbers from that file. Its docstring states the reason
plainly: so that the text and the table *cannot drift out of sync*. Re-run the sampler,
re-run the compiler, re-render — and every figure in the prose updates together. Numbers
copied by hand from a console into a paragraph are the most common source of published
errors in quantitative papers, and they are entirely avoidable.
→ [ADR-001](decisions/ADR-001-every-number-is-generated.md)

**Numbers that come from elsewhere declare where they came from.**
The few quantities computed outside the pipeline — a closure test, an R reference fit —
are listed in an explicit constants block with provenance comments, rather than being
quietly hardcoded among the generated ones. A hardcoded number that looks generated is
worse than one that is obviously manual.
→ [ADR-001](decisions/ADR-001-every-number-is-generated.md)

**The model is implemented, then checked against an established one.**
The Robust Design sampler is written directly — adaptive Metropolis–Hastings, four chains,
explicit warmup — rather than delegated to a black box. And the result is cross-checked
against the maximum-likelihood equivalent from an established R package. Writing it means
understanding it; checking it against a reference implementation means the understanding
is verified rather than assumed.
→ [ADR-002](decisions/ADR-002-implement-then-cross-check.md)

**Priors are justified from the literature, then tested for influence.**
Each prior carries a comment naming the published estimate it derives from and stating why
it is broad — a congeneric species, on an incommensurable timescale, with a burrowing life
history. The whole model is then re-fitted under a deliberately different prior to measure
how much the conclusions depend on that choice. Prior sensitivity analysis is widely
recommended and rarely done.
→ [ADR-003](decisions/ADR-003-priors-justified-and-tested.md)

**The estimator's own bias is simulated.**
One assumption of the design — no emigration within a session — is doubtful for an animal
that returns underground unpredictably. Rather than noting the caveat and moving on, a
simulation quantifies how far the abundance estimate is pushed when the assumption fails.
Naming a limitation is easy; measuring it is the part that changes what you can claim.
→ [ADR-004](decisions/ADR-004-simulate-your-own-bias.md)

**Data decisions are written where the data is used.**
A zero-effort day absent from the dataset, and a low-yield day kept because the captures
were real events — both are recorded in the analysis script's own header, with reasoning.
These are exactly the decisions that get forgotten, and exactly the ones a reviewer asks
about eighteen months later.
→ [ADR-005](decisions/ADR-005-data-decisions-live-in-the-code.md)

**The parameter is named for what it measures.**
The model estimates *apparent* survival, which confounds mortality with permanent
emigration. In a species that spends most of its life underground, that distinction is the
whole interpretation. Calling the parameter "survival" would be a modelling result quietly
converted into a biological claim.
→ [ADR-007](decisions/ADR-007-apparent-survival-is-not-survival.md)

---

## Why this is in a software portfolio

Because it is the origin of the practices in my other work, and the connection is not a
metaphor.

The habit of validating against data held out of model fitting is the same one that makes
[survey accuracy claims defensible](https://github.com/nicolaskass/geospatial-data-processing).
The habit of propagating uncertainty to whatever gets reported is the same. The habit of
building a single generated source of truth so that text and tables cannot disagree is the
same instinct as a
[documentation staleness check](https://github.com/nicolaskass/specialty-retail-erp) or a
citation verifier: **anything maintained by intention drifts, so bind it mechanically.**

Statistics under adversarial review teaches something specific that software work rarely
forces: that a number needs a defence, and that "it ran without error" is not one.

---

## On AI-assisted development

Built with [Claude Code](https://claude.com/claude-code) under the version-controlled
agent setup described in my
[ERP repository](https://github.com/nicolaskass/specialty-retail-erp#on-ai-assisted-development).

The boundary matters more here than anywhere else in my work. Figure composition,
packaging, refactoring and diagnostic scaffolding are pattern work. The modelling was not:
which parameters the design can identify, which prior is defensible and why, what a
posterior predictive check should be checking, whether a violated assumption biases an
estimate upward or downward. Those are claims about inference that carry my name into peer
review, and a confidently wrong one becomes a retraction rather than a bug report.

---

## Author

Nicolás Kass — biologist (zoology), PhD candidate at UNLP, ISO 9001 consultant, and
software architect. Field ecology and conservation of Patagonian amphibians; operational
and analytical systems for small businesses at [T³](https://t3.com.ar).

Findings, manuscript text and unpublished data are omitted throughout. The reproducibility
packages linked above are published under MIT.

**Licence:** the writing in this repository is published under
[CC BY 4.0](LICENSE) — reuse it, quote it, build on it, with attribution.
