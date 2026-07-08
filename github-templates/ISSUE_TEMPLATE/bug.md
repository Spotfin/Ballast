# Issue template: bug (worked example)

Severity comes first in this template, before reproduction steps, before anything. That ordering is deliberate: for a compliance-shaped product, the first question about any bug is not "how do I reproduce it" but "is this the kind of bug that means we stop other work". A guardrail violation in production is not a bug like a misaligned button is a bug, and the template should force that sorting at filing time.

To use for real, adapt and place at `.github/ISSUE_TEMPLATE/bug.md`.

---

```markdown
---
name: Bug
about: Something behaves in a way it should not
labels: bug
---

## Severity

<!-- Pick one. Be honest; upgrading later is embarrassing, downgrading later is fine. -->

- [ ] **S1: guardrail or trust violation.** A hard rule is being violated, protected data is exposed, or the product asserts something false in the regulated domain. Stop-the-line: this preempts feature work.
- [ ] **S2: wrong answers.** The product gives incorrect results (wrong entry matched, wrong text shown, wrong citation) without violating a hard rule. Fix before the next release.
- [ ] **S3: degraded.** Correct but broken around the edges: errors, slowness, dead ends. Schedule normally.
- [ ] **S4: cosmetic.** Nobody is misled and nothing is blocked.

If S1: which hard rule? HR-___

## What happens

<!-- One or two sentences of observed behavior. -->

## What should happen

<!-- And on what basis: build-guide acceptance criterion, hard rule, decision-log entry, or common sense. Cite the number if there is one. -->

## Reproduction

<!-- Steps, environment, and a synthetic example input. Per HR-2: never paste real user input into an issue. Reconstruct a synthetic equivalent. -->

## First-seen

<!-- If known: which block or PR introduced it. "Unknown" is fine. -->
```

---

## A worked example filing

> **Severity:** S2: wrong answers.
> **What happens:** searching a two-word informal phrase where one word is in the synonym table returns matches for the synonym-expanded word only, dropping the second word from the query.
> **What should happen:** both words participate in matching. Block 2.2 acceptance criterion 1 (fixture table passes) apparently did not cover multi-word inputs where only one word has a synonym.
> **Reproduction:** synthetic input "flarn permit" where "flarn" is in the synonym table; matches returned are identical to searching "permit" alone.
> **First-seen:** likely Block 2.3.

Note what the example does: it identifies the acceptance-criteria gap, not just the bug. A good S2 filing usually implies a missing fixture, and the fix PR should add it.

## Notes on the pattern

- The severity definitions must be written in terms of *your* product's stakes, not generic P1/P2 language. "Guardrail violation" and "wrong answer" are different tiers here precisely because the worked-example product's harm model says so. Rewrite the tiers from your hard rules.
- S1 preempting feature work is only real if it has been agreed in advance. That agreement is this template plus your hard rules; deadline-day is too late to negotiate it.
- The HR-2 reminder inside the reproduction section is an example of a guardrail enforced at the template level: the rule appears exactly where it is most likely to be broken.

---
*Ballast v0.1*
