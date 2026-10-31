# Epistemics

Keep present mechanics, historical changes, and motivation separate. Current code is mandatory evidence of the mechanism; it does not establish why an author chose that mechanism. Commit messages, comments, and human-supplied ticket context can state intent, but may be incomplete, stale, or contradictory.

## Confidence tiers

### Direct

An explicit source establishes the specific claim. Cite current code for what it implements, the actual diff for what changed, or an attributed statement for a stated reason. For example, a commit says "retry five times because upstream requests sometimes time out". Report that the commit states this rationale, then separately check what current retry code does. A title or comment alone cannot prove the change achieved its intended outcome.

Cite ticket excerpts as **User-provided context for MOL-12345**. This is direct evidence of what the user supplied, not proof that you read Jira or that every historical detail is independently verified.

### Supported

Multiple independent pieces of indirect evidence converge. Name each source and its contribution. A commit title, its copied text in a comment, and a user repeating that title are not three independent confirmations. Say "The evidence points toward X" and distinguish supported interpretation from an explicit record of intent.

### Inferred

A reasonable interpretation without an explicit statement of intent. Show the chain and qualify it: "Given the changed retry path and surrounding callers, this appears intended to reduce duplicate requests." Do not state author intent as fact just because the current code seems sensible.

### Speculative

A plausible explanation on thin evidence with viable alternatives. Say "One possibility is X, but we found no direct evidence". Keep alternatives visible rather than forcing a winner.

### Unknown

The evidence does not settle it. State what you read and what is missing. For example, "The current code allows five attempts; the available patches show when three became five, but the user skipped ticket context and the messages do not explain that choice." Unknown is a useful result, not a reason to invent a story.

## Contradictions

Keep both sources with their locations. A comment promising three attempts and a loop allowing five conflict. A supplied ticket excerpt requiring one action and code performing another also conflict. A title saying "remove retries" and a patch adding them conflict. Report what each says without assuming code, comment, title, or ticket is authoritative and current. A static mismatch is not automatically a runtime bug; name what would verify it without editing the product.

## Attribution and missing evidence

- Attribute local facts to file:line/symbol or commit hash and parent/current diff.
- Attribute supplied context to the actual human message and ticket key. Use a real conversation reference when available; do not invent one.
- Never claim you queried Jira or another external source. A link alone is not its content.
- Distinguish context provided, partial submission, explicit skip, and still pending. Silence does not mean skip.
- New code or no ticket skips only the question. Still read and explain the code and any available lineage.
- Missing Git, a shallow boundary, or a missing object is an evidence limit, not proof of no change history.

## Avoid rationalization

Do not work backward from a tidy present-day design to a fictional original intent. Do not treat a user's suggested explanation as a conclusion to validate. Do not turn absent evidence into proof that a concern never existed. Separate any historical rationale from whether it still applies today.

Before returning, check each claim against its source and tier. Preserve hedges for Inferred/Speculative claims, surface contradictions, and include concrete gaps. State separately how certain you are about mechanics, historical changes, and motivation.
