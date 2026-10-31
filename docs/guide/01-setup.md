# Set up gt36

## Install the plugin

Follow the local marketplace instructions in the [README](../../README.md#install). This checkout contains the installation source; no public marketplace entry is assumed. For Claude Code tool mappings, see the [adapter](../harness/claude-code.md).

## Pick your models

Run the installed `setup-gt36` skill, or ask the agent to read [its local file](../../skills/setup-gt36/SKILL.md).

Setup reads the bundled and workspace `.gt36/codex.json`, preserves current choices, and changes only requested fields. Defaults inherit the current model and effort. Explicit overrides must be supported by the runtime. Set `max_workers` as a ceiling and use nonempty lists for panel roles. These settings do not authorize delegation.

## Accept the verification offers, or don't

At the end of setup, `/setup-gt36` looks for a way to prove app behavior in your project, either a `verify-*` skill or an existing harness. If it finds neither, it offers once to generate one with [`/create-verification-skill`](../../skills/create-verification-skill/SKILL.md).

Say yes and it writes `.agents/skills/verify-<app>/`, a project-local skill that teaches agents to drive your app the way a user does. It proves the skill works once before handing it over. Say no and setup moves on. You can run `/create-verification-skill` yourself any time. [Verify and ship](./06-verify-and-ship.md#create-a-project-verification-skill) covers it in depth.

If you're new to gt36, say yes. An agent that can check its own work keeps going until the check passes. An agent that can't hands every result back to you to check by hand. Of everything in this guide, the verification skill pays off the most.

Then setup offers once to generate a formal verification skill with [`/create-formal-verification-skill`](../../skills/create-formal-verification-skill/SKILL.md), unless the project already has one. Tests check the cases someone thought to write. A formal verification skill checks one key property of your code, such as "a job never runs twice at once", in every case within a stated scope. Say yes when your code has concurrency, retries, a state machine, or a parser, where a bug is costly and tests rarely reach the case that breaks. Otherwise say no, and run it later if that changes. [Verify and ship](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill) covers it.

Skills read the configuration on their next use; setup does not change the current chat model.

## Keep the cost in check

When delegated work is authorized, gt36 spends extra tokens on subagents and review panels. That's the price of the rigor. To spend fewer:

- Rerun `/setup-gt36` and pick a smaller reasoning budget or cheaper models. A strong model in the main chat with cheaper, faster models in the code roles is a good split.
- Set a role to `auto` or `inherit-parent` so it runs on the chat's own model.
- Shorten a panel list. Each entry runs one subagent.
- Save `/gt36-mode` for work that needs rigor. A small, obvious edit doesn't.

## Run your first task

Pick something real but small, and describe it the way you'd describe it to a colleague:

```text
/gt36-mode add a --json flag to this command. text output stays byte-identical. verify both.
```

On Codex, `gt36-mode` applies the engineering principles through native tools. Follow the [adapter](../harness/codex.md); inherited playbooks and automatic delivery steps do not run.

Invoke the skill again when needed in a new task. Availability in the skill menu depends on the installed client.

Next: [Route work through `/gt36-mode`](./02-gt36-mode.md).
