# gt36 for Codex

gt36 is a collection of engineering skills adapted for Codex: understand the code, design a change, implement it, and verify the result. The focus is simple code, root-cause analysis, and evidence that the change works.

## install

This checkout includes a [local marketplace](./.agents/plugins/marketplace.json) named `gt36-local` that points to the plugin at the repository root. From this checkout, register the source:

```bash
codex plugin marketplace add .
```

Then open the plugin directory in the desktop app, select `gt36 local`, and install `gt36`. Start a new Codex chat to use the installed skills. See the official [plugin packaging and local installation documentation](https://developers.openai.com/plugins/build/plugins) for client-specific steps. Installing the upstream plugin does not include this fork's changes.

To use the checkout without installing it, ask Codex to read the skill's actual path and the [Codex adapter](./docs/harness/codex.md):

```text
Read skills/gt36-mode/SKILL.md and docs/harness/codex.md, then fix <issue> and verify the result.
```

Use an absolute checkout path when working from another project. Public skill names come from frontmatter; directory names match the bundled public skill names.

## get started

1. Use [`setup-gt36`](./skills/setup-gt36/SKILL.md) to configure model roles, effort, and worker limits in your project's `.gt36/codex.json`.
2. Use [`gt36-mode`](./skills/gt36-mode/SKILL.md) with a concrete goal and a way to verify it.
3. Ask [`gt36-help`](./skills/gt36-help/SKILL.md) when you need help choosing a skill.

The [bundled configuration](./.gt36/codex.json) inherits the current model and reasoning effort. Its worker limit is two, subject to the session's available capacity and permission to delegate. Setup preserves existing choices and changes only requested fields. It does not change Codex's personal `config.toml`.

Examples below use public skill names. Select the skill exposed in your installed catalog; a slash-command example does not guarantee that command is installed in the current chat.

## usage

On Codex, [`gt36-mode`](./skills/gt36-mode/SKILL.md) applies the engineering principles with native Codex tools. The [adapter](./docs/harness/codex.md) skips the inherited playbook router, automatic delivery steps, and background execution procedures. The plugin does not install an automation layer.

For a feature proposal, use [`tech-leader`](./skills/tech-leader/SKILL.md) to compare approaches and prepare an architecture and implementation plan, then stop for review. [`architect`](./skills/architect/SKILL.md) produces a design and waits for approval; [`applier`](./skills/applier/SKILL.md) implements an approved design or a simple, sufficiently specified change. These skills do not authorize commits, pushes, PR creation, or merging on their own.

```text
/gt36-mode the export writes duplicate rows when a retry lands mid-run. Reproduce, fix, and verify.
/tech-leader compare approaches and plan CSV export with current filters. Do not implement.
/explainer how do we cancel runs? Check for an n+1 lookup.
/interrogate review this diff and prove actionable findings.
/applier implement docs/designs/import-pipeline.md; I approve this version.
```

## skills

Use a skill directly when its purpose matches the task. Availability of delegation, models, and live verification depends on the current session; the Codex adapter takes precedence over inherited platform instructions.

<details>
<summary>all skills</summary>

| skill | use it when |
|---|---|
| [`/gt36-mode`](./skills/gt36-mode/SKILL.md) | default entry point for any non-trivial task. |
| [`/gt36-help`](./skills/gt36-help/SKILL.md) | you're new to gt36, or unsure which skill or principle fits. finds out what you're trying to do, answers that part, and hands you a prompt to type. runs only when you type `/gt36-help`. |
| [`/in-your-own-words`](./skills/in-your-own-words/SKILL.md) | you want the agent to play back your goal and the problem before it starts. runs only when you type `/in-your-own-words`. |
| [`/sharpen`](./skills/sharpen/SKILL.md) | you want a prompt made deeper, not bigger, before you send it: what a top professional would do that you didn't ask for, the raw material they'd start from, their methods and tools, and an outside check that the result is right. hands back the rewritten prompt and leaves your choices open. doesn't do the task. runs only when you type `/sharpen`. |
| [`/explainer`](./skills/explainer/SKILL.md) | you want a walkthrough of how a subsystem works. |
| [`/investigator`](./skills/investigator/SKILL.md) | you want a read-only explanation or recommendation. uses local code, history, and user-supplied ticket context; follows the Codex adapter for available tools. |
| [`/recall`](./skills/recall/SKILL.md) | you're starting or resuming work and want your recent context on a topic rebuilt from your own chat history and the shared record, handed back as a tight current-state brief. |
| [`/ketchup`](./skills/ketchup/SKILL.md) | you want what changed in this chat since your last message, in plain words and small visuals, with every item waiting on you listed and decidable from the reply alone, even when that makes the reply long. |
| [`/ketchup-video`](./skills/ketchup-video/SKILL.md) | you want the `/ketchup` catch-up as a short subtitled video of slides, with the calls waiting on you as text under it. |
| [`/blast-radius`](./skills/blast-radius/SKILL.md) | you have a small-looking change and want to know what else it could break, with the one fact it's safe because of proven by running code, not asserted. |
| [`/tech-leader`](./skills/tech-leader/SKILL.md) | compare approaches and plan one feature, then stop for review. |
| [`/architect`](./skills/architect/SKILL.md) | you want an architecture design document with usage, types, and module shape. always stops for human approval without implementing. |
| [`/applier`](./skills/applier/SKILL.md) | you want code implemented using real project precedents and suitable API contracts. missing-contract resolutions need your acceptance; tests change only when explicitly requested. |
| [`/arena`](./skills/arena/SKILL.md) | you want N parallel attempts at the same thing, then to grab the best parts of each. |
| [`/swarm`](./skills/swarm/SKILL.md) | you want N parallel workers across different slices or races, then one aggregated report. |
| [`/tester`](./skills/tester/SKILL.md) | review a feature or diff with blast-radius, interrogate, five engineering principles, and a plain-language report through bro. verifies failures without automatically fixing code or editing repository tests. |
| [`/interrogate`](./skills/interrogate/SKILL.md) | you have a diff and want different models to try to break it, keeping only findings a run proves. |
| [`/automate-me`](./skills/automate-me/SKILL.md) | you want your own `-mode` skill, drafted from how you've actually worked. |
| [`/make-bot-ui`](./skills/make-bot-ui/SKILL.md) | you want a page or dashboard whose buttons wake a Grok Bot over a webhook, including the sender-key handoff and Tailscale. |
| [`/setup-gt36`](./skills/setup-gt36/SKILL.md) | configure model roles, effort, and worker limits in `.gt36/codex.json`, preserving existing choices. |
| [`/reflect`](./skills/reflect/SKILL.md) | a long task landed and you want the recipe captured as a skill edit. |
| [`/correct`](./skills/correct/SKILL.md) | you keep correcting agents for the same mistakes. mines history for mistake classes, fixes each at the highest level that works (architecture, then types, lint, and ci, then tests, with docs last), and keeps a table pairing each rule with what enforces it. |
| [`/teach`](./skills/teach/SKILL.md) | you want to actually understand a change or subsystem, not just have it summarized. runs explainer + investigator and weaves one plain explanation, built up diagram by diagram. |
| [`/tdd`](./skills/tdd/SKILL.md) | you explicitly want a focused failing test before the fix. inside applier, a cheap test path alone does not authorize test edits. |
| [`/benchmark-checklist`](./skills/benchmark-checklist/SKILL.md) | you ran a benchmark or measured a speedup or regression. vets the number (limiter, tuning, errors, repeat runs, end-to-end relevance) before you report or act on it. |
| [`/no-comments`](./skills/no-comments/SKILL.md) | strip comments before review; spawns Comment Sicko, fixes accepted findings, offers encodings for claimed constraints. |
| [`/typescript-best-practices`](./skills/typescript-best-practices/SKILL.md) | you're reading or editing typescript. grounds the type-system-discipline principle in syntax. |
| [`/figure-it-out`](./skills/figure-it-out/SKILL.md) | you need an auditable approach to a large change; explicit plan-only mode only prepares the plan. |
| [`/show-me-your-work`](./skills/show-me-your-work/SKILL.md) | you want a reviewable decision trail. logs decisions to a tsv you can commit. |
| [`/create-verification-skill`](./skills/create-verification-skill/SKILL.md) | your project has no scripted way to prove app behavior. generates a project-local verify skill with a feature map, for any language or platform. |
| [`/maintain-verification-skill`](./skills/maintain-verification-skill/SKILL.md) | your verify skill's feature map has drifted from the app. source wave + one live pass, at most one PR of proven corrections. |
| [`/create-formal-verification-skill`](./skills/create-formal-verification-skill/SKILL.md) | tests can't reach the case that breaks a race, retry, state machine, parser, or unsafe code. verifies one property with the tool that fits and generates a project-local skill to recheck it on every change. |
| [`/maintain-formal-verification-skill`](./skills/maintain-formal-verification-skill/SKILL.md) | code your formal verification covers has changed since it was last checked. updates the model, harness, or properties to match the code, reruns, and says whether each failure is drift in the verification or a bug in the code. |
| [`/statechart`](./skills/statechart/SKILL.md) | someone who owns the behavior needs to see it, not read a spec. checks a tla+ model, draws what the user sees as a mermaid statechart, marks each state that breaks a rule, and gives the shortest path to each one. |
| [`/unslop`](./skills/unslop/SKILL.md) | you're cleaning up writing. removes AI tells. |
| [`/bro`](./skills/bro/SKILL.md) | you want the last message restated in plain human language, no jargon. |
| [`/technical-writing`](./skills/technical-writing/SKILL.md) | layered doc standard (Diátaxis + Google developer style + STE + Global English) for docs, RFCs, readmes, PR descriptions, commit messages. |

</details>

## principles

twenty-four short skills, one principle each. `gt36-mode` indexes them inline and reads that index at task start. the standalone files are there so other skills can reference a principle by name, and so the index can point at the full rule for each.

<details>
<summary>all twenty-four principles</summary>

| principle | group | rule |
|---|---|---|
| [laziness-protocol](./skills/principle-laziness-protocol/SKILL.md) | core | Bias toward deletion and the smallest change that solves the problem. |
| [foundational-thinking](./skills/principle-foundational-thinking/SKILL.md) | core | Apply before writing logic: choosing core types and data structures, sequencing scaffold-vs-feature work, asking what concurrent actors share. Get the data structures right so downstream code becomes obvious. |
| [redesign-from-first-principles](./skills/principle-redesign-from-first-principles/SKILL.md) | core | Redesign as if the requirement had been a foundational assumption from day one, instead of bolting it on. |
| [attack-the-premise](./skills/principle-attack-the-premise/SKILL.md) | core | Apply when two or more fixes that share one premise have failed the same gate. Write the premise down and test it directly before you write another fix that assumes it. |
| [subtract-before-you-add](./skills/principle-subtract-before-you-add/SKILL.md) | core | Remove dead weight, redundant validators, and stub references first, then build on the simpler base. |
| [minimize-reader-load](./skills/principle-minimize-reader-load/SKILL.md) | core | Count layers between question and answer, and hidden state in the reader's head; collapse one-caller wrappers and shrink mutable scope. |
| [outcome-oriented-execution](./skills/principle-outcome-oriented-execution/SKILL.md) | core | Apply during planned rewrites and migrations with explicit phase boundaries. Converge on the target architecture; don't preserve smooth intermediate states with throwaway compatibility code. |
| [experience-first](./skills/principle-experience-first/SKILL.md) | core | Choose user delight over implementation convenience; ship fewer polished features over more rough ones. |
| [exhaust-the-design-space](./skills/principle-exhaust-the-design-space/SKILL.md) | core | Build 2-3 competing prototypes and compare side by side before committing. |
| [build-the-lever](./skills/principle-build-the-lever/SKILL.md) | core | Apply to any non-trivial work, not just bulk work: edits, migrations, analyses, checks. Build the tool that does it or proves it (codemod, script, generator, or a skill your subagents follow) instead of working by hand. The tool is the artifact a reviewer can rerun. |
| [model-the-domain](./skills/principle-model-the-domain/SKILL.md) | architecture | Encode the domain in a structure instead of scattered conditionals. |
| [boundary-discipline](./skills/principle-boundary-discipline/SKILL.md) | architecture | Concentrate guards at system boundaries (CLI, config, network, external APIs); trust internal types and keep business logic in pure functions. |
| [type-system-discipline](./skills/principle-type-system-discipline/SKILL.md) | architecture | Make illegal states unrepresentable, brand semantic primitives, parse external data at boundaries, exhaust variants, derive from authoritative schemas. |
| [make-operations-idempotent](./skills/principle-make-operations-idempotent/SKILL.md) | architecture | Converge to the same end state regardless of partial prior runs. |
| [migrate-callers-then-delete-legacy-apis](./skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md) | architecture | Migrate callers and delete the old API in the same wave instead of preserving compatibility layers. |
| [separate-before-serializing-shared-state](./skills/principle-separate-before-serializing-shared-state/SKILL.md) | architecture | Eliminate the sharing first; serialize structurally only when one shared writer is a real invariant. |
| [prove-it-works](./skills/principle-prove-it-works/SKILL.md) | verification | Apply after completing a task, before declaring done. Verify against the real artifact (run the feature, read the actual value, inspect the diff), not a proxy, self-report, or 'it compiles.'. |
| [fix-root-causes](./skills/principle-fix-root-causes/SKILL.md) | verification | Trace each symptom back to the first place the system went wrong and fix it there. A guard is a fix only when the state it handles is legitimate. |
| [sequence-verifiable-units](./skills/principle-sequence-verifiable-units/SKILL.md) | verification | Apply to multi-step work (sweeps, migrations, runs of similar edits) and to how you stack commits and PRs. Break work into small units that each end in a verifiable state, check each before the next, and order delivery so the sequence proves itself to a reviewer. |
| [test-behavior-not-implementation](./skills/principle-test-behavior-not-implementation/SKILL.md) | verification | Apply when you write, change, or keep a test. Assert what the code's users can observe, so the test fails when that behavior breaks and keeps passing when only the internals change. |
| [explain-the-number](./skills/principle-explain-the-number/SKILL.md) | verification | Apply before you trust, report, or act on a number you measured: a speedup, a regression, a throughput, a latency, or an eval result. Find what limits it, and rule out that it measured something other than the work you think. |
| [guard-the-context-window](./skills/principle-guard-the-context-window/SKILL.md) | delegation | Route bulk to subagents; keep summaries in the main thread, not raw payloads. |
| [never-block-on-the-human](./skills/principle-never-block-on-the-human/SKILL.md) | delegation | Proceed, present the result, let the human course-correct after the fact; reserve confirmation for irreversible actions. |
| [encode-lessons-in-structure](./skills/principle-encode-lessons-in-structure/SKILL.md) | meta | Encode the rule as a lint, metadata flag, runtime check, or script instead of more text. |

</details>

## agent roles

The bundled [gt36 agent](./agents/gt36-agent.md) and [Comment Sicko](./agents/comment-sicko.md) files are role prompts. They do not register custom Codex subagent types. When delegation is authorized and available, use the session's native tools and supply the relevant skill, adapter, goal, and completion criteria. Otherwise, work in the current chat.

## tools and compatibility

The [Codex adapter](./docs/harness/codex.md) maps shared tool names, model roles, and paths. Use available native tools for browser, UI, terminal, and repository verification. If a referenced tool is unavailable, report the limitation and use an appropriate available alternative.

The [guide](./docs/guide/README.md) retains inherited examples, including playbooks and overnight runs. On Codex, follow the adapter instead of those platform-specific procedures.


For optional knowledge-graph navigation in this checkout, see [Graphify setup and usage](./docs/harness/graphify.md).

## make it yours

Use [`automate-me`](./skills/automate-me/SKILL.md) to draft a personal mode skill from accessible, in-scope working history. Use [`setup-gt36`](./skills/setup-gt36/SKILL.md) to update `.gt36/codex.json`. Keep existing choices unless you explicitly want to change them; unavailable model overrides fall back to inheritance as described in the adapter.

## license

MIT
