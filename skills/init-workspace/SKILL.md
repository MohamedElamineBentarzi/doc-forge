---
name: init-workspace
description: Creates the doc-forge workspace layout and the initial plan/status.md. Loaded by /init, and by /produce when it finds no workspace. Never overwrites existing state.
---
# Initializing a workspace

This skill is the single definition of the workspace layout. Do not restate
the directory list anywhere else — load this instead.

## Preflight (all three, before writing anything)

1. **Existing state.** If `plan/status.md` exists, STOP. Do not recreate, do
   not overwrite. Report the recorded phase and the next action from that
   file and tell the user to run `/doc-forge:produce` to resume. Re-init is
   never the fix for a broken run.
2. **Partial layout.** If some directories exist but `plan/status.md` does
   not, create only what is missing and say which ones you added.
3. **Wrong directory.** If the workspace contains `.claude-plugin/plugin.json`
   with name `doc-forge`, you are inside the plugin repo. Refuse and explain:
   the workspace belongs in the project being documented.

## Layout

    plan/            contracts, portfolio.md, status.md, covered.md, styles
    plan/contracts/  one contract per deliverable (written during INTAKE)
    sources/         raw material you provide
    knowledge/       distilled fact cards (RESEARCH output)
    drafts/          per-section drafts + handoff notes
    reviews/         issue reports
    final/           assembled deliverables

Put a `.gitkeep` in each empty directory — git does not track empty
directories, and a workspace that loses half its structure on clone will
confuse the resume logic later.

Instead of a bare `.gitkeep`, write `sources/README.md`:

    # sources/
    Raw material for this documentation project: papers, notes, exports,
    prior docs. Contracts may also point at paths OUTSIDE this workspace
    (a code repo you are documenting) — nothing needs to be copied here.
    If you do point elsewhere, run `/add-dir <path>` first or the research
    agents cannot read it.

## Source survey (cheap, ≤10 lines)

Glob the workspace to note what already exists: top-level directories,
approximate file counts, obvious languages or doc formats, any existing
`docs/` or `README`. Do NOT read file contents — this is an inventory for
INTAKE to confirm, not research. Record it under "Source inventory" in
status.md so intake can propose inferences instead of asking blind questions.

## plan/status.md — initial contents

This file is the resume contract: a brand-new session with no memory of this
one must be able to continue from it alone. Seed it exactly like this.

    # doc-forge status

    workspace: <absolute path>
    initialized: <YYYY-MM-DD>
    phase: INTAKE
    contracts_approved: no

    ## Next action
    Run `/doc-forge:produce` to start the intake interview.

    ## Deliverables
    | slug | contract | research | outline | draft | review | final |
    |------|----------|----------|---------|-------|--------|-------|
    | _none yet — set during INTAKE_ | | | | | | |

    ## Source inventory (from init; INTAKE confirms and refines)
    - <one line per notable source location>

    ## Blockers
    none

    ## Change log
    - <YYYY-MM-DD> workspace initialized

## Report to the user

Confirm what was created (or skipped as already present), then state the two
things they choose between before intake: put material in `sources/`, or plan
to point contracts at outside paths and `/add-dir` them. End with the next
command: `/doc-forge:produce`.
