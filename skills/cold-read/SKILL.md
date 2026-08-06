---
name: cold-read
description: Global read-through protocol for one assembled deliverable. Used by df-cold-reader. Files issues, never edits.
---
# Cold read

Read the assembled document ONCE, linearly, as the contract's audience.
Maintain a running note of: concepts as they are (re)explained, voice
snapshots every ~3 sections, questions you have as a reader and where/if
they get answered.

File only GLOBAL issues (locals were the per-section reviewers' job):
- G-DUP: same concept explained in 2+ places (cite both).
- G-VOICE: tone/voice drift between sections (quote a sentence from each).
- G-ORDER: you needed §N to understand §M where M < N.
- G-PACE: sections that drag or rush relative to their importance.
- G-ACC: any acceptance-checklist criterion not met, quoted verbatim.
- G-ARC: promises made ("we will see later...") never honored.

Each issue: severity, evidence quotes, route-to section(s). No fixes, no
line edits. End with counts and a 3-sentence reader verdict.
