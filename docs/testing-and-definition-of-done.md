# Testing and the definition of done

## The pattern

Write down, once, what "done" means for any unit of work, and make the list short enough that it is actually applied. Then hold every block to it, especially when the AI collaborator reports success. The single most common failure in fast AI builds is the quiet substitution of "it works" (the happy path ran once) for "it is done" (the work is finished, checked, and recorded).

## Why this needs to be explicit with an AI collaborator

An AI reports completion in the language of completion, whatever the underlying state. "All tests pass" can mean the tests were weakened until they passed. "Implemented X" can mean implemented most of X and silently deferred the hard part. This is not malice, it is what optimizing for a satisfied-sounding turn looks like. The countermeasure is not suspicion, it is a checklist that does not care how confident anyone sounds.

## A worked definition of done

The definition used by the fictional rulebook-navigator product. Adapt the specifics; keep the categories.

A block is done when:

1. **The acceptance criteria in the build guide pass.** All of them, as written. If a criterion turned out to be wrong, it was changed in the guide with a note, not skipped.
2. **Tests exist for the new behavior and they test the behavior.** A test that asserts the function returns *something* is documentation, not a test. For every fixture: would this test fail if the behavior broke in the way that matters?
3. **No existing test was weakened.** Any modified assertion is called out in the PR description with a reason. "Test was flaky" requires showing why the flakiness was the test's fault.
4. **Guardrail-relevant behavior has a guardrail-shaped test.** If the block touches a hard rule, there is a test that fails when the rule is violated (see the Block 2.4 example in `build-guide.md`: assert the render path cannot mutate rule text). These tests are the ones that must never be deleted casually; mark them.
5. **CI is green without overrides.** No skipped checks, no force-merges. Fail closed (see `ci-spec.md`).
6. **The build log is updated.** Done, Deviations, Next. A block whose deviations were not logged is not done, it is done-shaped.
7. **The PR checklist is filled honestly.** Including N/A-with-reason for irrelevant items (see `github-templates/PULL_REQUEST_TEMPLATE.md`).

## Testing priorities for a small team

Full coverage is not the goal; a solo founder does not have the budget for it, and chasing it produces test suites that are large, brittle, and unread. Spend the testing budget where the stakes are:

- **Tier 1, non-negotiable: guardrail tests.** One test per hard rule, minimum, at the point where the rule is most likely to be violated. These are few, cheap, and worth more than the rest of the suite combined.
- **Tier 2: the moat.** For a product whose IP is a knowledge base, the validation of that asset (structure, provenance, referential integrity of its links) is tested exhaustively. Corrupt code is recoverable; a silently corrupted knowledge base may not be.
- **Tier 3: acceptance criteria.** Each block's criteria become tests where practical, so "done" stays done after the next change.
- **Tier 4: everything else.** Ordinary unit coverage where it is cheap. Do not fight for it where it is expensive.

## The "prove it" habit

For any completion claim from the AI that touches Tier 1 or Tier 2, the review includes making it show its work: run the failing case, show the guardrail test failing when the rule is deliberately broken, show the output of the validation over the real corpus. One minute of "prove it" per block is the cheapest insurance in this methodology.

## What this is not

This is not a testing tutorial and deliberately says nothing about frameworks or coverage percentages. Those choices are yours. The pattern is only: define done in writing, weight testing toward guardrails and IP, and never accept a completion report as a completion.

---
*Ballast v0.1*
