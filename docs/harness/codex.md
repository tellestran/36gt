# Codex compatibility

Read this file once before using any gt36 skill in Codex. Apply these compatibility mappings to nested skills and references too. User instructions, repository guidance, and runtime permissions take precedence. `<gt36>` is the installed plugin root containing this file and `skills/`.

## Configuration

Read the bundled [Codex configuration](../../.gt36/codex.json). If the current workspace has its own `.gt36/codex.json`, overlay its scalar fields and individual role entries on the bundled configuration. A panel list replaces that entire panel. Do not change Codex's personal `config.toml`.

The configuration contains:

- `schema_version`: `1`.
- `max_workers`: `2` by default; a ceiling capped by the runtime's available capacity, not a requirement to delegate.
- `effort`: `inherit` by default; otherwise use only an effort supported by the current tool and selected model.
- `roles`: the exact role labels in shared skills. Single roles take a model identifier; panel roles take a nonempty list. Missing roles inherit the bundled values. All bundled roles inherit the current model.

`auto` and `inherit-parent` mean omit the model argument. Do not fall back to the Claude/Grok defaults in shared skills. Explicit models must be confirmed available in the current session and permitted by the tool schema. Pass model and effort separately. If an override is unsupported, inherit the parent and report the limitation once. Do not repeatedly call a tool with rejected model names.

Reject malformed JSON, unknown setting/role names, unsupported schema versions, nonpositive/noninteger worker limits, empty panels, or model values of the wrong type before applying settings. Report the file and error; do not silently overwrite it. Configuration does not grant permission to delegate, override models, or perform external actions.

## Tools and paths

| Shared instruction | Codex mapping |
| --- | --- |
| `Task`, `resume`, custom `subagent_type` | Use the session's actual subagent create/follow-up/wait/interrupt tools. Treat custom agents as prompt roles, not tool enum values. If unavailable or disallowed, work directly. |
| `gt36-agent` | Give the worker the relevant skill path, this adapter, goal, file ownership, and completion criteria. Do not search unrelated plugin caches. |
| `AskQuestion`, todo tools | Use an available equivalent or concise prose. |
| Other gt36 skills | Read `<gt36>/skills/<name>/SKILL.md`. Use the invocation name actually exposed in the skill catalog. |
| `.agents/skills/` | Project skills: `.agents/skills/`. Personal skills: `$CODEX_HOME/skills/`, default `~/.codex/skills/`. |
| Model configuration | `.gt36/codex.json` as described above. |
| `create-skill` | Use the available Codex `skill-creator` skill. |
| `control-ui`, `control-cli` | Use an available browser, native UI, terminal driver, or project verification skill. State when live verification is unavailable. |
| `deslop` | Review the actual diff for redundant code; use bundled review skills when relevant. Do not claim an unavailable skill ran. |
| Session history | Use exposed Codex history tools scoped to this project or supplied context. Never search unrelated chats. If history is inaccessible, report the gap. |
| `environment: cloud` | Use an available isolated runtime when needed. A worktree isolates files but does not isolate ports, processes, or databases. |

Only delegate when useful and allowed. Queue seats above the concurrency limit. A reviewer should use a fresh context; prefer a different model family when available, but do not fail solely because it is unavailable. State when independent review could not be performed.

## No inherited playbook layer

On Codex, do not load or execute `skills/gt36-mode/playbooks/`. Do not translate `/loop`, autopilot, orchestration, Shipping, or cleanup procedures. Do not register automations or promise background execution from these instructions.

For `gt36-mode`, use the applicable leaf engineering principles and carry out the user's task with native Codex tools. Skip its inherited router, mandatory playbook sequence, and automatic delivery steps. For a skill that only forwards to an inherited playbook (such as `tech-leader`), carry out its stated purpose directly: a planning request produces a plan and stops; an implementation request produces and verifies the change. Do not introduce an additional workflow layer.

For `gt36-help`, explain available skills and this compatibility boundary. Shared skill instructions do not themselves authorize commits, pushes, PR creation, messages, tracker updates, merging, or deletion. Follow the user's requested scope and the session's existing permissions.

## setup-gt36 on Codex

Read the bundled and project configuration. If no project file exists, copy the bundled configuration to `<workspace>/.gt36/codex.json` when setup is requested. Never overwrite existing choices on a rerun. Change only the requested fields, retain unspecified roles, validate the result, and show a brief summary. Use inherited model/effort when no explicit choice is supplied; no budget interview is required. Read the configuration again on the next gt36 task; no persistent model change is made to Codex itself.
