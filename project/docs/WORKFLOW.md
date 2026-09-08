# Agent workflow

How Grok, Claude, and bots share a repo without loading history into every prompt.

Inject a briefing. Retrieve a history. Git, GitHub issues, and archived logs keep the years. They must not sit in the prompt.

## Roles

- **Product owner (CG)** — scope, Ready/Done, device testing. Only CG moves an item to Done.
- **Developer agent** — interactive session (usually Claude). Implements, tests, deploys. Not `Claude.bot`.
- **Review agent** — interactive session (usually Grok). Design partner and pre-commit review. Check working tree, not only `git log`. Not `Grok.bot`.
- **Bots** (`Claude.bot`, `Grok.bot`) — automation. Headings and `(for …)` tags in task logs must use the bot names. Bot stamps are not interactive reviews.

Rename the developer/reviewer lines in this file if a project swaps tools. Do not fork a second workflow doc.

## Artifacts (one home per fact)

| Kind | Home | Auto-loaded? | Mutate |
|---|---|---|---|
| Product law | `AGENTS.md` | yes | Edit when the **law** changes |
| Current work | `docs/NOW.md` | yes (step 2) | **Rewrite** at every handoff |
| Settled choice | `docs/decisions/D-NNN-slug.md` + one index line | no | Write once; mark superseded |
| Task intent | GitHub issue body, or `docs/tasks/<id>.md` | no | Write at Ready; update constraints |
| Review transcript | `docs/tasks/<id>.log.md` | no | Append; archive when closed |
| What landed | `git log`, issue comments | no | Commits cite `D-NNN` / issue |
| How to verify | tests + this project's test plan | no | Run, don't narrate |

`NOW.md` budget: 40–80 lines. `AGENTS.md` budget: 80–120 lines. Always-on target: under ~3,000 tokens.

## Backlog gate

GitHub issue (or equivalent) is the backlog. Status: `Backlog` → `Ready` → `In progress` → `In review` → `Done`.

An item in `Backlog` is not a start signal. Pick up only `Ready`. On pickup, move to `In progress` and write or refresh the task packet. Only CG moves to `Done`.

## Per task

1. **Packet** (issue body or `docs/tasks/<id>.md`): intent, why, constraints / do-not, acceptance, out of scope. Not a diary.
2. Discuss open UX questions in `docs/tasks/<id>.log.md`, tagged `(for Grok)` / `(for Claude)` / `(for Claude.bot)` / `(for Grok.bot)`. Quote the other agent when the wording is the contract. Consensus before implementation.
3. Implement and run the verify command in `AGENTS.md`.
4. Review **before** commit. Reviewer writes findings in the log, then rewrites `NOW.md` (Open / Do not / Next). Do not paste the review into `AGENTS.md`.
5. New choice that must outlive the task: add `D-NNN-slug.md` and one index line. Repeating the same three laws in every stamp means they belong in `AGENTS.md` or a `D-NNN`, not in `NOW.md`.
6. Commit messages cite issue / `D-NNN`. Working tree beats any note.

Most tasks produce **zero** new decision files.

### Decision file shape

```markdown
# D-NNN — {{one line}}
Date: YYYY-MM-DD
Status: accepted
Task: #N

Decision: …
Why: …
Rejected: …
Follow-on: …
```

### `NOW.md` shape

Intent, Done, Open, Do not, Next, pointers (`HEAD`, packet path, log path). No quotes. One task (or two short sections). If it will not fit in 80 lines, extract a decision or archive the log.

## Session protocol

**Start** (every agent, including bots)

1. Be in this repo (`cd` here before launch).
2. Read `AGENTS.md` (auto) then `docs/NOW.md`.
3. If `NOW.md` names a packet or `D-NNN`, read **that file only**.
4. `git status` and `git log -8 --oneline`.
5. Do not open archive, full logs, or `AGENT-NOTES.md`.

**End**

Rewrite `docs/NOW.md` from scratch. Checklist: intent, open, do-not, next, pointers; new durable choice? → `D-NNN`; quotes? → task log only; `AGENTS.md` unchanged unless the law changed.

Bots: same two files. Bot prompt must say `AGENTS.md` + `docs/NOW.md`, not `AGENT-NOTES.md`.

## Launching a new instance

```bash
cd {{path to this repo}}
grok          # or: claude
```

First message:

```
Read docs/NOW.md. Then: {{the job in one sentence}}.
```

Do not type “load the project” or “read AGENT-NOTES.” Do not `@` `NOW.md`’s history files. Resume the existing session when it is the same thread; a new instance is for a full window or a different role.

## Compaction

**Task close:** promote new law → `AGENTS.md`; surviving choice → `D-NNN`; rewrite `NOW.md` to idle or the next Ready item; drop the log from `NOW.md` (move to `docs/archive/` if you want it kept).

**Release:** `docs/releases/{{VERSION}}.md`; update the shipping line in `AGENTS.md`.

**Quarter:** mark superseded decisions; delete snapshot plans from `AGENTS.md` (they belong in a packet). Never re-merge archive into `NOW.md`.

## Existing-repo adoption

1. Copy `project/` onto the repo root (or run the kit’s `instantiate.sh`).
2. Fill `{{PLACEHOLDERS}}`. Move snapshot plans out of any old `AGENTS.md`.
3. Write `docs/NOW.md` from the **current** open task only.
4. `git mv docs/AGENT-NOTES.md docs/archive/AGENT-NOTES-{{YYYY}}-qN.md` (or equivalent).
5. Harvest repeated stamps (“do not recut”, “do not enable network”) into `AGENTS.md` or `D-NNN` files.
6. Point bots at `AGENTS.md` + `docs/NOW.md`.

Leave the archive in git. Do not summarise it into a new always-loaded file.
