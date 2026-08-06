---
name: draft-section
description: Writing rules and handoff protocol for drafting one section from a curated brief. Used by df-writer during DRAFT.
---
# Drafting one section

## Before writing
1. Read the style file FIRST; it wins over your defaults.
2. Read your arc entry; your section must open from "assumes", create
   "opens", and set up "promises".
3. Read covered.md; anything listed there is referenced ("as defined in
   §X"), never re-explained.

## While writing
- Motivation before mechanism: the reader must know why this section exists
  within the first 2-3 sentences (specs included — one sentence of context
  before normative text).
- Prose is the default; lists only where the style file allows and the
  content is genuinely enumerable.
- Transitions carry ideas, not just order: the last paragraph should make
  the next section feel necessary (books/guides) or close the dependency
  cleanly (specs).
- Every fact comes from your cards. Missing fact -> `[GAP: what's missing]`
  inline. Never bridge a gap from your own training knowledge.
- Vary sentence rhythm; ban your own tics (scan your draft for a word or
  construction used 3+ times and rewrite).
- Respect the word budget ±20%.

## After writing — handoff note: drafts/<deliv>/<sec>.handoff.md

    covered: <ideas actually written, vs outline list — note any drift>
    introduced: <terms defined here>
    ended_on: <last idea + the hook/state left for the next section, 2 lines>
    gaps: <GAP markers, if any>

Then append to plan/covered.md: `<concept> — <deliv>/<sec>#<heading>` per
newly explained concept.
