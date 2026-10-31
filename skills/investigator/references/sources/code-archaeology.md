# Local code and Git archaeology

Read current source before history. The allowed evidence is current code/comments, local Git objects, and context the human supplies. Do not run gh, use a browser or MCP, or fetch remote history.

## Establish what exists locally

Run read-only checks in the target repository:

```bash
git rev-parse --is-inside-work-tree
git rev-parse --verify HEAD
git rev-parse --is-shallow-repository
git status --short -- <file>
git diff HEAD -- <file>
```

A repository without HEAD may contain new/untracked code. A non-Git folder still has source to read. Report unavailable checks accurately. A shallow clone or missing parent object limits lineage; do not fetch to repair it or claim the target is new just because the visible boundary looks like a root.

Separate current working-tree changes from committed history. Read changed hunks and related committed code independently. Skip the context question for uncommitted code, not for every historical part of a dirty file.

## Read the behavior and its revisions

Use commands as needed, filling placeholders from the actual target. Do not interpolate user prose or ticket content into shell commands.

```bash
git blame -L <start>,<end> -- <file>
git log --follow --format=fuller -p -- <file>
git log --oneline -20 -- <file>
git log -S '<exact_code_text>' -p -- <file>
git log -G '<code_pattern>' -p -- <file>
git show -s --format='%H%n%s%n%b' <commit>
git show --find-renames <commit> -- <path_at_commit>
git show <commit>:<path_at_commit>
git show <parent>:<path_at_parent>
```

Blame gives a last-touch clue, not necessarily the introducing commit. Inspect patches and the parent/current implementations around relevant symbols. Ignore unrelated formatting and file-wide touches. Follow a rename to the old path when reading parent blobs. When examining a merge, name the parent used; inspect the relevant parents before crediting the merge with the rationale.

A file-level creation commit does not prove that a particular function is new: read symbol/hunk history. Conversely, a new function in an old file may have only an addition and no revisions. Verify the relevant scope instead of using file age as a shortcut. A revision at HEAD with an earlier implementation is eligible history.

## Ticket candidates

Read the subject and body of commits that actually explain changes in the target. Recognize project keys such as `[MOL-12345]` using uppercase project-key plus digits, and inspect candidates for relevance. Multiple messages with the same key produce one ticket entry with all supporting hashes. A key in an unrelated commit is not a reason to question the human.

Keep this evidence for each candidate:

- Ticket key as written in the commit message.
- Supporting commit hashes and subjects.
- Relevant file/symbol and concrete before/after behavior.
- Whether the code has revisions rather than only an initial addition.
- Whether user-supplied context already covers that ticket, and any history limitations.

No key means no ticket question. An initial addition without revisions or an uncommitted change skips that question. Missing history means unknown lineage, not evidence of no revisions. Ask for qualified candidates visible in the available revised lineage, and record remaining gaps. Do not manufacture a Jira URL, retrieve PR bodies, or treat a ticket key as proof of its description.
