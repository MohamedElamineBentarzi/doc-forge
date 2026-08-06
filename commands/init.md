---
description: Create the doc-forge workspace layout (plan/ sources/ knowledge/ drafts/ reviews/ final/) and seed plan/status.md
argument-hint: "[optional: path to initialize — defaults to the current directory]"
---

Initialize a doc-forge workspace in `$1` (default: the current directory).

Load the `init-workspace` skill and follow it exactly: run its preflight
checks first, create the layout, survey existing sources, and seed
`plan/status.md` at phase `INTAKE`.

This command only scaffolds. Do not interview the user, do not write
contracts, do not read source files beyond the inventory glob — that is
`/doc-forge:produce`'s job. Finish by telling the user to run it.

Running this in an already-initialized workspace must be safe: the skill's
preflight stops on an existing `plan/status.md` rather than overwriting it.
