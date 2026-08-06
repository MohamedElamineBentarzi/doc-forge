# AGENTS.md — orientation for AI agents working on doc-forge

Read this before editing anything in this repo.

## What this repo is

doc-forge is a **Claude Code plugin**. It contains no application code, no
tests, no build step — only Markdown and two JSON manifests. Every file here
is a *prompt* that some agent will load at runtime.

The plugin implements a documentation factory: an orchestrator drives a phase
machine, and each phase spawns fresh-context subagents that read a curated set
of files and write files back. The design constraint the whole thing exists to
satisfy: **a single context window cannot hold a large project and produce a
long, coherent document**. So state lives on disk, and no agent ever sees more
than the slice of the world its job needs.

## The distinction that matters most

There are two different directories in play, and confusing them is the most
common way to break something:

| | **this repo** (the plugin) | **the workspace** (the user's project) |
|---|---|---|
| contains | `agents/ commands/ skills/ styles/` | `plan/ sources/ knowledge/ drafts/ reviews/ final/` |
| written by | you, when editing the plugin | the plugin, at runtime |
| lifetime | versioned, shared by all users | one project |

`plan/`, `sources/`, `knowledge/`, `drafts/`, `reviews/`, `final/` must
**never** be created in this repo. If a task mentions them, it is about
runtime behaviour described in prompts here, not about files you should add.
Likewise, do not add example workspaces or sample outputs as fixtures — they
would ship to every user.

## Layout

```
.claude-plugin/plugin.json       plugin manifest (name, version, author)
.claude-plugin/marketplace.json  marketplace "voidlab" pointing at ./
commands/produce.md              /doc-forge:produce — the orchestrator prompt
agents/df-*.md                   subagent definitions (frontmatter + role prompt)
skills/<name>/SKILL.md           procedures + output formats the agents load
styles/*.md                      per-deliverable-type writing rules
                                 (spec, book, adr, guide, report)
```

Roughly: **commands** decide *when* work happens, **agents** decide *who* does
it and with which tools, **skills** define *how* and in *what format*, and
**styles** define what the resulting prose sounds like. Keep those concerns in
their own layer — a command that inlines a format spec, or an agent that
restates a skill, will drift.

## The pipeline

```
INTAKE → RESEARCH → PLAN → DRAFT → REVIEW/REVISE ⟲ → COLD_READ
       → PORTFOLIO_REVIEW (if >1 deliverable) → ASSEMBLE → DONE
```

| Phase | Agent(s) | Skill | Reads | Writes |
|---|---|---|---|---|
| INTAKE | orchestrator, interactive | `intake` | user answers, workspace glob | `plan/contracts/*.md`, `plan/portfolio.md` |
| RESEARCH | `df-researcher` ×N parallel | `distill-topic` | assigned sources only | `knowledge/<topic>.md` |
| PLAN | `df-architect` per deliverable | `plan-deliverable` | contract, card paths, portfolio | `plan/<d>/outline.md`, `arc.md` |
| DRAFT | `df-writer` per section, sequential | `draft-section` | style, its cards, arc entry, prev handoff, `covered.md` | `drafts/<d>/<s>.md` + handoff |
| REVIEW | `df-reviewer-{continuity,prose,accuracy}` parallel | matching `review-*` | disjoint slices (see below) | `reviews/<d>/<s>-{cont,prose,acc}.md` |
| REVISE | `df-reviser` | `revise-section` | draft + the three reports | edited draft |
| COLD_READ | `df-cold-reader` | `cold-read` | assembled doc + acceptance list only | global issue report |
| PORTFOLIO | `df-portfolio-reviewer` | — | all finals + portfolio.md | cross-doc issue report |

INTAKE is the **only** conversational phase. Everything after it is
file-in / file-out.

## Invariants — do not weaken these

1. **Context isolation is the product.** Each subagent brief names exact input
   paths and says what *not* to read. Widening an agent's inputs "so it has
   more context" defeats the architecture. The three reviewers are
   deliberately asymmetric: prose sees the draft and style file *only* (if the
   text needs sources to make sense, that is the finding), accuracy sees cards
   and may grep sources, continuity sees the arc and history.
2. **Nothing large returns to the orchestrator.** Subagents return a path plus
   a ≤10-line summary, never file bodies. Any change that makes an agent
   return content is a regression.
3. **Everything durable is a file.** `plan/status.md` must always be enough for
   a brand-new session to resume. If you add a phase or an artifact, update
   what `produce.md` records in `status.md` too.
4. **Gates are hard.** Contracts (INTAKE) and outline+arc (PLAN) require
   explicit user approval. Do not add "proceed if the user seems happy" paths.
5. **Writers cannot invent.** Missing facts become `[GAP: ...]` markers, which
   the accuracy reviewer escalates as a research hole. Never relax this into
   "use your best judgement".
6. **`plan/covered.md` is the anti-repetition ledger.** Any agent that
   introduces or moves a concept updates it. Skipping it produces documents
   that explain the same thing three times.
7. **Failure is bounded.** REVIEW/REVISE loops at most 3 times; a phase whose
   gate fails twice stops and asks the user. Keep those bounds.

## Conventions for editing files here

- **Every agent and skill file needs YAML frontmatter.** Agents:
  `name, description, tools, model`. Skills: `name, description`. The `name`
  must equal the filename (or directory name for skills), and `description` is
  what a model uses to decide whether to load it — write it as selection
  criteria, not marketing.
- **`tools:` is a capability boundary, not a convenience list.** Reviewers and
  the cold reader are read-only by design (`df-reviewer-prose` gets `Read`
  alone). Only `df-reviser` has `Edit`. Do not add `Write` to a reviewer, or
  `Bash`/`WebFetch` to anything, without a stated reason.
- **`model:`** — `opus` for the two agents doing genuine structural judgement
  (`df-architect`, `df-cold-reader`); `sonnet` for the rest. Changing a tier
  changes cost for every user, so justify it.
- **Prose style in this repo:** imperative, dense, ~78-column hard wrap, no
  filler. These files are prompts read under context pressure; every wasted
  sentence competes with the user's actual material. Match the surrounding
  file rather than importing your own house style.
- **Formats are defined once, in the skill.** If you change a card, outline,
  handoff, or issue-report format, update the skill *and* check that the
  consumers of that format still line up (e.g. the card format is authored by
  `distill-topic` and consumed by `df-writer` and `df-reviewer-accuracy`).
- **Issue IDs are namespaced** so the reviser and orchestrator can route
  fixes: `C*` continuity, `P*` prose, `A*` accuracy, `G*` cold-read global,
  `X*` cross-document. Severities are `blocker | major | nit`.

## Adding things

**A new style** (`styles/<type>.md`): one screen, voice + structure + hard
bans, phrased so a reviewer can check compliance. Add the type to the
`deliverable_type` and `style` lines in `skills/intake/SKILL.md`.

**A new agent**: add `agents/df-<role>.md` with frontmatter, a role prompt
that loads a skill, and the return contract (`return only path + counts`).
Then wire it into a phase in `commands/produce.md` — an agent no phase spawns
is dead weight.

**A new phase**: update the phase machine in `commands/produce.md` (both the
diagram line and the phase section), and make sure `status.md` can represent
it so resume still works.

## Verifying a change

There is no test suite. What passes for verification:

1. JSON manifests still parse, and `version` in `.claude-plugin/plugin.json` is
   bumped if behaviour changed.
2. Every `name:` matches its filename; every skill referenced by an agent
   exists under `skills/`; every agent named in `commands/produce.md` exists
   under `agents/`.
3. Every style file referenced by `intake`'s contract format exists.
4. End-to-end: install the plugin locally, run `/doc-forge:produce` in a
   scratch workspace with a couple of source files, and confirm the phase you
   touched still produces its artifacts and updates `plan/status.md`.

## Release

Edit files → bump `version` in `.claude-plugin/plugin.json` → push. Users then
run `/plugin marketplace update voidlab` and `/reload-plugins`. Because users
install from a marketplace pointing at this repo's root, anything committed
here ships — keep scratch files out.
