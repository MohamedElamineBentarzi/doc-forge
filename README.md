# doc-forge

A Claude Code plugin that turns large projects into high-quality documents
(specs, books, ADRs, guides) using an orchestrator + fresh-context subagents
pipeline. All project state lives on disk in the workspace; the plugin carries
only the machinery.

## Install (global / user scope)

    /plugin marketplace add MohamedElamineBentarzi/doc-forge
    /plugin install doc-forge@voidlab        # choose "User" scope
    /reload-plugins

## Use

**1. Initialize a workspace.** Go to the project you want documented — an
empty directory is fine — and run:

    /doc-forge:init

This creates the layout below and seeds `plan/status.md` at phase `INTAKE`.
It never overwrites an existing workspace, so re-running it is safe.

**2. Add your material.** Drop papers, notes, and prior docs into `sources/`.
You do not have to copy a codebase: contracts can point at any path, but run
`/add-dir <path>` first for anything outside the workspace, or the research
agents cannot read it.

**3. Produce.** Run:

    /doc-forge:produce

Phase 0 interviews you and writes one contract per deliverable to
`plan/contracts/`; it stops there until you reply `approved`. Everything
downstream is file-in / file-out. Resume anytime with the same command — it
picks up from `plan/status.md`, so a fresh session loses nothing.

Step 1 is required. `/doc-forge:produce` will not scaffold a workspace — run
in an uninitialized directory it stops and points you at `/doc-forge:init`.

## Update

Edit files in this repo, bump `version` in `.claude-plugin/plugin.json`,
push, then in Claude Code:

    /plugin marketplace update voidlab
    /reload-plugins

## Workspace layout (created by `/doc-forge:init`)

    plan/        contracts, portfolio.md, status.md, covered.md, style pointers
    sources/     your raw material (you put things here)
    knowledge/   distilled fact cards (research output)
    drafts/      per-section drafts + handoff notes
    reviews/     issue reports
    final/       assembled deliverables
