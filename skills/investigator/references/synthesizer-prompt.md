# Investigator synthesis

Fill in the placeholders. Work directly for a small question or give this template to the delegated synthesizer.

## Inputs

- **Question:** {QUESTION}
- **Current code and local history findings:** {LOCAL_FINDINGS}
- **Human-supplied ticket context:** {SUPPLIED_CONTEXT_OR_NONE}
- **Context request status:** {PROVIDED_PARTIAL_SKIPPED_NOT_NEEDED_OR_PENDING}
- **History limitations:** {LIMITATIONS_OR_NONE}

Read [epistemics.md](epistemics.md). Reconcile the three evidence layers without extending the investigation to external sources. You may read current files and local Git to check citations. Do not edit files, run gh, browse, fetch, or use MCP. Do not ask the human; return any pending context request to the coordinator.

If a context request is still pending, return the source-grounded partial findings and the pending ticket list. Do not silently mark context skipped or present an unsupported final historical rationale. If the user provided context for only some requested tickets, the request is complete; record the rest as not provided without asking again.

## Output

### The question

State the target question and scope briefly.

### What the current code does

Required even for new code or skipped context. Describe inputs, outputs, branches, state changes, and interactions that matter, with file:line/symbol references. Label what is traced from source rather than observed in a run. State when a comment disagrees with the implementation.

### Change history

Give the relevant commit hash, subject, and actual before/after behavior for each meaningful change, following renames. Classify new/uncommitted parts separately from revised code and flag unavailable or incomplete history. A recent HEAD change is still history. Do not substitute the last file-touching commit for the behavior's lineage.

### Ticket context

For each deduplicated related key, name its contributing commits and one status: user-provided, not provided after a partial submission, skipped by user, or pending. Attribute excerpts or paraphrases as **User-provided context for <ticket>**, never as Jira content you fetched. No context candidates means state the exact automatic reason for not asking. Do not invent ticket URLs or infer purpose from the key alone.

### What we found

Give Direct or Supported claims and their evidence. Separate observations of mechanics from evidence of intent. Attribute commit and comment claims to their authors/text; do not assert their rationale still holds without checking current code. Human explanations are attributed statements, not independent corroboration.

### What we can reasonably infer

Optional. Label each inference and show the evidence chain with cautious language. Do not promote guesses to facts when context is skipped.

### Contradictions and competing hypotheses

Show both sides of code/comment, title/diff, or ticket/implementation disagreements with their locations. State what would settle the difference without choosing a side by default or applying a fix. Include competing explanations when the evidence cannot distinguish them. Omit only when there are none.

### What we do not know

Required. State unresolved intent, unavailable history, missing or skipped context, searches that returned nothing, and any difference between static tracing and runtime evidence. Never claim an external system was searched.

### Sources consulted

List only sources actually read: current file ranges and symbols, local Git commands and commits/parent paths, and user messages or supplied ticket excerpts. Give truthful conversation references when available; otherwise describe the supplied message without fabricating an ID. A pasted link without content does not count as reading that source.

### Confidence summary

Separate confidence in present mechanics, change history, and historical motivation. Use Direct / Supported / Inferred / Speculative / Unknown consistently.

### Constraints for a subsequent change

Include when this investigation precedes a code change. Give **Preserve / Change / Avoid / Risk**, distinguish evidence-backed constraints from uncertainties, and note that this report does not authorize implementation or satisfy architect's design approval.

## Final check

Current code must be analyzed, not replaced by commit titles or ticket prose. Every factual claim needs its actual source; every intent claim needs an explicit statement or a labeled inference. Preserve contradictions and gaps. Check that the provided/partial/skipped/pending distinction is truthful and that no child or synthesis worker has asked the human a second time.
