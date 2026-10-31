---
name: recall
description: "Reconstruct your recent working context from in-scope chat history, current code, local Git history, and human-supplied ticket context, then hand back a tight current-state brief. Use for 'recall my work on X', 'catch me up', 'what have I been working on', 'where did I leave off', before starting or resuming work."
disable-model-invocation: true
---

**Codex runtime:** Read the [Codex adapter](../../docs/harness/codex.md) once before using this skill. It maps tools, models, and paths and disables inherited playbook routing on Codex. Other runtimes use the instructions below unchanged.

# Recall

**Before you start or resume work, you rebuild the user's recent working context and hand back a tight capsule of where things stand now and what to do next.**

Keep it tight and on-topic. Read only what the in-scope threads need, then stop.

Rebuild context from your own in-scope chat history, current code, local Git changes, and context the human supplies. Chats record prior reports and decisions; source and Git show what exists locally now. The **investigator** skill reads the current mechanism, traces its revisions, and asks for relevant ticket context when needed. It does not search remote issue trackers, chat systems, documents, or error services. Do not present old chat reports as fresh production observations.

Locate transcripts through the current session’s history tools or a workspace-scoped path supplied by the runtime or user. Do not infer a private history directory. If history is unavailable, report that limitation and use supplied context and local Git evidence.

1. Classify, then route. One specific prior chat to resume is the `session-pickup` playbook, not this. Turning habits into a durable skill is `automate-me`. A human-readable summary of your work is a different task. Recall loads working context across recent chats before you act. If the user already gave you a full state capsule (paths, branch, the change), use it and skip the mining.
2. Lock the scope before searching. Pin the window ("recent" is a real range, default the last 7 days), the topic if named, and the workspace (default the active one. Never read another project's transcripts without being asked). State the scope back. Never quietly turn "all" into "recent N".
3. Fan out across your chat history. Spawn parallel subagents on a fast, cheap model, each taking a slice of the corpus. Tell every subagent to order candidates by real modification time (`ls -t`) and never by UUID name, grep the topic first and then read only the matching chats and only their relevant regions, and skip the current chat plus obvious noise (subagent, eval, and test chats). Each returns the same schema, one block per chat: topic, the user's goal, decisions, open threads, struggles and corrections, and artifacts (PRs, tickets, branches), each citing the chat UUID. For one or two chats, skip the fan-out and search directly. The raw transcripts stay in the subagents. The main thread gets only their findings. Pick each subagent's reasoning level and fast or standard speed for the job, per the Subagents section of the **gt36-mode** skill.
4. Ground a named feature, file, subsystem, area, or bug with **investigator** while mining chat history. Ask it what the current code does, which local changes and reverts explain its shape, and how supplied context compares. Pass relevant code findings and human-provided ticket context from the in-scope conversations so they are not requested again. Children return NEEDS_CONTEXT to this coordinator; aggregate one question offering context or explicit skip and resume synthesis after the answer. Do not reuse the old per-MCP source playbooks or fetch remote evidence. Mark missing history or context honestly. Skip this step only for pure activity recall with no named code target ("what did I do this week").
5. Verify against available local state. Check branches, commits, and working-tree facts with local Git. PR or ticket status mentioned in transcripts is a past report unless fresh context was supplied; do not claim investigator checked it remotely or invoke gh to fill its evidence gaps. When the answer hinges on what an agent actually did (the tools it ran, files it read, errors it hit), read the full transcript, not just a trimmed local copy.
6. Write the brief to the contract below. Group by thread. Stay on the named topic.

## Output contract

Lead with the capsule, then the thread status, then the problems, then the next move. Deeper detail goes below or gets cut.

- **Capsule.** At most 5 bullets. What this work is and where it stands overall.
- **Threads.** One line each, prefixed with exactly one status tag: `[merged #N]`, `[open PR #N]`, `[in flight <branch>]`, `[verified, uncommitted]`, `[reverted #N]`, or `[planned, not started]`. A thread with no tag is not done yet, so tag it. If a PR status comes only from an earlier transcript or supplied report, say when and where it was reported instead of implying fresh remote verification.
- **Problems.** At most 5, the recurring ones. Include the symptoms users keep reporting and any fix that shipped and was reverted, so the next attempt starts where the last one failed.
- **Next move.** The single most useful next action, concrete.

An adjacent feature or ticket stays out unless it blocks this one. When the capsule and thread lines outgrow a screen, cut detail before you cut threads. Write the brief through the **unslop** skill, cite chat findings by UUID, local findings by file/symbol or commit hash, and supplied ticket context by ticket key and the human message, and sanitize private context before any public output.

**Reply:** the brief, to the contract above.
