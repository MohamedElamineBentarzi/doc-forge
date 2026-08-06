---
name: review-continuity
description: Checklist and report format for continuity/chaining review of one section. Used by df-reviewer-continuity.
---
# Continuity review

Checklist:
1. Opening picks up exactly where prev handoff `ended_on` left off — no cold
   restart, no re-summary of the previous section.
2. Every term used is defined here or appears in covered.md BEFORE this
   point. Forward references: blocker in specs, major elsewhere.
3. The section delivers its arc entry: assumes respected (nothing used that
   the reader doesn't have), opens created, promises set up.
4. No claim contradicts covered.md or the prev handoff.
5. Re-explanation of a covered.md concept: major (it is the repetition bug).

Report (reviews/<deliv>/<sec>-cont.md):

    ## C<n> [blocker|major|nit] <one-line title>
    where: <quoted offending text or "opening paragraph">
    problem: <why it breaks the chain>
    fix: <minimal concrete change>

End with counts: blockers/majors/nits.
