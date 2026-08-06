---
name: distill-topic
description: Format and method for turning raw sources into a compact knowledge card. Used by df-researcher during RESEARCH.
---
# Knowledge card

One topic per card. Cap: 800 words (default; brief may override). Cards are
the ONLY thing writers see about the world — anything omitted here does not
exist downstream, anything padded here wastes a writer's attention.

File: knowledge/<topic>.md

    # <Topic>
    ## In one paragraph
    <the concept, stated plainly>
    ## Facts
    - <atomic fact> [src: path:line | doc §]
    - ...
    ## Behaviors / constraints
    - <what it does / limits / invariants> [src]
    ## Concrete example
    <one minimal, real example — code or scenario, from sources>
    ## Terms
    <term>: <one-line definition>        # feeds the glossary
    ## Conflicts (only if any)
    - source A says X [ref]; source B says Y [ref]
    ## Not covered
    <what you saw but deliberately excluded, one line each>

Method: skim broad (glob/grep) -> read deep only what the topic needs ->
write facts with refs as you go -> compress to cap at the end. Prefer
deleting a fact over dropping its source ref. Never editorialize; "this is
elegant" is not a fact.
