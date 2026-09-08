# Reproducibility packaging

The standard applied to all four analysis packages, and what it costs.

## Contents

- [Why publish the package at all](#why-publish-the-package-at-all)
- [The package layout](#the-package-layout)
- [Determinism](#determinism)
- [The generated-numbers layer](#the-generated-numbers-layer)
- [Data documentation](#data-documentation)
- [What this does not solve](#what-this-does-not-solve)

---

## Why publish the package at all

Journals increasingly ask for data availability. Most authors satisfy it with a spreadsheet
and a sentence.

That is not reproducibility. A reader with the raw data and the paper still cannot obtain
the paper's numbers, because the numbers came from code the reader does not have, run with
choices the paper did not fully describe.

The standard here is stricter and simple to state: **a stranger with this repository can
regenerate every quantity the manuscript reports.** Data, scripts, pinned dependencies,
pre-computed outputs to compare against, and an open licence.

The cost is real. Publishing the code means publishing the mistakes in it, permanently and
under your name. That is the point — it is the same trade as an honest error bar, and the
same reason both are rarer than they should be.

## The package layout

Identical across all four, so that knowing one means knowing all of them:

```
package/
├── data/
│   ├── <the actual data>.csv
│   └── README_data.md        ← column key, units, study-site notes
├── scripts/
│   ├── <primary analysis>.py ← the fit, and the figures it produces
│   ├── extended_analysis.py  ← diagnostics, sensitivity, simulation
│   └── generate_reportables.py ← compiles every reported quantity to JSON
├── outputs/                  ← pre-computed, from a fixed seed
│   ├── *_posterior_summary.csv
│   ├── reportable_quantities.json
│   ├── extended_diagnostics.json
│   └── fig_*.png
├── requirements.txt          ← with tested versions named
├── LICENSE                   ← MIT
└── README.md                 ← what it is, how to run it, what to expect
```

**Pre-computed outputs ship with the package.** Not because a reader should trust them, but
so a reader can diff their run against them. Reproduction that produces no comparison point
is not verification — it is just execution.

**The README names tested versions**, not only package names. "numpy" is not a dependency
specification; "tested with Python 3.11, numpy 1.26, scipy 1.11, matplotlib 3.8" is.

## Determinism

Every stochastic step is seeded, and the seed is a constant in the file, not a command-line
default that can be forgotten.

- The MCMC runs from a fixed seed, and the shipped outputs are labelled with it.
- Bootstrap resampling uses an explicitly seeded generator.
- Simulation studies take the same seed.

Seeding is not about hiding variability — the whole point of the analysis is quantifying
it. It is about making a *specific reported number* regenerable. A reader who gets a
different value should be able to conclude that something differs in their environment,
rather than shrugging at randomness.

Modern NumPy generator API rather than the legacy global random state, so that seeding is
local to the analysis and cannot be disturbed by an unrelated import.

## The generated-numbers layer

The practice I would keep if I could keep only one.

A script reads the posterior summary and the raw data, and compiles **every quantity the
manuscript reports** into a single JSON file. The manuscript source then reads its numbers
from that file rather than containing them.

Its own docstring gives the reason: so that the text and the tables *cannot drift out of
sync*.

```
sampler → posterior_summary.csv → generate_reportables.py
       → reportable_quantities.json → manuscript
```

Re-run the sampler, re-run the compiler, re-render the manuscript, and every number in the
prose, every cell in the table, and every caption update together. There is no step at
which a human retypes a value.

Manual transcription from a console into a paragraph is among the most common sources of
error in published quantitative work, and it is the easiest to eliminate entirely. It is
also the error that survives peer review most reliably, because a plausible wrong number in
a sentence looks exactly like a right one.

**Quantities computed elsewhere are declared as such.** A handful of numbers come from
outside the pipeline — a closure test computed in R, a reference maximum-likelihood fit.
These live in an explicit constants block with provenance comments naming where each came
from and that it is frozen. A hardcoded number sitting among generated ones, undeclared, is
worse than an obviously manual one: it inherits the credibility of the automation without
the substance.

See [ADR-001](decisions/ADR-001-every-number-is-generated.md).

## Data documentation

Each package carries a `README_data.md` beside the data: what each column is, in what
units, how it was recorded, and what the study site notes are.

The audience is a stranger, and also me in two years. Field data columns carry abbreviated
names that are obvious during a field season and opaque afterwards, and the meaning of a
code is not recoverable from the data itself.

## What this does not solve

Honest limits of the standard:

- **A pinned version list is not a container.** Dependencies drift, and a package that
  reproduces today may not in five years. A container image would fix that and would add
  weight most readers will not want; this is a deliberate trade toward accessibility, and
  it will eventually be wrong.
- **Reproducing the analysis is not reproducing the study.** The field methods, the
  detection process and the observer are not in the repository, and they are where most of
  the real uncertainty lives.
- **Nothing verifies the packages against each other.** Four repositories share a layout by
  convention, and there is no check that they still do — the same class of gap I closed
  elsewhere with an automated
  [citation checker](https://github.com/nicolaskass/geospatial-data-processing), and have
  not closed here.
- **The seed guarantees a number, not its correctness.** Determinism reproduces a mistake
  exactly as faithfully as a result.
