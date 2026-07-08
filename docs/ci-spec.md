# CI as the enforcement layer: guardrails as checks

## The pattern

Every guardrail that can be expressed as a machine check becomes a CI check, and every such check **fails closed**: when the check cannot run, cannot decide, or finds something ambiguous, the build fails. A guardrail that lives only in a document is a request. A guardrail that fails the build is a rule.

This document describes the pattern in prose. It deliberately contains no pipeline configuration, because the checks worth having are specific to your product and your hard rules, and copying someone else's pipeline is how you end up with green checkmarks that verify nothing you care about.

## Why CI is the right enforcement point

With an AI collaborator, enforcement has to live somewhere that does not depend on anyone's attention on a given afternoon. The human forgets, the AI's context window closes, but the pipeline runs every time. CI is also the one place where "no" is cheap: a failed check blocks a merge without an argument, without anyone having to be the person who says no, and without relying on the AI to remember that it agreed to a rule three sessions ago.

The discipline this requires from you: when a check blocks something you want to ship, you fix the work, not the check. The moment a check gets disabled "temporarily" to make a deadline, every check becomes advisory. This is worth writing into your hard rules explicitly (see HR-4 in `guardrails/hard-rules.md`).

## Fail closed, concretely

Fail-open is the default posture of most tooling: if the linter crashes, the build continues; if a file is unparseable, it is skipped. For guardrail checks, invert this:

- Check cannot parse a file it is supposed to validate: **fail**, do not skip the file.
- Check's input is missing (the corpus moved, the config path changed): **fail**, do not report "0 problems found".
- Check finds a case it does not recognize: **fail** and name the case, do not default to allow.
- Check itself errors: **fail the build**, do not continue without it.

The reasoning: for a compliance-shaped product, the cost of a silently skipped check is unbounded, while the cost of a false failure is a few minutes of annoyance. Fail-closed converts unknown risk into visible friction, which is the trade you want everywhere the stakes are real.

## Turning hard rules into checks: a worked example

The fictional rulebook-navigator product (see `guardrails/hard-rules.md` for its rules) derives its checks like this:

- **HR-1 (never paraphrase rules)** becomes: a check that scans user-facing strings for rule-shaped assertions (a maintained list of patterns like "you must", "you may not", "it is legal to"). Matches fail the build and must be either rewritten or added to an allowlist with a written justification in the same commit. The allowlist is itself reviewed like code.
- **HR-2 (no real user text in fixtures or the knowledge base)** becomes: fixtures may only originate from a designated synthetic-fixtures directory, and a check verifies test code references no other data source. This one is structural rather than content-scanning, because content-scanning for "is this real user text" is unwinnable; instead the check makes the safe path the only path.
- **HR-3 (provenance mandatory)** becomes: every knowledge base entry must carry `source` and `confidence` fields with values from the defined vocabulary, links must reference entries that exist, and orphaned or malformed entries fail the build with the filename. This check is the exhaustively-tested Tier 2 asset described in `testing-and-definition-of-done.md`.
- **HR-4 (guardrails beat deadlines)** cannot be a check on the code, so it becomes a check on the process: branch protection requires all checks green, no administrator bypass, and the PR template asks whether any check was modified in this PR and why.

Notice the pattern in the second example: when a rule cannot be verified directly, restructure so the compliant path is the only expressible one, then verify the structure. That move is often available and always stronger than scanning.

## What to check on every merge

A reasonable starting shape, in prose:

1. The ordinary suite: build, tests, lint.
2. The guardrail checks derived from your hard rules, as above.
3. IP-protection checks: nothing from the protected asset's directory is bundled into any public-facing artifact; no protected file appears in the diff of a repo that is ever mirrored publicly.
4. A checklist-integrity check if you can manage it: the PR body contains the template's required sections (cheap to check, and it keeps the template from decaying into decoration).

## Maintenance

- Every check has an owner comment: which hard rule or decision it enforces, with the ID. A check nobody can explain gets deleted by accident eventually.
- When a check fires falsely more than rarely, fix the check's precision rather than training yourself to ignore it. An ignored check is worse than no check, because it launders risk as green.
- New hard rule: same PR adds the rule text and its check, or documents why no check is possible yet and opens a task for it.
