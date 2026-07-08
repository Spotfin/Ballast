# Hard rules: write the non-negotiables before the features

## The pattern

Before feature work starts, write down the short list of things that must never happen in this product, number them, and give each one a sentence of rationale. These are the hard rules. Everything else in this methodology points back at them: the CLAUDE.md tells the AI to read them, the build guide flags blocks that touch them, the PR template checks against them, and CI enforces the ones that can be enforced (see `docs/ci-spec.md`).

## Why before features

Two reasons, one human and one structural.

The human reason: non-negotiables written under deadline pressure are negotiable. The only time you can honestly decide what you will never trade away is before there is something tempting to trade it for. A rule written the week you need to break it is not a rule.

The structural reason: hard rules are load-bearing for every downstream artifact. The technical plan's "where the IP lives" section, the CI checks, the PR checklist categories, and the guardrail tests all derive from this list. Written first, the rules shape the system. Written later, they become a compliance layer bolted onto a system that already violates them somewhere.

## What qualifies as a hard rule

The list must stay short, five to ten items, or it stops being a list of non-negotiables and becomes a style guide nobody reads. Tests for inclusion:

- Violating it causes harm you cannot fully undo: legal exposure, leaked IP, lost user trust, corrupted core data.
- You would rather miss a deadline than break it. If not, it is a preference, not a hard rule.
- It can be stated in one or two sentences that two different readers would interpret the same way.

If something matters but fails these tests, it belongs in the definition of done, the build guide, or a lint rule. Not here.

## Worked example

The hard rules of the fictional rulebook-navigator product used throughout this repo: a product that maps plain-language questions to official terminology from a regulated rulebook, whose moat is a hand-curated knowledge base.

> **HR-1: The product never asserts what a rule requires.** It shows official text verbatim, with a citation. Explanation copy may define terms, never restate obligations.
> *Rationale: paraphrased requirements are where liability lives. Verbatim-plus-citation is defensible and mechanically checkable. (Origin: D-002 in the decision log.)*
>
> **HR-2: No real user-submitted text is ever stored, ingested, or used in fixtures.** All test data is synthetic. All knowledge base content comes from approved source types via their profiles (see `../knowledge-map/source-profiles.md`).
> *Rationale: the product must be able to say "we do not hold that" and mean it. One real query pasted into a fixture makes that sentence false forever.*
>
> **HR-3: Every knowledge base entry carries provenance and a confidence level.** No source, no merge. No exceptions for "obvious" entries.
> *Rationale: the knowledge base is the IP. Its value is exactly the trustworthiness of its provenance. An entry without a source is a rumor stored in the moat.*
>
> **HR-4: When a guardrail check conflicts with a deadline, the guardrail wins.** Checks are never disabled, skipped, or weakened to get a merge through. If a check is genuinely wrong, fixing it is its own reviewed PR.
> *Rationale: the first "temporary" bypass converts every check from a rule into a suggestion. This rule protects all the others.*
>
> **HR-5: The protected asset never leaves the private repo.** The knowledge base is not pasted into issues, chat logs, prompts to third-party tools with training rights, or public artifacts. Only the compiled index ships.
> *Rationale: the moat is only a moat while it is private. Leakage through casual channels (an issue comment, a shared prompt) is more likely than theft.*
>
> **HR-6: Anything that looks like it requires breaking HR-1 through HR-5 stops work and opens a decision-needed issue.** Silence is not consent; the absence of an explicit exception means no.
> *Rationale: this is the escape valve that makes the other rules livable. There is a defined thing to do when a rule seems wrong, so nobody has to improvise.*

Note the shape: each rule is one behavioral sentence plus one rationale sentence, and the rules reference the artifacts that operationalize them. HR-6 is the one most worth stealing verbatim; every hard-rule list needs its own escape valve, or the rules get broken quietly instead of questioned loudly.

## Keeping the rules alive

- Every rule gets a number, and the numbers are used everywhere: PR checklists, build-guide flags, CI check comments, decision-log entries. A rule that is never cited is either universally internalized or universally ignored, and it is worth finding out which.
- Rules change through the decision log only, like the technical plan. Adding a rule is cheap. Removing or weakening one requires a decision entry with reversal triggers, which is deliberately heavy.
- Once per milestone, reread the list against the product as it actually exists. The dangerous drift is not a violated rule, it is a rule that quietly stopped describing anything real. Record the reread by refreshing the file's "Last reviewed" freshness line (keep "Last updated" for content changes); the gap between those two dates is your drift meter.

---
*Ballast v0.1*
