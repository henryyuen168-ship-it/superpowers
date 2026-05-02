# OpenClaw Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill Reference | OpenClaw Equivalent |
|---|---|
| `Read` tool | `read` |
| `Write` / `Edit` tools | `write` / `edit` |
| `Bash` tool | `exec` |
| `Task` tool (dispatch subagent) | `sessions_spawn(task: "...", mode: "run")` |
| `TodoWrite` | `update_plan` |
| `WebFetch` | `web_fetch` |
| `WebSearch` | `web_search` |
| `mcp__` tools | Not applicable — use OpenClaw native tools |

## Subagent dispatch

OpenClaw supports subagents via `sessions_spawn`:

```text
sessions_spawn(
  task: "Your task instructions here",
  mode: "run",
  runtime: "subagent"
)
```

For persistent sessions, use `mode: "session"`.

Skills like `subagent-driven-development` and `dispatching-parallel-agents` work natively — replace any `Task(...)` invocations with `sessions_spawn(...)`.

## Common OpenClaw-native coordination tools

| Need | OpenClaw tool |
|---|---|
| spawn a clean worker | `sessions_spawn` |
| inspect/steer spawned workers | `subagents` |
| message another visible session | `sessions_send` |
| track the live plan | `update_plan` |
| schedule a reminder/follow-up | `cron` |
| inspect another session's recent context | `sessions_history` |

## Git worktrees

OpenClaw agents use feature branches by default, not mandatory git worktrees. When a skill references `using-git-worktrees`, prefer the branch-first workflow unless the local repo policy explicitly requires a worktree:

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

## Notes

- OpenClaw may load and read skill files directly rather than requiring a dedicated skill invocation command.
- When a skill references foreign-platform activation rituals, translate the intent, not the exact ceremony.
- If a cross-platform reference conflicts with OpenClaw-specific instructions, prefer the OpenClaw mapping.
