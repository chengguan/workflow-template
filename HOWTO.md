# How to start work

For you (CG). Agents follow `AGENTS.md` + `docs/NOW.md` in the product repo. Do not paste this file into a prompt.

## 1. New project

```bash
mkdir -p ~/Projects/{{name}}
~/Projects/workflow-template/instantiate.sh ~/Projects/{{name}}
cd ~/Projects/{{name}}
```

Fill `{{PLACEHOLDERS}}` in `AGENTS.md` (one-line product, layout, hard rules, verify command) and leave `docs/NOW.md` idle until there is a first task. Leave `docs/WORKFLOW.md` unless roles differ.

```bash
git init
git add AGENTS.md docs
git commit -m "Add agent workflow (NOW.md briefing, archived history)."
```

Then start the first task below. Spec (`docs/SCOPE.md` or equivalent) is a packet, not `AGENTS.md`.

## 2. Feature, bug, or task in an existing project

Same sequence every time. The only difference is the packet.

| Kind | Packet | Extra |
|---|---|---|
| Feature | GitHub issue (Ready), or `docs/tasks/<id>.md` | Move board `Ready` → `In progress` on pickup |
| Bug | GitHub issue labeled `bug` | Brief it in chat first; do not let an agent open a bug issue on its own |
| Small leftover | `docs/BACKLOG.md` one-liner, or a 20-line `docs/tasks/<id>.md` | Skip a board if the project does not have one |

Then:

1. **Rewrite `docs/NOW.md`** (or tell the agent to, as the first act). Intent, Open, Do not, Next, pointer to the packet. Overwrite, do not append.
2. **Launch inside the repo**, not from `$HOME`:

```bash
cd ~/Projects/{{name}}    # charispdf or CharisRemote
grok                      # or claude
```

3. **First message** — the job, not “load the project”:

```
Read docs/NOW.md. Then: {{one sentence}}.
```

Examples:

```
Read docs/NOW.md. Then: pick up issue #21 (Ready) and discuss the open UX questions with Grok before coding.
```

```
Read docs/NOW.md. Then: implement the Terms/Privacy pages from docs/BACKLOG.md. Do not reopen the audit-fix.
```

```
Read docs/NOW.md. Then: archive 1.3.0 when I give you the build number. Do not bump CURRENT_PROJECT_VERSION until then.
```

Resume the existing session if it is the same thread. A new instance is for a full context window or a different role (implement vs review).

## 3. What not to do

- Do not launch from `~` and say “load CharisRemote.”
- Do not `@` `AGENT-NOTES`, `BUILD-PLAN`, or the archive.
- Do not type “get familiar with the repo.”
- Do not start work that is still `Backlog`. Ready is the gate.
- Do not put the diary in `NOW.md`. Quotes and evidence go in `docs/tasks/<id>.log.md`.

## 4. When the task ends

The agent rewrites `NOW.md` (idle, or the next Ready item). A choice that must outlive the task becomes one `docs/decisions/D-NNN-slug.md` plus one index line. You move the board to Done.

Filled samples: `examples/` in this kit. In-repo protocol: `docs/WORKFLOW.md`.
