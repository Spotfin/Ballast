# The build guide: decompose the plan into reviewable blocks

## The pattern

Take the technical plan and decompose it into **blocks**: units of work sized so that one block becomes one PR that one person can actually review. Each block gets three things written down before work starts: **scope**, **out of scope**, and **acceptance criteria**. Blocks are grouped into milestones that each end in something demonstrable.

The build guide is the execution plan. It changes more often than the technical plan and that is fine; the point is that it changes *visibly*, in a document, rather than silently in the AI's working assumptions.

## Why blocks, specifically

With an AI collaborator, the human's review capacity is the bottleneck of the whole system. The AI can produce a week of code in an afternoon. If the unit of work is "the feature", you get thousand-line PRs, review becomes skimming, and skimming is how a guardrail violation ships. Blocks keep every diff at a size where real review is possible, which is the only kind of review that counts.

The three fields do distinct jobs:

- **Scope** tells the AI what to build. Specific enough that two readers would build roughly the same thing.
- **Out of scope** is the more important field. It is where you pre-empt the AI's helpfulness: the adjacent refactor, the extra endpoint, the "while I was in there". Every out-of-scope line is drift you declined in advance instead of during review.
- **Acceptance criteria** make "done" checkable. Each criterion should be verifiable by running something or looking at something specific. "Works correctly" is not a criterion; "searching an unknown phrase returns the empty state, not an error" is.

## Sizing rules of thumb

- A block should be reviewable in under 30 minutes of honest attention.
- If you cannot write acceptance criteria for it, it is not one block, it is several (or it is research, which gets time-boxed instead).
- If two blocks cannot be described without referencing each other's internals, merge them.
- A block that touches a hard rule (see `guardrails/hard-rules.md`) gets flagged as such in the guide, and its PR gets reviewed with that rule open in the other window.

## Worked example: one milestone

From the fictional rulebook-navigator product. Milestone 2 of its build guide: "A user can search informal language and see matching official terminology with citations."

> ### Milestone 2: informal search works end to end
>
> Demonstrable outcome: type "can I do X without a permit", see ranked official-terminology matches, each with verbatim rule text and a section citation.
>
> **Block 2.1: knowledge base read layer**
> - Scope: load and validate knowledge base entry files at build time; expose entries to the publish step; reject entries missing `source` or `confidence` fields.
> - Out of scope: any write/ingest tooling; ranking; the search index format.
> - Acceptance: publish step fails with a named file and field when an entry is invalid; valid corpus loads and entry count is logged; the validation failure is a build failure, not a warning (fail closed, see `ci-spec.md`).
> - Touches hard rules: HR-3 (provenance mandatory). Flag for review.
>
> **Block 2.2: informal-phrase normalization**
> - Scope: normalize user input and entry phrases for matching (case, punctuation, defined synonym folding). Table-driven, table lives with the knowledge base.
> - Out of scope: fuzzy or semantic matching; any change to entry file format.
> - Acceptance: fixture table of 20 synthetic input/expected pairs passes; unknown input passes through unchanged rather than erroring.
>
> **Block 2.3: search endpoint over the compiled index**
> - Scope: query the compiled index, return ranked matches with citations. Runs client-side per D-001.
> - Out of scope: result rendering; ranking beyond the plan's stated ordering; analytics of any kind.
> - Acceptance: known-phrase fixture returns its expected entry first; unknown phrase returns the empty state; no user input is transmitted off the device (verify: zero network calls fired from the search path in the test harness).
>
> **Block 2.4: verbatim rendering path**
> - Scope: render official text exactly as stored, with citation, in the result view.
> - Out of scope: styling polish; explanation copy.
> - Acceptance: rendered text is byte-identical to stored text for the full fixture set; every rendered rule shows its citation; no template or helper in this path can transform rule text (assert the render path calls no string-mutating helpers on it).
> - Touches hard rules: HR-1 (never paraphrase). Flag for review.

Notice the pattern in the acceptance criteria: the ones that matter most are restatements of hard rules as checks. That is deliberate. The build guide is where guardrails stop being principles and start being tests.

## Keeping it current

- When a block's scope changes mid-flight, edit the guide in the same PR and say so in the build log under Deviations.
- When a whole milestone reshuffles, that is usually a decision-log entry.
- Completed blocks stay in the document, marked done. The guide doubles as a history of what the plan actually was, which is worth more than a clean-looking document.
- The guide carries "Last updated" and "Last reviewed" lines under its title. Rereading it at the start of each milestone and refreshing the reviewed date is a cheap habit, and it is the difference between a guide that describes the build and one that quietly became fiction.

---
*Ballast v0.1*
