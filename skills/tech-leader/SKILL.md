---
name: tech-leader
description: "Compare approaches and produce an architecture plus implementation plan for one feature, then stop for human review. Use for /tech-leader or an explicit request to plan a feature with Tech-leader. This is the direct entry to the existing gt36-mode Tech-leader playbook, not an implementation workflow."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Tech-leader

**Document language (all runtimes):** Write every document created or revised by this skill in English, regardless of the language of the user's request. This includes briefs, architecture proposals, implementation plans, headings, tables, and diagram labels. Pass this requirement to every delegated or nested skill. Preserve exact code identifiers, file paths, and verbatim source quotations. Conversation with the user may remain in the user's language.

Run the existing planning playbook in this chat. This skill is an entry point, not a second workflow.

1. Read [gt36-mode](../gt36-mode/SKILL.md) in full and apply its guidance within this planning task. Do not re-match the request to Feature, the large-work execution fallback, or this wrapper. Do not enable a persistent mode merely because this skill was invoked.
2. Read the [Tech-leader playbook](../gt36-mode/playbooks/tech-leader.md) in full. It is the single source of truth for the sequence, document contract, write scope, and review gates. Pass through the user's feature description, constraints, supplied context, and prior findings unchanged in intent. If no concrete feature can be inferred, ask for its description before starting; do not plan the literal placeholder `<feature>`.
3. Copy the playbook's steps into the todolist and execute that playbook here. The receiving chat owns coordination; do not spawn a gt36-agent just to run the whole playbook. Delegated design work follows the playbook and gt36-mode guidance.
4. Hand back the playbook's document and explanation, then stop for human review. Do not call applier or a delivery playbook, edit application code/tests/configuration, commit, open a PR, merge, or schedule execution.

`/tech-leader <feature>` and `/gt36-mode tech-leader <feature>` use the same playbook and produce the same single `docs/designs/<feature-slug>.md` artifact. Keep workflow changes in the playbook rather than duplicating them here.
