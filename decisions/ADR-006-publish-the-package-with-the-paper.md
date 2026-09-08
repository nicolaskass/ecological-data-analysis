# ADR-006 — Publish the reproducibility package with the paper

**Date:** 2026-04 · **Status:** Accepted

## Context

Journals increasingly require a data availability statement. The requirement is usually
satisfied with a spreadsheet in a supplementary file, or a sentence offering data "on
reasonable request" — a phrase that empirically resolves to *no* in a large fraction of
cases.

Neither is reproducibility. A reader holding the raw data and the paper still cannot
regenerate the paper's numbers, because the numbers came from code they do not have, run
with choices the methods section abbreviated.

The reasons not to publish the code are real and worth stating plainly:

- **The code will be read.** Analysis code written under deadline is not written to be
  admired, and publishing it means publishing that.
- **Errors become permanent and attributable.** A mistake in a private script is a private
  mistake; a mistake in a public repository is discoverable forever, under your name.
- **It invites scrutiny that a spreadsheet does not.** Nobody re-derives your numbers from a
  supplementary table. Somebody might from a runnable repository.
- **The manuscripts are not published yet.** Making the packages public before acceptance
  exposes methods and data ahead of the paper.

## Decision

**Publish a complete reproducibility package per manuscript, in public, under MIT.**

The standard: **a stranger with the repository can regenerate every quantity the manuscript
reports.** Data, scripts, pinned dependencies with tested versions, pre-computed outputs
from a fixed seed to compare against, documented data columns, and an open licence.

Four packages, one identical layout, so that understanding one means understanding all
four.

On timing: the packages went public while the manuscripts were in review or preparation.
That is a deliberate acceptance of the disclosure risk, taken because the packages are the
data availability statement, and because a reproducibility package that appears after
acceptance has already failed the reviewers who most needed it.

On the objections: they are the reason to do it. Publishing code that will be read is the
same trade as reporting an honest error bar — it makes the work checkable, and being
checkable is the only thing that distinguishes a result from an assertion. Both are rarer
than they should be for the same reason: they trade a small amount of comfort for
credibility that cannot be obtained otherwise.

## Consequences

**What was gained**

- The analysis is verifiable rather than merely described.
- Reviewers can run it, which converts vague methodological questions into specific ones.
- The packages are citable artefacts in their own right.
- Writing for publication improves the code — the standard applied is visibly higher than
  for a script nobody will see, which is an argument for publishing rather than against.
- Four packages sharing a layout means the standard is reusable rather than heroic.

**What it cost**

- **Substantial work beyond the analysis**: documentation, cleanup, dependency pinning,
  data dictionaries, a README that assumes nothing.
- **Permanent exposure of mistakes.** Accepted deliberately; it remains the real cost and
  it is not trivial.
- **Disclosure before publication.** Data and methods are public while the manuscripts are
  in review — a scooping risk taken with open eyes, mitigated by the papers being close to
  submission and by results being absent from the packages' framing.
- **Ongoing maintenance.** A pinned dependency list ages, and a package that reproduces
  today may not in five years. A container would fix that and add weight most readers will
  not want.
- **No enforcement across packages.** Four repositories share a layout by convention, with
  nothing checking that they still do.

The general position: **publishing the working is the difference between a claim and a
result.** Every reason not to is a reason it is worth doing, because each one describes
scrutiny being avoided.
