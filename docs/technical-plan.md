# The technical plan: settle architecture once

## The pattern

Write one document that settles the architecture, before feature work starts, and keep it separate from the execution plan. The technical plan answers "what are we building and out of what". The build guide (see `build-guide.md`) answers "in what order, in what size pieces". Mixing them is the classic failure: a plan that interleaves architecture with task lists gets edited constantly, and every edit is a chance for the architecture to drift without anyone deciding it should.

The reason this matters more with an AI collaborator: an AI will happily redesign your architecture mid-task, articulately, with confidence, because from inside a single session the redesign looks like an improvement. It has no memory of the three sessions where you already considered and rejected that design. The technical plan is that memory. It converts "the architecture is whatever the current session thinks" into "the architecture is what the document says, and changing the document is a deliberate act".

## What goes in it

Keep it short. Five to ten pages covers a real product. Sections that earn their place:

1. **System shape.** The components, what each owns, and the boundaries between them. Prose and one diagram, not a specification.
2. **Data model at the level of nouns.** The core entities and their relationships. Not full schemas; those live with the code and change more often than the nouns do.
3. **Where the IP lives and how it is protected.** For a product with proprietary knowledge, name the asset explicitly, where it is stored, and what is never allowed to happen to it. This section feeds directly into the hard rules.
4. **Load-bearing technology choices.** Only the choices that would be expensive to reverse: language, storage shape, hosting model, the boundary between client and server. Each with one paragraph of why. Everything else is a library choice and does not belong here.
5. **What we are explicitly not building.** The features and generality that were considered and cut. This section does more work than any other, because most drift arrives disguised as "while we're at it".

## What stays out

- Task breakdowns, milestones, sequencing. That is the build guide.
- Anything that changes weekly. If a section needs editing every sprint, it was execution detail, not architecture.
- Aspirational scale. Design for the product you are building, note the growth path in one line, move on.

## The stability rule

Once the plan is settled, it changes only through the decision log. A change to the technical plan is a decision-log entry with context, rationale, and reversal triggers, and the plan cites the entry (e.g. "Storage: flat files, see D-003"). This gives you exactly one door through which architecture can change, and a guest book of everyone who came through it.

The corollary for the AI collaborator, worth stating verbatim in your CLAUDE.md: do not relitigate the technical plan inside a feature PR. If the plan seems wrong, that is a decision-needed issue, not a refactor.

Give the plan two freshness lines under its title: "Last updated" (content changed) and "Last reviewed" (read and confirmed still true). Updating and confirming are different acts. A plan nobody has reviewed in three months is not settled, it is unexamined, and the gap between the two dates is what tells you which one you have.

## Worked example (abridged)

An abridged technical plan for the fictional rulebook-navigator product used throughout this repo:

> **System shape.** Three parts. (1) The knowledge base: versioned flat files mapping informal phrases to official terminology, with provenance. (2) A publish step that compiles the knowledge base and the official rule text into a static search index. (3) A client app that searches the index locally and renders official text verbatim with citations. There is no runtime server component that handles user queries.
>
> **Where the IP lives.** The knowledge base is the moat. It exists in one private repo, is never bundled raw into the client (only the compiled index ships), and no real user-submitted text is ever added to it. Provenance is mandatory per entry.
>
> **Load-bearing choices.** Static publish over live API: user queries never leave the device, which collapses the privacy surface to nearly nothing (D-001). Flat files over database: reviewability of the IP beats query convenience at current scale (D-003).
>
> **Not building.** Accounts, personalization, user-generated content, a general-purpose rules engine, and any feature that requires receiving user queries server-side. Each of these was cut because it either enlarges the compliance surface or dilutes the one asset that matters.

Notice what the example does: every architectural choice is traceable to either the IP or the compliance constraint. That is the test of a good technical plan for this kind of product. If a choice cannot be traced to something that matters, it is a preference, and preferences do not need a plan.

## Signs it is working

- Feature PRs stop containing architecture arguments.
- New sessions with the AI start from "read the technical plan" instead of a fresh explanation.
- When the architecture does change, you can point to the day, the reason, and the trigger that was hit.

---
*Ballast v0.1*
