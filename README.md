# Multi-agent workflow template

A 4,000-line `AGENT-NOTES.md` cost on the order of 90,000 tokens every time a new Grok, Claude, or bot session started — most of it closed work.

The mistake was not taking notes. It was injecting history. This kit injects a briefing (`AGENTS.md` + `docs/NOW.md`) and retrieves the rest.

Copy `project/` into a product repo. Do not treat this directory as an app. MIT licensed.

```bash
git clone https://github.com/chengguan/workflow-template.git ~/Projects/workflow-template
```

**Operator steps** (new project, or a feature / bug / task in an existing one): [`HOWTO.md`](HOWTO.md).

## Template vs skill

**Use this template.** It is the system of record.

Grok, Claude, and bots only share what is **in git**. Grok auto-loads `AGENTS.md`; recent Claude Code does too; bots see the repo. A Grok skill lives in `~/.grok/skills` and is invisible to Claude and the bots unless you copy it into each harness, where it will drift.

`project/AGENTS.md` is already the agent-facing skill: every tool injects it. `docs/NOW.md` is the briefing. Putting the same protocol in a Grok skill would create a second home for the same facts.

A **thin** Grok skill is worth adding later only as a pointer: “scaffold from `~/Projects/workflow-template`” or “you launched from `$HOME`; `cd` to the repo.” It must not restate `WORKFLOW.md`. Do not add that skill unless you want `/workflow-template` as a shortcut.

## Layout

```
workflow-template/
  README.md                 this file (humans)
  HOWTO.md                  start a project or a task
  LICENSE                   MIT
  AGENTS.md                 kit guard: do not build product here
  instantiate.sh            copy project/ into a dest dir
  project/                  COPY THIS TREE onto a repo root
    AGENTS.md               durable law + load protocol
    docs/
      NOW.md                current task only (overwrite)
      DECISIONS.md          index, one line each
      WORKFLOW.md           roles, gates, session protocol
      decisions/            D-NNN-slug.md bodies
      tasks/                packets + optional .log.md
      archive/              frozen notes; never auto-loaded
      releases/             one file per shipped version
  examples/                 density samples; do not copy into a repo
```

## New or existing project

Day-to-day: [`HOWTO.md`](HOWTO.md).

Adopting this kit on a repo that already has `AGENT-NOTES.md`: `instantiate.sh` (skips existing files; `-f` overwrites), then **Existing-repo adoption** in `project/docs/WORKFLOW.md`.

## What a new agent is allowed to load

1. `AGENTS.md` (auto)
2. `docs/NOW.md`
3. Stop

Everything else is retrieved when `NOW.md` names it, or when a task collides with one index line in `DECISIONS.md`.
