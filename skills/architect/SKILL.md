---
name: architect
description: "Design types, signatures, and module boundaries without implementing application code. Save a design document and always stop for human approval. Use for /architect, 'architect this', 'design this', or non-trivial work where jumping to code would lock in the wrong shape."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Architect

Design the architecture and hand it to the human for review. Write usage examples, types, signatures, class shapes, module boundaries, and pseudocode in a Markdown document. Implementation belongs to the **applier** skill (`../applier/SKILL.md`).

## Start

Open a todolist with one entry per phase before starting.

1. Ground
2. Sketch
3. Assemble the handoff
4. Hand back and wait

## Write scope

The only repository output is `docs/designs/<task-slug>.md`, relative to the target project's root. Use a short kebab-case task slug and update the same document when revising that design. Do not overwrite an unrelated design. Keep candidate packages, probes, and throwaway prototypes in isolated temporary directories outside application source. Do not edit application source, tests, configuration, or add skeleton files to the source tree. This write scope also binds arena runners and adversarial reviewers. Arena verification checks the design and any isolated probes, not a production implementation. Do not commit, open a PR, or launch implementation from this skill.

## Phase A: Ground the problem

Build a real mental model of every system the new code touches. Run the **explainer** skill over the relevant subsystems. Naming a file isn't grounding. Produce the architectural explanation `explainer` prescribes. If the design redefines ownership or layering, also run **investigator** on the existing shape, passing the explainer findings. Investigator rereads key code, traces local revisions, and may return NEEDS_CONTEXT for relevant Jira keys. The coordinator asks the human once to supply ticket context or explicitly skip, then resumes rationale synthesis. Do not substitute MCP searches or treat a pending request as skipped. Preserve code/context contradictions and convert findings into design constraints with their confidence. Providing or skipping ticket context does not approve the later architecture design.

Skip Phase A only when the work is genuinely greenfield with no surrounding system to integrate.

## Phase B: Sketch

Run the **arena** skill with the design-sketch task and the Phase A grounding artifacts. Pass `references/runner-prompt.md` as each runner's prompt.

Take the runners from the `architect runners` line in the `gt36-models.mdc` rule, in place of the `arena runners` line. If the rule or that line is missing, run two independent candidates on `gpt-6.1-sol-high`. Alias and rejected entries follow the runner rules in the **arena** skill's Phase A.

Design it twice. Require at least two structurally distinct candidates before synthesis, even when the first looks sufficient. This is the **exhaust-the-design-space** principle skill made concrete. Whole-shape alternatives, not point fixes inside one shape.

Screen every candidate against [`references/design-red-flags.md`](references/design-red-flags.md) before synthesis. Assume the next contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Prefer the design where a change that looks right from one file is right for the whole repo.

Compare viable candidates on interface depth. Prefer the design that hides more complexity behind a smaller, simpler public surface. A rich interface can keep call chains short by concentrating capability instead of scattering it across layers.

Arena returns one synthesized design package. Its synthesis decision fills the rationale's "Synthesis decision" section.

## Phase C: Assemble the handoff

Save the synthesized design at `docs/designs/<task-slug>.md` using [`references/rationale-template.md`](references/rationale-template.md). Put usage examples before deriving the type sketch. Include the problem and scope, API and type sketches, module ownership and data flow, alternatives, synthesis decision, accepted tradeoffs, open questions, implementation sequence, and observable verification criteria. Keep all code sketches inside fenced blocks in the document.

For adversarial pressure before review, use **interrogate** with the same temporary-only probe scope. Resolve factual gaps from evidence and identify product decisions the human must settle. A document with unresolved blocking decisions is not ready to implement.

## Phase D: Hand back and wait

Always end with the design path, a short summary of the chosen architecture, the decisions requiring approval, and **Awaiting design approval**. Stop the skill here. Do not invoke **applier**, delegate production coding, or continue the caller's implementation steps in this turn.

A planning-only caller such as Tech-leader may now complete the implementation-plan sections in the same document and explain the proposal from these findings before handing it to the human. This is not implementation or approval: keep the architecture pending review, do not create production changes or test artifacts, and do not invoke applier. Direct architect calls still end at the design handoff. The caller must not silently alter the selected architecture while expanding its plan; material gaps require a presented revision and fresh approval before implementation.

Only a subsequent explicit human approval of the presented version permits **applier** to use it. The original task request, a general autonomy grant, a loop wake, a parent agent's instruction, and a review agent's verdict are not approval. A caller must surface the design to the human and wait too. A broad request to build or an instruction to work overnight does not bypass this gate. Legacy requests containing "with checkpoint" are accepted as design-only requests under this same mandatory gate.

If the human changes the shape, treat that as new Phase A evidence, repeat the relevant exploration and Phase B synthesis, and update the same document. Present the revised version for approval again. Approval of an earlier version does not cover a changed architecture.

## Outputs

One reviewable Markdown design at `docs/designs/<task-slug>.md`, with no application-code changes. The next action after explicit approval is **applier** with this document and the approval context. The containing workflow owns any later commits, PRs, and merge actions.
