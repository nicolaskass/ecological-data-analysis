# Decision Records

An **Architecture Decision Record (ADR)** is a short document capturing one significant
decision at the moment it is made: the situation that forced it, the option chosen, and
what that choice costs. The convention was proposed by Michael Nygard in 2011, for
software. The records here are methodological — decisions about inference rather than about
code — and the format holds, because a modelling decision has exactly the property the
format was designed for: it is made once, quietly governs every number that follows, and is
impossible to reconstruct later from the output alone.

Each record states context, decision, and consequences including the costly ones. An ADR
that lists only benefits is marketing, not a record.

Dates are when the decision landed in a repository, taken from repository history.

| # | Decision | Date | Status |
|---|---|---|---|
| [001](ADR-001-every-number-is-generated.md) | Every reported number is generated, never transcribed | 2026-05 | Accepted |
| [002](ADR-002-implement-then-cross-check.md) | Implement the model, then cross-check it | 2026-04 | Accepted |
| [003](ADR-003-priors-justified-and-tested.md) | Priors are justified from literature and tested for influence | 2026-05 | Accepted |
| [004](ADR-004-simulate-your-own-bias.md) | Simulate the estimator's own bias | 2026-05 | Accepted |
| [005](ADR-005-data-decisions-live-in-the-code.md) | Data decisions live in the code | 2026-04 | Accepted |
| [006](ADR-006-publish-the-package-with-the-paper.md) | Publish the reproducibility package with the paper | 2026-04 | Accepted |
| [007](ADR-007-apparent-survival-is-not-survival.md) | Apparent survival is not survival | 2026-05 | Accepted |
