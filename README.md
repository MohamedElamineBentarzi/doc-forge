# doc-forge

A Claude Code plugin that turns large projects into high-quality documents
(specs, books, ADRs, guides) using an orchestrator + fresh-context subagents
pipeline. All project state lives on disk in the workspace; the plugin carries
only the machinery.

## Install (global / user scope)

    /plugin marketplace add <your-github-username>/doc-forge
    /plugin install doc-forge@voidlab        # choose "User" scope
    /reload-plugins

## Use

In any workspace:

    /doc-forge:produce

Phase 0 interviews you and writes contracts to `plan/contracts/`.
Everything downstream reads/writes files only. Resume anytime with
`/doc-forge:produce` — it picks up from `plan/status.md`.

## Update

Edit files in this repo, bump `version` in `.claude-plugin/plugin.json`,
push, then in Claude Code:

    /plugin marketplace update voidlab
    /reload-plugins

## Workspace layout (created by the plugin)

    plan/        contracts, portfolio.md, status.md, covered.md, style pointers
    sources/     your raw material (you put things here)
    knowledge/   distilled fact cards (research output)
    drafts/      per-section drafts + handoff notes
    reviews/     issue reports
    final/       assembled deliverables
