---
name: gt36-help
description: Guides users through gt36 setup, /gt36-mode, and picking the skill, playbook, or principle for a task. Type /gt36-help with a question.
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# gt36 help

Answer the user's question about gt36, hand them a prompt they can send, and link the file the answer came from. For a help question, don't start the work. The user asked how, and a gt36 run spends real tokens, so let them send the prompt.

A message that asks for work, such as "use gt36 to fix this bug", is not a help question. Read [`gt36-mode`](../gt36-mode/SKILL.md), do the work under it, and explain the applicable runtime behavior.

This file maps questions to skills and guide pages. Read the relevant local file before quoting it and link its actual path. Do not invent a public repository or marketplace entry.

## Find out what they need

Infer the need from the message and the conversation. A named situation, such as "which skill reviews a PR?", goes straight to its section. If the need is still unclear, ask one multiple-choice question with these options, then answer only the section they pick:

- Get set up
- Start a task with `/gt36-mode`
- Pick a skill for a situation
- Fix a run that went wrong
- Make gt36 my own

Check the state that changes the answer, and mention it only when it does:

- No workspace `.gt36/codex.json` means the bundled configuration supplies the defaults.
- No `verify-*` skill or other app harness in the project means agents have no scripted way to drive the app. Mention `/create-verification-skill` when the question is about proving a change works.
- No formal verification skill in the project means no property is checked past what tests reach. Mention `/create-formal-verification-skill` when the question is about a race, an invariant, or another case tests rarely hit. When one exists and the code it covers changed after the commit it records as last in sync, mention `/maintain-formal-verification-skill` if the question touches that code.

When the workspace configuration is missing and it matters, ask whether the user wants to pick a model for each role and a reasoning budget now. It matters when the user is new, the question is about setup or cost, or the answer depends on which models run. Ask at most once per chat. If the need is also unclear, ask both questions together. Offer two choices:

- Now: give them `/setup-gt36` to type, and answer their question too.
- Later: answer their question, and add one line saying every role keeps its default model until they run `/setup-gt36`.

## Get set up

Follow the local marketplace instructions in the [README](../../README.md#install), then run [`setup-gt36`](../setup-gt36/SKILL.md) if model or effort overrides are needed. It preserves existing `.gt36/codex.json` choices and validates changes against the current runtime. Defaults inherit the current model and effort.

The skills use the Agent Skills format. On Codex, the [adapter](../../docs/harness/codex.md) supplies tool, model, and path mappings. For Claude Code, read [its adapter](../../docs/harness/claude-code.md). Do not assume that custom modes, schedulers, or custom subagent types exist.

## Start a task with `/gt36-mode`

Give a goal and an observable completion check. On Codex, use the principles with native tools and skip inherited playbook routing. Invocation names come from frontmatter; local directory names can differ. `agents/gt36-agent.md` is role guidance, not a registered Codex subagent type.

Read [`references/prompting.md`](references/prompting.md) for examples. Use only available tools and delegate only when authorized.

## Pick a skill

The default answer is `/gt36-mode`, which runs most of the others when its steps need them. Name a skill directly when the user wants more or less of something than the playbook gives. Read the skill before you recommend it, and give one example prompt.

| The user wants to | Skill |
|---|---|
| Do any non-trivial task with rigor | [`/gt36-mode`](../gt36-mode/SKILL.md) |
| Compare approaches and get a design plus implementation plan for one feature without coding | [`/tech-leader`](../tech-leader/SKILL.md) |
| Check that the agent understood their goal and the problem before it starts | [`/in-your-own-words`](../in-your-own-words/SKILL.md) |
| Get a prompt rewritten deeper, not bigger, before sending it: what a top professional would add, their raw material and methods, and an outside check. It returns the prompt and doesn't do the task | [`/sharpen`](../sharpen/SKILL.md) |
| Know how code works now, or where new code should live | [`/explainer`](../explainer/SKILL.md) |
| Investigate a read-only question: explanation or recommendation, with local history and ticket context when relevant | [`/investigator`](../investigator/SKILL.md) |
| Understand a change or subsystem, explained plainly | [`/teach`](../teach/SKILL.md) |
| Catch up on their own recent work on a topic | [`/recall`](../recall/SKILL.md) |
| Catch up on what changed in this chat since their last message, in plain words and small visuals, with every item waiting on them listed and decidable from the reply alone | [`/ketchup`](../ketchup/SKILL.md) |
| Catch up like `/ketchup`, in a short subtitled video of slides, with the waiting calls as text under it | [`/ketchup-video`](../ketchup-video/SKILL.md) |
| Know what a small diff could break outside itself | [`/blast-radius`](../blast-radius/SKILL.md) |
| Design architecture and review a document before any implementation | [`/architect`](../architect/SKILL.md) |
| Implement from an approved design or clear request, following real project precedents and API contracts; edit tests only when explicitly requested | [`/applier`](../applier/SKILL.md) |
| Get several attempts at one brief, merged into the best one | [`/arena`](../arena/SKILL.md) |
| Run parallel checks over slices, or race workers, as cloud agents | [`/swarm`](../swarm/SKILL.md) |
| Combine blast-radius and adversarial review, apply five engineering principles, and get a plain-language evidence report | [`/tester`](../tester/SKILL.md) |
| Have different models review a diff and try to break it | [`/interrogate`](../interrogate/SKILL.md) |
| Explicitly request a focused test-first bug fix when a practical local test exists | [`/tdd`](../tdd/SKILL.md) |
| Apply TypeScript rules to `.ts` or `.tsx` work | [`/typescript-best-practices`](../typescript-best-practices/SKILL.md) |
| Strip comments before review, using a reviewer that didn't write them | [`/no-comments`](../no-comments/SKILL.md) |
| Clean AI tells out of prose | [`/unslop`](../unslop/SKILL.md) |
| Write docs, an RFC, a README, a PR description, or a commit message to a standard | [`/technical-writing`](../technical-writing/SKILL.md) |
| Hear the last reply again in plain words | [`/bro`](../bro/SKILL.md) |
| Give agents a scripted way to drive the app and prove behavior | [`/create-verification-skill`](../create-verification-skill/SKILL.md) |
| Bring a verification skill and its feature map back in line with the app | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.md) |
| Formally verify a race, invariant, or other property that tests rarely reach | [`/create-formal-verification-skill`](../create-formal-verification-skill/SKILL.md) |
| Bring a formal verification back in line after the code it covers changed, and learn whether a failure is drift or a bug | [`/maintain-formal-verification-skill`](../maintain-formal-verification-skill/SKILL.md) |
| Show a PM or designer what the user sees as a chart checked by a TLA+ model, with the shortest path to each state that breaks a rule | [`/statechart`](../statechart/SKILL.md) |
| Vet a performance number before reporting or acting on it | [`/benchmark-checklist`](../benchmark-checklist/SKILL.md) |
| Run a large or cross-cutting change, or one to review after stepping away | [`/figure-it-out`](../figure-it-out/SKILL.md) |
| Keep a decision log during a run, and review it afterward | [`/show-me-your-work`](../show-me-your-work/SKILL.md) |
| Pick a model for each role and a reasoning budget | [`/setup-gt36`](../setup-gt36/SKILL.md) |
| Turn their own working habits into a personal mode skill | [`/automate-me`](../automate-me/SKILL.md) |
| Turn what a finished task taught into skill edits | [`/reflect`](../reflect/SKILL.md) |
| Stop agents from repeating the same mistakes in this repo | [`/correct`](../correct/SKILL.md) |
| Build a page whose buttons wake a Grok Bot over a webhook | [`/make-bot-ui`](../make-bot-ui/SKILL.md) |
| Find their way around gt36 | `/gt36-help` |

If a skill directory next to this one is missing from the table, read its frontmatter and route by its description. The `principle-*` directories are covered under principles below.

Close calls:

- `/explainer` explains what the code does. `/investigator` runs the Investigator playbook: explainer (or verified supplied findings), historical investigation when relevant, a cited explanation or recommendation, then unslop. Its historical stage traces local Git changes and asks once for Jira ticket context or an explicit skip. It never fetches external sources. New or uncommitted code skips only that question, not code reading. `/teach` combines the findings and preserves their uncertainty and context-request status.
- `/arena` gives every worker the same brief and merges the best parts. `/swarm` splits work into slices or a race and returns one report.
- `/architect` only designs, saves `docs/designs/<task-slug>.md`, and always stops for human approval. `/applier` writes and verifies code after that version is explicitly approved, or directly from a simple clear request. Autonomy does not bypass this gate; a redesign needs fresh approval. Applier cites real project precedents and reuses suitable API contracts. A missing-contract proposal needs your acceptance before dependent code. It runs existing checks but changes tests, snapshots, fixtures, or test configuration only when you explicitly request those edits or approve a plan naming them. A direct applier call does not automatically commit or open a PR.
- `/interrogate` reviews the diff. `/blast-radius` looks for breakage outside the diff and proves the one fact that makes the change safe.
- `/recall` rebuilds context across recent chats. `/ketchup` covers only this chat since the user's last message. Resuming one specific chat or branch is the Session pickup playbook.
- Use `/tech-leader <feature>` directly, or `/gt36-mode tech-leader <feature>`. The entry skill reads gt36-mode and runs the same Tech-leader playbook; it does not duplicate the workflow. It runs recall, sharpen, architect with its arena, figure-it-out in plan-only mode, then teach using supplied findings. It produces one design-and-plan document and stops without coding, test edits, commits, or PRs.
- `/figure-it-out` designs one rigorous run. An explicit plan-only request prepares the plan without executing it. The Orchestrate playbook runs a program that spans days and many PRs. The Autonomous run playbook drives one task to a finish condition.

Not in gt36:

- `/deslop`, `control-cli`, and `control-ui` require separately available tools.
- Scheduling requires an available, explicitly authorized mechanism. Skill authoring uses the available `skill-creator` tool.
- gt36 has no `/orchestrate` skill. Orchestrate is a `/gt36-mode` playbook. If the slash menu shows `/orchestrate`, another plugin provides it.

## Playbooks and principles

Playbooks are step lists inside `/gt36-mode`, not skills, so they have no slash command. Inside `/gt36-mode`, describing the task picks one, and these phrases name one directly:

- "babysit this pr" or "check on pr 123" runs Babysit. It drives the PR to merge-ready and stops there. It doesn't merge unless the user asks to merge, land, or ship.
- "land the stack" runs Shipping.
- "take over this branch" runs Session pickup.
- "pause safely" runs Pause safely.
- "full autopilot on this queue" runs Autopilot-full. "autopilot, don't merge" runs Autopilot-stack.
- "run the eval playbook" runs Eval.

Without `/gt36-mode`, a phrase such as "babysit this pr" can start another skill for the same job instead. The Playbooks section of [`gt36-mode`](../gt36-mode/SKILL.md) lists every playbook and when it applies. [Guide page 6](../../docs/guide/06-verify-and-ship.md) covers opening, babysitting, and landing a PR.

Planning is routed through playbooks. For one feature, `/tech-leader <feature>` (or `/gt36-mode tech-leader <feature>`) produces the architecture and implementation plan in `docs/designs/<feature-slug>.md`, explains the proposal, and stops for review. Planning mode, when available, can support this step. For program-level work that spans phases or stacked PRs, asking `/gt36-mode` for a plan runs the [Multi-phase plan playbook](../gt36-mode/playbooks/multi-phase-plan.md), which writes the plan and doesn't implement it. For a design question, the Prototype playbook can test ideas in temporary code; `/architect` synthesizes a Markdown design document and stops for human approval. `/applier` builds the approved design.

Principles are one-rule skills that `/gt36-mode` reads and cites in its replies. The user rarely invokes one. They steer with the names instead, as in "apply prove it works. show me the real output." Typing `/principle-<name>` still loads one on demand. [Guide page 8](../../docs/guide/08-principles.md) lists them.

## Fix a run that went wrong

| Symptom | Fix |
|---|---|
| The mode stopped applying after a few turns | It was started with Enter. Start it as a Custom Mode, or start each task with `/gt36-mode`. |
| A question got treated as the next step of the last task | Say "new task", or say the turn doesn't need the mode. |
| A new model choice had no effect | The rule from `/setup-gt36` applies to new chats. Start one. |
| Runs cost more than expected | See the cost paragraph under Get set up. |
| A skill didn't load on its own | Only `/setup-gt36` loads from the user's words. The others load when the user types them or when `/gt36-mode` runs them, and it doesn't run every skill. |
| Parallel agents overwrote each other | Give each agent its own worktree, or run them as cloud agents, which each get their own machine. |
| An overnight run moved but finished nothing | `/loop` needs a check that can pass or fail, not a duration. See [guide page 7](../../docs/guide/07-overnight.md). |
| The reply claims success from a green build | Ask for the real command, flow, stored value, or profile. That's the prove-it-works principle. |

For a run that drifts, [`references/prompting.md`](references/prompting.md) has one-line steers. [Guide page 10](../../docs/guide/10-recipes-and-pitfalls.md) has more pitfalls and the recipes worth copying.

## Make gt36 my own

- [`/automate-me`](../automate-me/SKILL.md) drafts a personal mode skill from the user's own history, to use alongside `/gt36-mode`.
- [`/reflect`](../reflect/SKILL.md) after a session turns its lessons into skill edits the user approves.
- `/gt36-mode write a skill for <workflow>` runs the authoring playbook. The eval playbook tests a skill change blind.
- Fix a misbehaving skill in its own PR, not inside the feature work where it went wrong.

[Guide page 9](../../docs/guide/09-make-it-yours.md) covers each of these.

## Reply

Lead with the answer. Give at most one example prompt in a code block, adapted from [`references/recipes.md`](references/recipes.md) when one fits, then the link to that file. Keep it short unless the user asked for the whole map.
