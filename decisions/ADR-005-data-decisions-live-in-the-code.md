# ADR-005 — Data decisions live in the code

**Date:** 2026-04 · **Status:** Accepted

## Context

Field data arrives with irregularities that require judgement, and the judgements are made
long before the paper is written.

Two real examples from this dataset:

- A scheduled sampling day on which **no effort occurred** because of weather. It is absent
  from the dataset entirely — which is correct, because zero effort is not zero captures,
  and recording it as a day with no captures would bias detection probability downward.
- A day with **adverse conditions and few captures**, which was nonetheless kept, because
  the captures that did occur are real events and excluding them would discard information
  on the basis of the result being inconvenient.

Both are defensible. Both are invisible in the data file. And in eighteen months, when a
reviewer asks why the design lists a certain number of occasions when the field notes
suggest another, the reasoning has to come from memory — or from a lab notebook nobody can
find, or from a methods paragraph written months after the decision.

The standard place for this is the manuscript's methods section, which fails in a specific
way: it is written late, summarised for length, and separated from the code where the
decision actually takes effect.

## Decision

**Record data decisions in the analysis script's header, next to the design they modify.**

The script that implements the design states the study structure — sessions, occasions,
dates — and directly beneath, the exceptions and their reasoning: the zero-effort day and
why its absence is correct, the low-yield day and why it was retained, with the note that
the reference implementation treats it the same way.

Three properties make this the right location:

**It is the same file as the constants it explains.** The occasion counts appear a few lines
below the note explaining why they are what they are. Neither can be changed without seeing
the other.

**It is version-controlled with the analysis.** The decision, its reasoning and the code it
affects move together through history, so `git log` can answer when a decision changed.

**It is read by anyone who touches the analysis**, which is the population that most needs
it — including the author, later.

The biological context sits in the same header for the same reason: the interpretation of a
parameter depends on the natural history of the animal
([ADR-007](ADR-007-apparent-survival-is-not-survival.md)), and separating them invites
somebody to change one without the other.

## Consequences

**What was gained**

- Every judgement call is recoverable, with reasoning, years later.
- Reviewer questions about the design are answered from the record instead of from memory.
- The methods section is *written from* the code rather than reconstructed, so it cannot
  quietly disagree with what was run.
- A reader of the reproducibility package sees the decisions, not just their consequences.

**What it cost**

- **Long docstrings**, unidiomatic in code whose purpose is not partly documentary.
- **Nothing verifies the prose against the code.** A note could describe an exclusion that a
  later edit removed, and no test would fail. The coupling is by convention — the same
  weakness I closed with an automated checker in my
  [geospatial work](https://github.com/nicolaskass/geospatial-data-processing) and have not
  closed here.
- **Duplication with the manuscript**, since the methods section must still state it, and
  the two can drift.
- **It relies on remembering to write it down at the time.** A decision made in the field
  and recorded three weeks later is already reconstructed rather than recorded.

The general point: **put the reasoning where the decision takes effect.** A justification
stored somewhere else — a notebook, a methods section, an email — is one reorganisation away
from being lost, and the cost is paid at the worst possible moment, when somebody
authoritative is asking why.
