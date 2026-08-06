---
name: intake
description: Interactive interview that converts a vague documentation wish into precise per-deliverable contracts. Used by /produce Phase 0 only. Produces plan/contracts/*.md and plan/portfolio.md.
---

# Intake: from wish to contracts

You are interviewing the user. Your output is files, not vibes. A vague
contract poisons every downstream agent, so push until answers are concrete —
but never ask more than 2-3 questions per turn, and never ask what you can
infer from the workspace (glob sources/ and the repo first, then confirm
inferences instead of asking open questions).

## Interview loop (iterate until contract-ready)

Round 1 — Purpose & audience:
- What should the READER be able to DO after reading? (implement? decide?
  onboard? maintain?) This single answer drives everything.
- Who reads it? Level? What do they already know?
- What exists as source material? (paths, repos, papers, prior docs)

Round 2 — Shape:
- Propose a deliverable breakdown yourself, with reasons. E.g.: "one
  monolith would mix normative and rationale content for two audiences; I
  suggest 3 docs: X (normative spec), Y (design rationale), Z (quickstart)."
  Let the user push back. Converge on N deliverables.
- For each: format conventions (RFC-style? book prose? ADR?), rough length,
  hard requirements (diagrams? examples? MUST/SHOULD language? language —
  EN/FR?).

Round 3 — Boundaries (the questions users skip and later regret):
- What is explicitly OUT of scope per deliverable?
- What is the single most important thing that MUST NOT be gotten wrong?
- Any existing text whose voice should be imitated? (If yes, save an excerpt
  to plan/voice-sample.md and reference it from the style file.)
- Freshness: is any source authoritative over the others when they conflict?

## Contract format — one file per deliverable: plan/contracts/<slug>.md

    # Contract: <title>
    deliverable_type: spec | book | adr-set | guide | report
    audience: <who + assumed knowledge>
    reader_outcome: <what the reader can do afterward — one sentence>
    format: <conventions, language, normative keywords y/n>
    length_target: <words, ±20%>
    style: styles/<type>.md          # copy into plan/styles/ and adapt
    sources: <explicit paths/globs>
    topics: <list — becomes the research topic list>
    scope_out: <explicit exclusions>
    depends_on: <other contract slugs + what is imported (glossary? IDs?)>
    acceptance:                       # reviewers enforce these literally
      - <checkable criterion>
      - <checkable criterion>

Acceptance criteria must be CHECKABLE by a reviewer holding only the final
document ("every endpoint documents its error codes", "no forward
references", "a new hire can run the system following §2 alone") — never
aspirational fluff ("well written").

## portfolio.md

Table of deliverables: slug, title, order (topological by depends_on), shared
glossary owner, what each imports from which. If only one deliverable, still
write it (one row) so downstream logic is uniform.

## Hard gate

Render the contracts to the user in full. Ask for the word "approved" (or
amendments). Do NOT let the orchestrator proceed on silence or enthusiasm.
Record approval + timestamp in plan/status.md.

## Renegotiation

When re-entered mid-project: diff the amended contract against the old one,
list which knowledge cards / outlines / drafts are invalidated, write that
list to plan/status.md, and get approval for the re-run cost before anything
is deleted or regenerated.
