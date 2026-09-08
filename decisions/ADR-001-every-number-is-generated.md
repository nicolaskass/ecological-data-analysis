# ADR-001 — Every reported number is generated, never transcribed

**Date:** 2026-05 · **Status:** Accepted

## Context

A quantitative manuscript contains dozens of numbers: parameter estimates, credible
intervals, sample sizes, test statistics, model comparison values. They appear in the
abstract, in the prose, in tables, and in figure captions — frequently the same quantity in
several places.

The normal workflow is that the analysis prints them and a human types them into the
manuscript.

That workflow has a failure mode which is quiet, common, and survives review. Numbers get
transcribed with a digit wrong. The analysis is re-run after a reviewer's request and the
abstract is updated while a sentence in the discussion is not. A table is rebuilt and one
cell keeps its old value. Nothing looks wrong: a plausible number in a sentence is
indistinguishable from a correct one, and no reviewer recomputes every figure.

The exposure grows with every revision. A manuscript that goes through three rounds of
review has had its numbers re-derived three times, and each round is a fresh chance for one
place to be missed.

## Decision

**A single generated artefact is the source of truth for every reported quantity, and the
manuscript reads from it.**

```
sampler → posterior_summary.csv → generate_reportables.py
       → reportable_quantities.json → manuscript source
```

A dedicated script compiles every reportable quantity — design counts, posterior summaries,
the reference maximum-likelihood fit, the closure test — into one JSON. The manuscript
source pulls its numbers from that file rather than containing them.

Its docstring states the reason explicitly: so that the text and the table cannot drift out
of sync. Re-run the sampler, re-run the compiler, re-render, and prose, tables and captions
move together. There is no step at which a value is retyped.

**Quantities computed outside the pipeline are declared as such.** A few numbers come from
elsewhere — a closure test computed in R, a frozen reference fit. They live in an explicit
constants block with provenance comments naming their origin. A hardcoded value sitting
undeclared among generated ones is worse than an obviously manual one, because it inherits
the credibility of the automation without the substance.

**The compiled artefact ships with the reproducibility package**, so a reader can inspect
exactly the set of numbers the paper claims.

## Consequences

**What was gained**

- Transcription errors are eliminated as a category, not reduced.
- Revision is safe: re-running propagates everywhere at once, which removes the main risk
  of responding to reviewers.
- The full set of claimed quantities is inspectable in one file.
- Provenance is explicit for the numbers that have external origins.

**What it cost**

- **Infrastructure before writing.** The compiler must exist and be maintained before the
  first draft benefits from it — real setup cost for a single paper, only paying off across
  revisions and across papers.
- **The manuscript source is no longer plain prose.** It is a document with executable
  chunks, which means the writing environment now has dependencies, and a co-author who
  edits in Word cannot participate in the same way. This is a genuine collaboration cost
  and the most common reason the practice is refused.
- **A bug in the compiler is a systematic error**, propagating identically everywhere
  instead of appearing in one place where it might be noticed.
- **The static-inputs block is a maintenance hazard.** Frozen values need re-checking when
  upstream analyses change, and nothing enforces that.
- **The JSON must stay in step with the sampler's output format**, which is an unverified
  coupling — the same class of gap I closed elsewhere with an automated citation checker and
  have not closed here.

The reasoning generalises past manuscripts, and it is the same one behind a documentation
staleness check or a citation verifier in my software work: **two representations of the
same fact, maintained by intention, always drift.** The fix is never more discipline. It is
to make one of them generated from the other.
