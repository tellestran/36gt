---
name: tester
description: "Review a change or feature with blast-radius analysis, adversarial interrogation, and executable evidence, then explain the findings plainly. Use for /tester or an explicit request for this combined review. Reports verified defects, cleared risks, and unverified gaps; does not apply fixes or automatically edit repository tests."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Tester

Run one review: **frame and apply principles → blast-radius → interrogate → reconcile evidence → bro**. The receiving chat owns the review and final answer. Read each linked skill before using it; reuse findings between stages rather than starting unrelated investigations.

## 1. Frame the target and review rules

Use the user's named feature, files, diff, or design and expected behavior. Otherwise use interrogate's branch-diff scope, including working-tree changes. Read the relevant implementation and callers, not only the diff. State the exact base and target being reviewed. If there is no usable base/history, report that and scope to the available code; do not invent a diff. Ask when intent or target cannot be inferred.

Read these five principles and use them throughout the review, not as five separate investigations:

| Principle | What to investigate |
|---|---|
| [Boundary discipline](../principle-boundary-discipline/SKILL.md) | Raw inputs are validated at actual system boundaries; domain logic and public APIs do not leak transport/framework details. Distinguish real untrusted inputs from redundant internal guards. |
| [Fix root causes](../principle-fix-root-causes/SKILL.md) | Trace a reproduced failure to the first wrong decision or missing handling of a legitimate state. Separate the source of the fault from downstream symptoms; look for other in-scope occurrences. |
| [Laziness protocol](../principle-laziness-protocol/SKILL.md) | Check whether decisions have one source of truth and whether a smaller change meets the same intent. |
| [Make operations idempotent](../principle-make-operations-idempotent/SKILL.md) | For state-mutating paths, examine duplicate execution, retries, interruption, restart, stale artifacts, and convergence. Use isolated disposable state for runtime checks. Mark inapplicable paths rather than inventing lifecycle scenarios. |
| [Subtract before you add](../principle-subtract-before-you-add/SKILL.md) | Consider removing obsolete branches, redundant guards, or speculative layers before recommending new mechanisms. Tie suggestions to actual usage and the requested behavior. |

These are review criteria. Their instructions to fix, extract, remove, or redesign become recommendations here, not permission to modify the product. A preference about structure is not a verified defect without a demonstrated violation of the intended behavior.

## 2. Map and check the blast radius

Run [blast-radius](../blast-radius/SKILL.md) over the framed target. Follow downstream consumers, pinned library behavior, wire/data contracts, and lifecycle ordering where relevant. Identify the one or two facts safety depends on and use the cheapest meaningful executable check against the real code.

Keep confirmed risks, cleared cases, and unproven assumptions distinct. Pass the scope, intent, dependency map, commands, observed results, and open hypotheses into the next stage. If historical rationale requires ticket context, preserve investigator's coordinator-owned provide/skip question and pending state.

For a wide change, use the upcoming interrogate fan-out as blast-radius's multi-reviewer pass rather than running a second arena for the same scope. Do not treat the initial analysis as a verdict the adversaries must agree with.

## 3. Challenge through interrogate

Run [interrogate](../interrogate/SKILL.md) on the same target and intent. Keep its reviewer model configuration, arena isolation, adversary brief, and rerun requirement; add no tester model role. Pass all five review criteria and the verification/write limits below to every reviewer. Give existing evidence as starting material, not an answer key; reviewers must challenge it and investigate additional in-scope failures.

Use interrogate's arena phases only: frame/fan-out and verification, not pick/graft/implementation. Reviewers report reproductions and do not apply fixes. If parallel reviewers or isolated execution are unavailable, perform the feasible checks directly and state that the multi-reviewer stage did not run; do not imply independent review occurred.

## 4. Reconcile and verify

Rerun candidate findings against the exact reviewed code and permitted environment. Keep a defect only when a run demonstrates behavior that violates the stated intent; record the command, expected result, actual output, and source location. A failure to launch, missing service, or unsupported assumption is a verification limitation, not proof of a product bug. For designs, distinguish a probe of existing behavior from validation of the proposed design.

Merge duplicate findings by root cause, retain all independently verified defects, and explain dropped candidates. Keep source-backed concerns that cannot be executed in a separate **unverified risks** section, with the missing check; never promote them to interrogate findings. A clean test run or no verified findings does not prove the whole feature safe. Stop after relevant evidence is sufficient or a concrete blocker prevents the remaining checks; do not expand into unrelated cleanup.

## 5. Explain through bro and hand back

Apply [bro](../bro/SKILL.md) to the combined review answer. Its input is the review explanation, not the user's request or an unrelated earlier message. Simplify jargon while retaining commands, citations, expected/actual results, confidence, and limitations. Return one coherent answer rather than separate stage reports:

- **Scope and intent:** what was reviewed, base/target, and expected behavior.
- **Verified findings:** priority by observed impact, location, root cause, command, expected/actual output, and smallest supported fix recommendation. Say **no verified findings** when appropriate.
- **Safety evidence and cleared risks:** critical safety facts, how far each was verified, and what passed.
- **Unverified risks and limits:** pending hypotheses, blocked checks, missing context, and reviewer stages not run. Include concise applicability/results for the five principles without treating them as automatic defects.
- **Recommended next step:** the cheapest remaining check or scoped fix proposal, separating recommendations from work performed.

## Verification and write limits

This skill authorizes review, not implementation, architectural redesign, commits, PR creation, or merging. Run appropriate existing tests/checks without snapshot-update, record, autofix, or baseline-rewrite modes. Do not add or edit repository tests, snapshots, fixtures, test configuration, or product code unless the user separately requests that work. Invoking `/tester` alone is not test-authoring permission.

Standalone reviews may use minimal temporary repro scripts/probes in isolated scratch locations to exercise real code, as blast-radius and interrogate require. Do not copy production logic into a mock reproduction, add a permanent harness, or disguise a repository test change as a helper. When called from applier or another workflow, inherit its stricter limits: without test-edit authorization, use permitted existing executable paths and do not create a new regression test or equivalence harness under a temporary name to bypass the caller's rule. Report any resulting proof gap.

Run mutating scenarios only against disposable local state; do not replay retries or crashes against shared/live data. Keep each reviewer's scratch output separate, preserve the user's uncommitted work, and clean up only processes and scratch artifacts created by this review after capturing evidence. Pass these constraints to all delegated work. Any later fix follows applier and the existing design, contract, and test-authorization gates.
