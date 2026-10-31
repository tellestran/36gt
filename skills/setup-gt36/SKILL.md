---
name: setup-gt36
description: Configure gt36 model roles, reasoning effort, and worker limits in .gt36/codex.json. Use for /setup-gt36, "configure gt36 models", "gt36 budget", or changing gt36 model choices.
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Setup gt36

Configure `.gt36/codex.json` using the [Codex adapter](../../docs/harness/codex.md). Keep model configuration separate from personal application settings.

## Steps

1. Read the bundled [configuration](../../.gt36/codex.json) and any workspace `.gt36/codex.json`. Existing choices are the starting point. If the workspace has no configuration, copy the bundled file when setup is requested.
2. Read the current session's supported model identifiers and reasoning efforts. Keep `inherit-parent` and `inherit` unless the user requests overrides. Never invent a model identifier or encode effort into a model name.
3. Apply only the requested role, effort, or `max_workers` changes. Keep unspecified fields and roles. Panel roles take nonempty lists; single roles take model strings. `auto` and `inherit-parent` mean omit a model override. Worker limits do not authorize delegation.
4. Validate the schema version, allowed setting and role names, positive integer worker limit, supported effort, and role value types using the adapter. Report invalid existing configuration rather than resetting it. An unavailable override falls back to inheritance with a clear explanation.
5. Write the workspace configuration and read it back. Summarize the changed choices and path. Skills read the file on their next use; setup does not change the current chat's model or application settings.

When relevant and absent, offer a project-local verification skill through `/create-verification-skill` or a formal verification skill through `/create-formal-verification-skill`. Create one only when the user requests it.
