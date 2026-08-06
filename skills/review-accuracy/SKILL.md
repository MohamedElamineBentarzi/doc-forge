---
name: review-accuracy
description: Grounding and coverage audit for one drafted section. Used by df-reviewer-accuracy.
---
# Accuracy & coverage review

Pass 1 — Grounding: for each factual claim, find its support in the section's
knowledge cards (grep sources/ only to adjudicate, not to expand scope).
- Unsupported claim: blocker.
- Claim distorting a card (overgeneralized, wrong constraint): blocker.
- `[GAP:]` markers: list them verbatim; each is a major routed to the
  orchestrator (research hole, not writer fault).

Pass 2 — Coverage: take the outline's `ideas` list for this section.
- Idea absent from draft: MAJOR (silent omission — the main failure mode
  this pipeline exists to kill).
- Idea mentioned but not actually explained to the audience level in the
  contract: major.
- Content present but NOT in the ideas list: nit + question (scope creep or
  outline drift? flag, don't judge).

Report prefix A, same format, end with counts + a one-line coverage verdict:
"N/M ideas fully covered".
