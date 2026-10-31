---
name: figure-it-out
description: "Design an auditable playbook when no narrower one fits: a large migration, an ambitious multi-part change, or work a human reviews after stepping away. Scales rigor to the task, runs a hypothesis loop, and logs decisions. Use for /figure-it-out or 'figure it out'."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Figure it out

When the task matches no playbook, design one. The deliverable before any code is the workflow itself: a sequence of phases that scales rigor to the task, runs the scientific method, and leaves a decision trail a human can audit after stepping away.

## Plan-only mode

When the caller explicitly requests `plan-only`, including the Tech-leader playbook, this branch takes precedence over Start and Phases A-E below. Use the supplied brief, current-code findings, architecture, candidate comparison, risks, and document path. Frame the goal, scope, observable done criteria, and rigor, then organize small implementation units with dependencies, intended scope, outcomes, verification methods, and open decisions. Reuse the caller's findings; narrow read-only checks are allowed to clarify a specific gap.

Write or update only the caller's plan document. Tech-leader uses `docs/designs/<feature-slug>.md` for both design and plan, preserving architect's architecture and review status. If no path is supplied, use `docs/designs/<task-slug>.md` and avoid overwriting unrelated work. The architecture can be proposed and awaiting approval; completing a plan does not approve or implement it. Record a material design gap for the caller to revise rather than silently changing the selected shape.

Do not call architect or arena again, build a verification harness, create or modify tests, run implementation experiments, start implementation workers, invoke applier, execute planned units, commit an audit trail, open a PR, or schedule execution. Propose concrete verification and any test changes as future work subject to the caller's approval rules. Distinguish existing evidence from checks not yet run. Return the plan path, ordered units, dependencies, tradeoffs, and unresolved decisions, then stop. Default execution below is not resumed at the end of plan-only mode. Without an explicit plan-only request, retain the normal workflow below.

## Start

Open a todolist whose first item is to read the Principles section of the **gt36-mode** skill. Then add the phases below as todos.

## Phase A: Frame

Ground first, then commit. Don't start the run until you can state:

- The definition of done as a falsifiable predicate (the **prove-it-works** principle skill).
- Scope, quantified: rough units and effort, plus the blockers grounding surfaced.
- The rigor level, biased high. One-way doors and high blast radius get more. Reversible low-stakes steps get less. Rigor is gates and artifacts, not "try harder".

Present the framing and tradeoffs before committing to a long run. This run-level checkpoint is separate from the mandatory approval of each architect design. Reversible work proceeds (the **never-block-on-the-human** principle skill), but a multi-hour run earns one checkpoint.

## Phase B: Design the workflow

Decompose into atomic, independently-landable units. Sequence riskiest-unknown-first. Scaffold and verification come before features (the **foundational-thinking** principle skill).

- Build the verification harness before the work, with the baseline captured from the pre-change state, so the check reads as "old value vs new value".
- For one-way-door design decisions, run the **architect** skill (it runs **arena**), present its document, and stop for explicit human approval before executing design-dependent steps. A multi-hour autonomy grant does not approve the design. Skip it for mechanical work whose shape is already concrete. A second arena over a settled design is over-engineering (the **laziness-protocol** principle skill).
- Decide what fans out. Parallelize only across seams, and give each worker its own worktree or branch (the **separate-before-serializing-shared-state** principle skill). Don't over-fan. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **gt36-mode** skill.
- Write the designed phase list down. That list is what the human reviews.

Then execute the workflow through **applier** for code changes, carrying any approved architecture document and human approval context. If architect has just returned, wait for the human before this execution. Add its steps to the todolist as concrete items between the Phase C and Phase D entries. Run each under the Phase C loop discipline, and weave the Phase D log through them, a row as each step lands, rather than saving the whole trail for the end.

## Phase C: Run the loop

Each unit is an experiment. State the hypothesis, make the smallest change, measure against the predicate on the real artifact, keep it if it advanced, revert it if it didn't. Apply the **sequence-verifiable-units** principle skill, verifying each unit before starting the next instead of batching checks at the end.

- Verify by inspecting the artifact, never a self-report. When something passes too easily, suspect the observation method before the system.
- Pair delegated work with a judge. If a worker games the gate, reset and harden the contract. If the gate itself is wrong, fix the gate in its own change rather than routing around it.
- A verdict is VERIFIED, NOT VERIFIED, or INCONCLUSIVE. Inconclusive is not a pass. Don't hide a negative.

## Phase D: Keep the audit trail

Log the run via the **show-me-your-work** skill. figure-it-out's work is usually ambitious enough to commit the trail so the reviewer can read it in the PR. The trail plus the diff is what lets the human come back and trust the work.

## Phase E: Verify and hand back

Check the whole against the Phase A predicate on the real product, not just the harness. Encode any recurring correction as a gate, a lint rule, a check, or a script (the **encode-lessons-in-structure** principle skill).

**Reply:** the playbook you designed, the rigor level and why, the decision-trail path, what's verified against the predicate, and what's still open.
