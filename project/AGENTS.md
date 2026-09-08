# {{PROJECT}}

{{ONE_LINE_WHAT_THIS_IS}}

Shipping: **{{VERSION}}** (build {{BUILD}}).

## Layout

- `{{SRC}}` — {{SRC_ROLE}}
- `docs/` — current briefing (`NOW.md`), decision index, task packets, workflow

## Hard rules

- {{RULE_1}}
- {{RULE_2}}
- {{RULE_3}}

Keep this list as product law that outlives a task. Dated choices go in `docs/decisions/`, not here.

## Context loading

1. This file.
2. `docs/NOW.md`.
3. Stop.

Do not read `docs/archive/`, `docs/tasks/*.log.md`, or any `AGENT-NOTES.md` unless `docs/NOW.md` names that exact path. Durable choices live in `docs/DECISIONS.md` (index). Read one `docs/decisions/D-*.md` only when this task collides with that decision.

Do not create `CLAUDE.md` (or a second instruction file) that copies this one. Grok loads every matching name; two copies double the tokens.

## Verify before handoff

{{TEST_COMMAND}}

## Roles and gates

`docs/WORKFLOW.md`. Do not paste it here.
