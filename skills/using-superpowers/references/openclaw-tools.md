# OpenClaw Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill Reference | OpenClaw Equivalent |
|---|---|
| `Read` tool | `Read` (same) |
| `Write` / `Edit` tools | `Write` / `Edit` (same) |
| `Bash` tool | `exec` tool |
| `Task` tool (dispatch subagent) | `sessions_spawn(task: "...", mode: "run")` |
| `mcp__` tools | Not applicable — use OpenClaw native tools |

## Subagent dispatch

OpenClaw supports subagents via `sessions_spawn`:

```
sessions_spawn(
  task: "Your task instructions here",
  mode: "run",           # one-shot execution
  runtime: "subagent"    # uses configured subagent model
)
```

For persistent sessions: use `mode: "session"`.

Skills like `subagent-driven-development` and `dispatching-parallel-agents` work natively — replace any `Task(...)` invocations with `sessions_spawn(...)`.

## Git worktrees

OpenClaw agents use feature branches, not git worktrees. When a skill references `using-git-worktrees`, use standard branch workflow instead:

```bash
git checkout -b feature/<name>
# ... work ...
git checkout main && git merge feature/<name>
```

## Spec and plan locations

| Skill Default | OpenClaw Location |
|---|---|
| `docs/superpowers/specs/` | `specs/active/` |
| `docs/superpowers/plans/` | `scratch/plans/` |
