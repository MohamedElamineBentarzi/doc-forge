---
name: plan-deliverable
description: How to produce outline.md and arc.md for one deliverable from its contract and knowledge cards. Used by df-architect during PLAN.
---
# Planning one deliverable

## outline.md

    # Outline: <title>   (contract: <slug>)
    ## S01 <section title>
    goal: <one sentence — what the reader gains>
    ideas:            # exhaustive; writers are audited against this list
      - <idea>
      - <idea>
    cards: knowledge/a.md, knowledge/b.md    # ONLY what this section needs
    budget: <words>
    ## S02 ...
    ---
    ## Coverage matrix
    | contract topic | section(s) |    # every topic mapped, none orphaned

Ordering principle differs by type:
- book/guide: motivation-first; each section earns the next one.
- spec: dependency-first; definitions before use, no forward references.
- adr-set: one decision per ADR; order chronologically or by system layer.

Card allocation is a context budget. A section receiving 6+ cards is a smell:
split the section or merge cards.

## arc.md — one entry per section (this file creates the human flow)

    ## S01
    assumes: <concepts the reader has at this point — from prior sections
              or audience baseline>
    opens: <the question/tension this section raises>
    promises: <what the NEXT section will resolve or build on>
    introduces: <terms first defined here>
    tone note: <optional — e.g., "slow down, this is the hardest section">

Write each entry as a brief to a ghostwriter who will read nothing else.
If you cannot fill "opens"/"promises" for a section, the ordering is wrong —
fix the outline instead of writing filler.
