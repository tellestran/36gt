# Rationale template

The design handoff saved at `docs/designs/<task-slug>.md`. Keep it concise but complete. Sentence-case headings, no boilerplate. Replace the italic notes with actual content. All code sketches belong in fenced blocks here, not in application source. Replace template-relative links with links valid from the destination document.

## Problem

*One paragraph. What we're trying to do, the in-scope and excluded behavior, and what about the existing system or constraints makes the shape non-obvious. If [Phase A](../SKILL.md#phase-a-ground-the-problem) surfaced constraints the design must honor (existing types to interop with, callers we can't break, invariants that crossed our boundary), name them here so the reader sees the same constraints you saw.*

## Usage (caller's view)

*Write this first, before the type sketch. Show the README or quickstart the consumer reads, plus two or three realistic call sites in their own code. What they import, what they call, what comes back. The type sketch in [Shape](#shape) is derived from this. The two must agree. When they diverge, reconcile the sketch to the usage, not the reverse. The caller's experience is the spec. The types serve it.*

## Shape

*The recommended architecture. Data structures first. Include fenced sketches of types and signatures, a module map with ownership, and how data flows through the signatures. Name the load-bearing decisions. State which invariants are encoded in types, where validation lives, and what the system deliberately does not do. Judge interface depth explicitly. State what complexity the public surface hides, what remains exposed to callers, and why the interface is no larger than needed. Cite the principle behind each decision (e.g., `per boundary-discipline`). Don't restate it.*

## Synthesis decision

*Filled in by [arena](../../arena/SKILL.md). Records which candidate became the base and why, what was adapted from each of the others, and what was rejected and why.*

## Tradeoffs accepted

*One bullet per tradeoff the chosen shape makes. Form: "we accept X in exchange for Y." Name anything a future reader might mistake for an oversight, including things that look like premature optimization or premature simplification.*

## Alternatives considered

*Required. Name at least one concrete alternative shape, with one line on why it lost. Judge each alternative on interface depth, not implementation simplicity alone. Name the complexity it exposes to callers and the complexity it hides. Two or three alternatives belong here when the design space had real contenders. One is fine when the constraints forced the answer, with the conclusion phrased as "this was the only viable shape because..." Avoid listing flavors of the same shape. This section covers design alternatives the chosen shape considered and rejected, not other runner candidates.*

## Open questions and risks

*Things you noticed during the sketch that the human needs to weigh in on, and risks worth flagging before implementation starts. Phrase as questions, not assertions, so the human's answer is the resolution rather than a comment.*

## Implementation sequence

*Small verifiable units in dependency order, including migrations or compatibility work only when needed. Name the first unit that applier should build after explicit human approval. This section is a handoff, not permission to execute.*

## Verification criteria

*Observable outcomes and the existing commands, harnesses, or real surfaces that can check them. Include relevant failure paths, regression coverage, and known verification limitations. The implementer must be able to tell what constitutes a pass.*

## Review status

*Identify this design's revision and describe what changed when revising it. Start as Awaiting design approval. Record approval only when an explicit human message approves this revision, with a truthful pointer to that message if available. Neither an Approved label alone nor a parent agent's claim is evidence of human approval. Revised architecture needs fresh approval.*
