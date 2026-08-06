---
name: revise-section
description: How to apply issue reports to a draft with minimal collateral damage. Used by df-reviser.
---
# Revision

Order of operations: accuracy blockers -> continuity blockers -> prose
blockers -> majors in the same order -> nits (judgment).

Rules:
- Touch only text implicated by an issue; everything unflagged is frozen.
- Use targeted edits, not rewrites-from-scratch; length must stay within the
  section budget.
- A fix must not create a new violation (re-check the surrounding paragraph
  against the style file after each edit).
- Conflicts between reports: accuracy > continuity > prose. Log any issue
  you deliberately defer with a reason.
- If fixes changed what the section introduces or how it ends, update the
  handoff note and covered.md — stale handoffs corrupt the next section.

Output a fix log: issue ID -> fixed | deferred(reason).
