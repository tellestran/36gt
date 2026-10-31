# gt36 in Claude Code

gt36 skills use shared tool names. In Claude Code, read this file once per session, then map each shared name in a skill with the tables below. `<gt36>` means the folder that holds `skills/` and `docs/`. If a shared field has no row, drop it and say so. Never pass a field or value the tool schema rejects.

## Tools

| Shared instruction | Claude Code |
| --- | --- |
| `Task` | `Agent` |
| `AskQuestion` | `AskUserQuestion`. In `-p` mode it is absent, so ask in plain text. |
| Open a todolist | Use `TodoWrite` if the session offers it. Otherwise keep the list in your reply and update it at each phase. |
| Run another gt36 skill (`/name` or "the **name** skill") | `Read` `<gt36>/skills/<name>/SKILL.md` and follow it. The `Skill` tool refuses most gt36 skills. When you tell the user to type it, write `/gt36:<name>`. |
| `Grep`, `Glob` | `Bash` with `rg` when the session has no `Grep` or `Glob`. |

## Subagent fields

| Shared instruction | Claude Code |
| --- | --- |
| `subagent_type: generalPurpose` | `subagent_type: "general-purpose"` |
| `gt36-agent`, `comment-sicko` | `gt36:gt36-agent`, `gt36:comment-sicko` |
| `readonly: true` | `subagent_type: "Explore"`. It keeps `Bash`, so the brief also says "do not edit files", and you check `git status` afterward. |
| `readonly: false` | `general-purpose`. It keeps the session's MCP servers. |
| `environment: "cloud"` | `isolation: "worktree"`. Name the branch or commit the worker checks out. |
| `environment: "local"` | Omit. |
| `cloud_base_branch: <b>` | Tell the worker to check out `<b>` first. |
| `run_in_background: true` | Omit. Subagents run in the background by default and notify you when they finish. |
| `resume` | Spawn a new agent and paste the earlier result into its brief. |
| `file_attachments` | List the paths in the brief. |

## Models

| Shared instruction | Claude Code |
| --- | --- |
| `claude-<family>-<version>-<effort>[-fast]`, such as `claude-opus-5-5-xhigh` | `model: "opus"` and `effort: "xhigh"`. `model` takes only `opus`, `sonnet`, `haiku`, `fable`. Drop `-fast`. |
| A `grok-*`, `gpt-*`, or other non-Claude slug | Omit `model` so the agent runs on the session model, and say so. |
| `auto` or `inherit-parent` | Omit `model`. |
| `.gt36/codex.json` | Read the bundled and workspace configuration. Use only supported model and effort values; otherwise inherit the session model and report the limitation. |
| A role value `<model> <effort>`, such as `opus high` | `model: "opus"` and `effort: "high"`. |

## setup-gt36

Use the bundled [setup skill](../../skills/setup-gt36/SKILL.md) and `.gt36/codex.json`. Keep existing choices and validate overrides against this session's `Agent` tool schema. Do not create a second model rule file.

## Paths

Resolve bundled skills from their actual frontmatter names and paths under `<gt36>/skills/`. Project skills live under `.claude/skills/` and personal skills under `~/.claude/skills/`. Read the installed local files and give local file links; do not invent marketplace or repository URLs.

Use only workspace-scoped session history made accessible by the runtime. Report unavailable history without searching unrelated projects.

## Unavailable features

Use the forge CLI for PR operations when available and authorized. Treat review-bot comments as ordinary review threads. A worktree isolates source files, not the whole runtime. If an instruction needs an unavailable service, cloud machine, scheduler, or tool, state the limitation and continue independent work. Do not invent a service or translate it into a new automation layer.
