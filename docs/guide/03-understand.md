# Understand the code before changing it

Editing code you don't understand is how subtle regressions ship, and that's as true for the agent as for you. Agents usually fail in one of two ways. They misread what you want, or they don't have the context to do the work right. [What goes in a prompt](./02-gt36-mode.md#what-goes-in-a-prompt) handles the first. This page handles the second.

gt36 gives you four ways in. `/explainer` explains what the code does now. `/investigator` runs a read-only investigation for an explanation or recommendation, adding historical rationale when relevant. `/teach` blends both into one explanation. `/recall` rebuilds your own recent context on a topic. Each one also makes the agent explain itself in words you can check. That's how you supervise an agent that may know the code better than you do.

![A detective studies a machine blueprint with a magnifying glass while robots fetch case files; the evidence board behind her links clues under /explainer and /investigator.](./images/understanding.jpg)

## Start with a read-only investigation

When the cause is unclear, ask for findings, not a fix:

```text
/gt36-mode investigate why background jobs time out every few hours. give me what we know, what data you used, and your best hypotheses. don't change any code yet.
```

"don't change any code yet" routes this to the [Investigator playbook](../../skills/gt36-mode/playbooks/investigator.md). It is also available directly as `/investigator <question>`. It runs `/explainer`, adds the internal historical rationale workflow for motivation or regression questions, and returns a cited explanation without recursively invoking `/investigator`. Supplied explainer findings are reused and checked against current code. For a choice between options, it returns a recommendation with a trade-offs table. Asking "what data you used" makes the agent separate its evidence from its guesses. When the findings point at a fix, start the fix as a new task.

## Trace behavior with `/explainer`

```text
/explainer how do we dedupe notifications? is there an n+1 when we look up subscribers?
```

Ask the question you actually have. [`/explainer`](../../skills/explainer/SKILL.md) reads the code and answers at the level of a senior engineer onboarding you onto the subsystem, with the runtime flow, the key types, and the non-obvious parts. For a big subsystem it fans out two to four read-only explorers first. For a narrow question it just reads and explains.

## Dig up history with `/investigator`

```text
/investigator why was the retry limit set to five? does the reason still hold?
```

[`/investigator`](../../skills/investigator/SKILL.md) first reads the current implementation, comments, types, and relevant callers to explain what the code does. For a question about motivation or regression, it then traces the behavior through local Git, reading the actual before/after code rather than trusting commit subjects. It does not query MCPs, Jira, PRs, chat, or other remote sources.

If a relevant revision has a subject such as `[MOL-12345] change something`, investigator lists the ticket, commit hash, subject, and behavior changed, then asks you to find the ticket on Jira and paste its description, motivation, acceptance criteria, or relevant comments. Multiple tickets are grouped into one question. Choose **Provide ticket context** to provide context, or **Skip and continue from code and Git** to skip. You can supply only some tickets; the rest are recorded as not provided without repeated questions. Choosing to provide context leaves the request pending until you send it. A link alone is not fetched or treated as ticket content.

Uncommitted code, code added for the first time without revisions, and history without ticket keys skip only the ticket question. The code is still read and explained. A revision at HEAD still qualifies if earlier code exists. Missing or shallow history is a limitation, not evidence that code is new; investigator does not fetch it.

The report compares present behavior, historical patches, and your supplied context. It distinguishes evidence from inference and attributes ticket excerpts to you, without claiming to have read Jira. If a comment, commit title, or ticket description disagrees with the code, it shows both sides and the gap instead of silently choosing one or fixing the code.

Use `/explainer` for a focused runtime walkthrough, or `/investigator` for the full read-only question flow, including history when needed and a recommendation when comparing options. When another skill supplies an explanation, investigator reuses and checks it rather than starting that work again.

## Actually understand it with `/teach`

```text
/teach me how this PR changes retries. convince me it fixes the cause and not the symptom.
```

[`/teach`](../../skills/teach/SKILL.md) is for when a summary isn't enough. It runs `/explainer` and `/investigator`, for a small change maybe just one of them, and weaves the findings into a plain explanation that builds up diagram by diagram. The "convince me" framing is worth reusing. It turns the explanation into an argument you can test instead of a tour.

It works on the agent's own choices too:

```text
/teach me why you implemented it this way and not with a queue. what did you trade off, and why?
```

Teaching helps the agent as much as you. An agent that has to explain its work must read the code and back each claim with evidence, instead of stating it confidently and moving on.

## Rebuild your own context with `/recall`

```text
/recall catch me up on the export work from last week
```

[`/recall`](../../skills/recall/SKILL.md) mines your own recent chats and uses investigator to compare current code, local change history, and ticket context you supply. It hands back a brief on where things stand and what's next, distinguishing locally checked state from reports in earlier conversations. Your old chats hold context that a fresh agent lacks, so start new work on an old topic by loading it first, then hand over the new input:

```text
/recall my work on the virtualized list from yesterday, then read this bug report.
```

If you want to resume one specific chat, that's the Session pickup playbook below, not `/recall`.

## Take over prior work with Session pickup

When another agent (or you, last week) left a branch mid-flight:

```text
/gt36-mode take over this branch. read the decision log, figure out what's done, and continue from there. don't redo finished work.
```

The [Session pickup playbook](../../skills/gt36-mode/playbooks/session-pickup.md) treats the prior trail as authoritative. It reconstructs the branch state and decisions, names the resume point, and verifies inherited claims against the original goal instead of re-deriving everything from scratch.

**Pitfall:** don't skip this page's skills because "the agent will read the code anyway." An agent that starts editing without a traced model tends to fix the symptom at the first plausible spot. `/explainer` first is cheaper than the second bug.

Next: [Design the change](./04-design.md).
