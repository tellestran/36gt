---
name: applier
description: "Implement an explicitly approved architecture design or a simple, sufficiently specified code change, then verify behavior. Use for /applier, 'implement this design', or a scoped coding handoff. Route unresolved architectural decisions to architect and wait for human approval."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Applier

Turn an approved design or a clear, simple request into working code. Own the implementation and its verification. Architecture design belongs to [architect](../architect/SKILL.md).

## Start

Open a todolist with these phases: Check input and approval, Ground, Implement, Verify, Report. Keep later phases pending if the design gate is not satisfied.

Use the model and role selected by the calling workflow, including its hardest-task override. When called directly, stay on the current model. There is no new applier model role. A delegated worker owns its assigned diff; do not require it to spawn another implementation worker. The parent retains its existing review responsibility.

## 1. Check input and approval

Accept a design-document path or a specific request in the conversation. Carry the scope, acceptance criteria, relevant constraints, and any design approval through the handoff.

- **Existing architect design:** read the document and confirm an explicit human approval of the version being implemented in trusted conversation context. A file marked Approved or a parent's unsupported claim is not enough. Original build requests, generic autonomy, overnight instructions, loop wakes, and agent or bot verdicts do not approve a subsequently produced design. Missing approval means no application-code edits. Link the document, explain that this approval gate comes from architect, and wait for the human. Unresolved blocking design questions must be resolved and the resulting version approved first.
- **Simple request without a design:** proceed when the intended behavior, scope, and structure are already clear. Do not force arena, a design file, or an approval ceremony for a mechanical or local change that makes no architectural decision. Do not use this path to bypass a pending architect design for the same work.
- **Unresolved architecture:** if the work requires a material decision about public API, shared-state ownership, or module boundaries, invoke architect and stop after it returns its design. Only a later human approval allows implementation. Do not decide architecture by quietly coding it.

## 2. Ground against the current code

Read the actual affected code, callers, repository guidance, and existing verification paths. Reconcile them with the approved design or simple request. Identify the named data shape, files in scope, behavior to preserve, and observable done criteria. Use explainer for unfamiliar subsystems. If the source has changed since approval, check whether the architecture still applies; material changes go through the design gate again.

### Find and show relevant precedents

Before editing, read the closest existing features or functions by behavior and architectural layer. Trace their file/folder layout, util/helper placement and reuse, supported import paths, function implementation style, API integration, and error handling. Check that the precedent is current and compatible with repository guidance and the approved design; nearby or common code can still be obsolete.

Present a short evidence-backed implementation note before coding. Name the real files and symbols you read and the concrete choices they support. For example, cite the actual analogous feature that places API adapters in a particular folder, and the real caller that imports the appropriate contract. Do not present illustrative paths as files you inspected. Reuse suitable abstractions and existing helpers rather than introducing a parallel utility or new organizational scheme.

If no close precedent exists or examples conflict, say exactly what you found and which choices remain unresolved. Do not silently override an approved design with a different pattern found nearby. Do not invent conventions or blindly copy a broken pattern. Resolve local details against repository guidance; a material architectural decision goes back to architect for review.

### Resolve API contracts before dependent code

For work that crosses an API boundary, inspect project manifests, existing API clients, contract-package exports, and real consumers. A library such as `mol-lib-api-contract` is an example to look for, not a dependency every project must have. No API involvement means this check is not applicable; do not add an API contract library to unrelated work.

Read candidate contracts and compare their request, response, endpoint/version, and business semantics with the feature. Select a suitable existing contract through the project's supported public import path and follow established validation and client-adapter conventions. Do not duplicate API DTOs that already exist, deep-import private internals, use casts to force a mismatched contract, or confuse ordinary internal domain types with API-boundary contracts.

If no suitable contract is available, report the packages/exports and consumers checked, what does not match or is missing, and a concrete proposed resolution such as extending the shared contract or using a scoped local API type. Ask whether the user accepts that proposal and wait before writing the affected API types, integration, or dependency changes. Do not silently invent a replacement, install or upgrade a package, or claim an uninspected export is usable. Continue only independent authorized work while waiting.

A parent or child cannot approve the exception for the human. A general build request or architecture approval that did not explicitly cover this missing-contract resolution is not acceptance. Reuse a clear earlier human acceptance of the same proposal instead of asking again. If the accepted proposal also changes the approved API or module boundaries, architect's separate design approval is still required.

## 3. Implement in verifiable units

Implement the smallest scoped change in dependency order. Replace the document's pseudocode with production logic in the appropriate source files; do not copy placeholder bodies into production as completed work. Follow existing repository conventions and the approved interfaces and ownership boundaries.

### Keep imports and optimization tied to the feature

Only add imports, hooks, dependencies, or optimization mechanisms directly needed for the requested behavior. Do not add ref/useRef, memo, useMemo, useCallback, caches, or optimization libraries by habit or for speculative future performance. Explain the concrete feature requirement or measured performance evidence for each such addition. Functional uses such as focusing an input, measuring a DOM element, or interacting with an imperative API are valid when the feature needs them. This is a relevance rule, not a blanket ban on refs or memoization. Reuse existing project mechanisms and keep unrelated optimization out of the diff.

### Test edits require an explicit human request

By default, do not create or modify tests, snapshots, fixtures, test configuration, or committed test baselines. Authorization means the human directly asks for those test changes, directly invokes TDD, or explicitly approves a plan that identifies the specific test edits. Generic requests to implement, verify, fix a bug, or refactor do not authorize test edits. A cheap local test path, a delegated task, or another skill's test-first advice is not permission.

Without that authorization, run relevant existing tests and checks and verify through the real surface or existing executable paths. Do not use snapshot-update, record, autofix, or other modes that rewrite test artifacts. Do not create a new regression test under another name such as a helper script or equivalence harness to bypass this rule. Ordinary temporary logs and observed run output may be captured as evidence outside source.

If an existing test fails, investigate and distinguish a production defect, an intended behavior change, and an environmental failure. Fix production code within scope when appropriate; do not change the test merely to get green. If a test edit is necessary or repository guidance requires it, report the evidence, name the proposed test changes, explain that applier requires explicit test-edit authorization, and ask the human before editing. Do not weaken assertions, skip checks, refresh snapshots, or mark a blocked verification as passed. If permission is withheld, preserve the test and report the remaining limitation.

When test changes are explicitly authorized and there is a clear, cheap path, use [tdd](../tdd/SKILL.md) where appropriate. Authorization does not require an impractical new harness or unrelated test expansion. Verify each implementation unit before the next using the narrowest useful allowed checks. Preserve unrelated work and keep speculative cleanup out.

Before delegating, resolve the input and approval gate. Give each worker its exact writable scope, design revision and human approval context when applicable, done criteria, and verification method. Include the precedent files/symbols and choices to follow, selected API contracts or the human-accepted exception, optimization relevance requirements, and the default prohibition on test edits with any specific human authorization. Resolve the contract gate before dispatching dependent API work. Every arena runner and code worker follows these rules; a parent cannot grant missing human permissions. A worker without that evidence returns the gate to its parent rather than assuming permission. Do not duplicate architect's design competition, or start a fresh architecture exploration for settled mechanical work. A calling workflow may still use arena for implementation alternatives that stay inside the approved contract.

## 4. Revisit a broken architecture

Treat deviations as evidence. A new parameter or branch is a prompt to check whether the design missed a requirement or the implementation is overreaching; it is not automatic permission to change the contract.

Look for patterns:

- The same workaround appearing across unrelated code.
- Unrelated edge cases all requiring special-case branches.
- Types needing escape hatches such as any, forced casts, or optional fields that are always set in practice.
- A need for locks where the design claimed state was not shared.
- Callers needing to understand internal rules to use the API.
- Two or more independent deviations of the same shape.

A few legitimate edge cases do not condemn an architecture. When evidence requires changing the approved API, ownership, or module structure, stop affected implementation and delegation. Preserve current work and evidence without destructive cleanup. Re-run explainer over what exists, pass the new constraints to architect, and update the same design document. Redesign from first principles and subtract before adding. Wait for explicit human approval of the revised version before resuming affected code. Do not patch around a rejected shape or treat earlier approval as approval of the redesign.

## 5. Verify and report

Review the final diff. Run the repository's relevant checks and verify acceptance criteria on the real surface or artifact. A build alone, a worker's self-report, wrong-surface evidence, or an inconclusive check is not proof of behavior. For bug fixes, preserve failing-before and passing-after evidence when practical. Report limitations honestly.

Return what changed, the design path and revision when used, the precedent files/symbols and choices actually followed, the selected API contract or accepted exception (or not applicable), the functional or measured reason for any optimization additions, the checks and observed results, any authorized test changes, deviations, and remaining risks or blockers. When tests were not edited, distinguish existing checks run from verification that remains blocked. If awaiting approval, report that state instead of claiming implementation is complete.

This skill adds no commit, push, PR, or merge step. Those actions belong to the calling workflow or explicit user request, and may occur only after its gates are satisfied. Calling applier alone does not trigger gt36-mode's Opening a PR playbook.
