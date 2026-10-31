# Build the change and clean the diff

The build playbooks share one discipline. Say what you observed, and let the playbook demand the evidence. This page shows what to put in the prompt for each common build task, then the cleanup habit that keeps diffs reviewable.

## Prompt each build playbook with what you know

A bug prompt states the symptom and asks for a reproduction first:

```text
/gt36-mode this command emits two records after a retry. repro first, then fix and verify.
```

A feature prompt states the behavior and what must not change:

```text
/gt36-mode add a --json flag. text output stays byte-identical. verify both forms.
```

A refactoring prompt pins behavior before structure moves:

```text
/gt36-mode move parsing into one module, zero behavior change. record the current output first and prove it's unchanged after.
```

A perf prompt states the measurement, not an impression:

```text
/gt36-mode startup takes 1.8s on this fixture. trace it, fix the measured cause, show me before and after.
```

Each of these routes to its playbook ([Bug fix](../../skills/gt36-mode/playbooks/bug-fix.md), [Feature](../../skills/gt36-mode/playbooks/feature.md), [Refactoring](../../skills/gt36-mode/playbooks/refactoring.md), [Perf issue](../../skills/gt36-mode/playbooks/perf-issue.md)), and the playbook supplies the steps you didn't type: reproduce before fixing, name the data shape before implementing, pin behavior before restructuring, profile before optimizing.

For sustained improvement of one number, there's the [Hillclimb playbook](../../skills/gt36-mode/playbooks/hillclimb.md). Give it the metric, a target, and a floor on attempts, and it loops one hypothesis at a time with a frozen measurement harness. It keeps wins and reverts everything else.

Both perf playbooks run [`/benchmark-checklist`](../../skills/benchmark-checklist/SKILL.md) on their numbers. Perf issue vets its baseline and every number after it, and Hillclimb vets its harness before freezing it. [Verify and ship](./06-verify-and-ship.md#vet-a-measured-number-with-benchmark-checklist) shows when to type it yourself.

Sometimes you want the cause before any fix. For a live symptom, such as a leak, an idle CPU spin, or a visual glitch, the [Runtime forensics playbook](../../skills/gt36-mode/playbooks/runtime-forensics.md) instruments the running process. For a profile you already captured, the [Trace forensics playbook](../../skills/gt36-mode/playbooks/trace-forensics.md) reads the artifact and maps the hot frame to source. Both return a diagnosis, not a fix:

```text
/gt36-mode here's a cpuprofile from the slow startup. tell me where the time goes and which source lines own it. no fix yet.
```

## Implement after design review

The build playbooks use [`/applier`](../../skills/applier/SKILL.md) for their code-writing handoff. If `/architect` is needed, they first present `docs/designs/<task-slug>.md` and wait for your explicit approval of that version. A worker or an overnight instruction cannot approve it for you.

After reading the document, you can say:

```text
/applier docs/designs/import-pipeline.md. i approve this version. implement and verify it.
```

Applier also accepts a simple clear request directly. It does not force a design file for a local mechanical change, and it adds no commit or PR step when called on its own. If implementation exposes a wrong architecture, it stops affected code, returns to architect, and waits for approval of the revised design.

## Follow project precedents and API contracts

Before editing, applier shows the real analogous features/functions it read and the choices it will follow: folder layout, util/helper placement, imports, function style, API integration, and error handling. The note cites actual files and symbols. If there is no suitable example or the examples conflict, it says so rather than inventing a convention or copying an obsolete pattern.

For API work, applier checks existing contract libraries and their public exports, compares the request/response and business meaning, and reuses the appropriate contract through the project's supported import path. `mol-lib-api-contract` is one example, not a required library for every project. If no contract fits, it reports what was checked and proposes a specific resolution, then waits for your acceptance before dependent code. Accepting a missing-contract proposal does not also approve a changed architecture.

Imports and optimization must serve the requested feature. Applier explains any new ref, memoization, cache, or optimization dependency with a concrete functional need or measured performance evidence. A ref for focus or DOM interaction is valid; speculative optimization is not.

## Run existing checks; change tests only when requested

Applier does not add or edit tests, snapshots, fixtures, test configuration, or committed baselines by default. It still runs existing tests, lint, typecheck, build, and real-surface checks without artifact-update modes. "Implement and verify" does not request test edits.

If you want test changes, request them explicitly or explicitly approve a plan naming those changes. If a test fails or project guidance requires a test update, applier explains the evidence and asks for those specific edits rather than rewriting the test, weakening an assertion, or skipping a check. This also applies before the coding handoff, including Refactoring's baseline step, and to every delegated worker.

## Write the failing test first with `/tdd`

When a bug has a cheap local test path, ask for a failing regression test:

```text
/tdd fix this bug with a failing regression test first.
```

A direct request like this explicitly authorizes the focused test changes. A cheap test path alone does not authorize test edits inside an applier workflow. [`/tdd`](../../skills/tdd/SKILL.md) writes the smallest test that fails for the intended reason, then the fix, then reruns the test. If a test would need broad harness setup or brittle mocks, the skill says so and uses the closest executable check instead. Don't force a test where a real command is stronger evidence.

## Load the TypeScript rules by name

[`typescript-best-practices`](../../skills/typescript-best-practices/SKILL.md) turns the type-system principles into concrete rules: discriminated unions, `unknown` at boundaries, exhaustive variants, schema-derived types. It doesn't load on its own, so type `/typescript-best-practices` when a task touches `.ts` or `.tsx` files.

## Clean before you commit

The [Opening a PR playbook](../../skills/gt36-mode/playbooks/opening-a-pr.md) runs `/deslop` on the diff before each commit and applies [`/unslop`](../../skills/unslop/SKILL.md) to the PR description and commit bodies. `/deslop` requires a separately available tool, not in gt36. If you don't have it, ask for the same outcome in plain words: remove narrating comments, unsupported guards, dead compatibility paths, and unrelated edits.

For prose, `/unslop` takes a target and any extra rules you have:

```text
/unslop the readme changes, no emdashes
```

You'll develop your own shorthand. The skill reads intent fine from terse prompts like `unslop that, tighten it`.

## Strip the comments with `/no-comments`

Comments need their own pass, and not from the agent that wrote them. An author defends its comments the way you'd defend yours. So before review, hand them to fresh eyes:

```text
/no-comments the diff
```

[`/no-comments`](../../skills/no-comments/SKILL.md) spawns [Comment Sicko](../../agents/comment-sicko.md), a read-only reviewer with a short keep list: license headers, doc comments on a public API, links that explain what code can't, behavior forced by an external dependency you can't reshape. Everything else goes. A surprise in your own code gets no such pass. The comment comes back as a refactor flag, and `/no-comments` fixes the flags it accepts at the root cause. If a refactor flag needs a new architecture, no-comments runs architect and waits for your approval before applier applies the fix. When a comment claims a constraint, "do not remove", the skill offers to encode the claim as a type, test, or lint. Either way, the comment comes out.

The division of labor is worth keeping straight. `/deslop` cleans slop out of the code, `/unslop` cleans it out of prose, and `/no-comments` hands the comments to a reviewer who didn't write them.

**Pitfall:** cleanup is not optional polish. A diff with narrating comments and defensive dead weight reads as unfinished to reviewers, and the extra code is where the next bug hides. If the diff feels padded, say `deslop it` before you commit, not after review calls it out.

Next: [Verify and ship](./06-verify-and-ship.md).
