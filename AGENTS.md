# Repository guidance

## Purpose and scope

This repository contains the `gt36` engineering-skills plugin, adapted from pstack for Codex. It primarily ships Markdown instructions, JSON configuration, agent-role prompts, and a few executable verification helpers. There is no root application build or root package manager project.

Keep the Codex integration small: configuration and compatibility mappings, without introducing a replacement playbook, automation service, or orchestration framework. The automation pack was deliberately removed; do not restore it unless requested.

## Repository map

- `plugin.json`: portable plugin identity and OpenAI-specific metadata, including the onboarding skill.
- `skills/*/SKILL.md`: discoverable skills; optional references, scripts, and assets live beside each skill.
- `agents/*.md`: role prompts. These files do not register custom Codex subagent types.
- `docs/harness/codex.md`: Codex tool, model, path, and configuration mappings.
- `docs/harness/claude-code.md`: Claude Code compatibility mappings.
- `.gt36/codex.json`: the bundled Codex model configuration currently present on disk.
- `docs/guide/` and `README.md`: user documentation, including inherited examples.
- `skills/gt36-mode/scripts/`: Bun-based PR watcher, plan checker, and worktree audit helper.
- `skills/statechart/scripts/`: TypeScript statechart tooling and its verification fixtures.

## Naming and paths

Use `gt36` consistently in plugin metadata, configuration, file names, and documentation. The mode skill lives at `skills/gt36-mode/`, help at `skills/gt36-help/`, setup at `skills/setup-gt36/`, and the agent role at `agents/gt36-agent.md`. The adapter reads `.gt36/codex.json`.

Resolve public skill names from frontmatter and verify local paths. When changing a name, reconcile the directory, frontmatter, manifest, links, adapter, and configuration together.

## Instructions for Codex

### Load the repository context

- Read this file and `docs/harness/codex.md` before using a bundled skill. Apply this repository guidance when inherited platform instructions conflict with the Codex adapter.
- Use `.gt36/codex.json` as the configuration in this checkout. Do not create a second configuration or silently reset the existing one based on a public skill name.
- Inspect the relevant files and current Git status. Use actual on-disk paths from the repository map; resolve a skill's public name from its frontmatter rather than guessing its directory from that name.
- Read only the skills and references needed for the current request. Reading skill files to edit or review them does not activate their operational instructions.

### Apply skills when requested

These are local file entry points, not promises that a slash command is installed in the current Codex session:

| User request | Local entry point |
| --- | --- |
| Use `gt36-mode` | `skills/gt36-mode/SKILL.md`; follow applicable principles and skip inherited playbook routing. |
| Configure `setup-gt36` | `skills/setup-gt36/SKILL.md`; use the adapter's Codex setup branch and preserve existing choices. |
| Explain a subsystem | `skills/explainer/SKILL.md` when requested or useful for a substantial explanation. |
| Investigate historical rationale | `skills/investigator/SKILL.md`; distinguish local Git evidence from unavailable external context. |
| Design with `architect` or `tech-leader` | `skills/architect/SKILL.md` or `skills/tech-leader/SKILL.md`; deliver the requested design and stop without implementation. |
| Implement with `applier` | `skills/applier/SKILL.md`; use the agreed scope and applicable Codex mappings. |
| Run `tester` | `skills/tester/SKILL.md`; report findings and evidence without automatically applying fixes. |
| Test-first implementation with `tdd` | `skills/tdd/SKILL.md`. |
| Configure help for `gt36-help` | `skills/gt36-help/SKILL.md`; describe the actual Codex capabilities and naming limitations. |

Honor explicit skill invocation policies. A simple change does not require chaining explainer, architect, arena, tester, and applier. Use ordinary Codex execution when no specialized skill is needed. Do not invoke a missing skill or tool just because an inherited document names it.

### Carry out the request

- For an implementation request, complete the scoped edits and relevant verification. For a question or review, return the findings; for a plan-only request, deliver the plan and stop.
- Keep routine reversible decisions within the authorized task. Ask only when missing information or a material unresolved decision blocks progress; reuse decisions the user has already made.
- Use native Codex tools with their actual schemas. Model settings and role prompts are not permission to spawn workers, bypass runtime limits, or create separate sidebar chats.
- Do not add background runs, automations, mandatory playbook stages, or automatic delivery steps. Do not restore removed automation files.
- Verify the result with checks proportionate to the change. Report a missing runtime, dependency, unresolved path, or unavailable live check directly; do not replace evidence with an agent's self-report.
- Write all repository documentation in English, including guides, skill instructions, examples, and documentation updates. Keep conversational replies in the user's language.
- Respond in the user's language. Keep progress updates and the final report concise, with the changed files, relevant checks, and remaining limitations when they matter.

## Editing skills and configuration

- Read the relevant files and their callers before editing. Shipped skills are product content, not repository-wide instructions automatically activated by reading them for maintenance.
- On Codex, apply `docs/harness/codex.md`. Do not run the inherited playbooks or translate their automatic delivery and background-execution steps.
- Keep skill frontmatter valid YAML with a meaningful `name` and `description`. Preserve existing invocation policy and platform-specific metadata unless the task changes them.
- Resolve bundled references relative to the current file. Verify links, manifest paths, and referenced scripts after moving or renaming anything.
- Keep model configuration separate from Codex's personal `config.toml`. Do not write user-level configuration or installed plugin caches unless explicitly requested.
- Default to inherited model and effort. Use explicit overrides only when confirmed available and allowed by the runtime. `max_workers` is a ceiling, not a requirement to delegate.
- Keep shared engineering guidance portable. Put platform substitutions in the relevant adapter instead of duplicating full workflows across platforms.
- Preserve attribution and license metadata. Do not invent repository URLs, published marketplace entries, or claims that installation has been verified.

## Verification

Use checks appropriate to the files changed. Documentation and manifest edits normally need JSON/YAML parsing, reference checks, and review of the actual content; they do not require running every helper suite.

For PR watcher or plan-checker changes, run from `skills/gt36-mode/scripts/`:

```sh
bun install --frozen-lockfile
bun run typecheck
bun test watch-pr check-plan.test.ts
```

Install dependencies when absent or changed. Missing `bun-types` produces TS2688 in `watch-pr/tsconfig.json`; resolve the declared dependencies rather than removing the type configuration. Keep `bun.lock` consistent with intentional dependency changes.

For statechart changes, inspect `skills/statechart/scripts/package.json` and its runtime requirements first. Run its declared tests from that directory using a Node runtime capable of executing the TypeScript sources. Do not treat committed model-checker outputs as newly verified evidence.

Do not run `worktree-audit.sh` cleanup procedures, invoke the live GitHub watcher, or access external accounts merely to validate documentation. Distinguish static validation, local tests, and an actual Codex plugin installation in reports.

## Working-tree discipline

- Preserve user edits and unrelated files. Inspect Git status before making changes.
- Do not assume a commit or remote exists; this repository was initialized before its first commit.
- Do not stage, commit, push, publish, or modify remotes unless requested. Use `codex/` for a new branch when one is needed, unless the user specifies another name.
- Keep dependencies, caches, temporary output, and secrets out of source control. Follow `.gitignore`; do not commit `node_modules/`.
- Report the concrete change, relevant checks, and any remaining path or runtime limitations. Do not claim unrun checks passed.

## Optional Graphify context

The project-local Graphify skill lives at `.agents/skills/graphify/SKILL.md`.
Read [GRAPIFY.md](./GRAPIFY.md) for setup, usage, graph scope, and update commands
before using Graphify in this repository. Use `.venv-graphify/bin/graphify` when
`graphify` is not on PATH. Repository guidance and the Codex
adapter take precedence over the upstream skill's platform assumptions.
The initial `graphify-out/` graph covers code helpers only, not Markdown skills
or role prompts. Check source files for missing context. Refresh the graph when
its source changes; do not rebuild it or install hooks merely to answer a question.
