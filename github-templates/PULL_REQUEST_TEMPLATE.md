# Pull request template (worked example)

This is a worked example of a PR template built around a guardrail checklist, using the fictional rulebook-navigator product's hard rules. Adapt the categories and items to your own `guardrails/hard-rules.md`. To use it for real, place your adapted version at `.github/PULL_REQUEST_TEMPLATE.md` in your repo.

Two design notes before the template itself:

**Group the checklist by category, not as one long list.** A flat list of twelve checkboxes gets pattern-clicked. Grouping by concern (scope, guardrails, testing, records) forces a mental context switch per group, which is where actual checking happens.

**"N/A + reason" beats an unchecked box.** Every item must end in one of three states: checked, N/A with a stated reason, or a written explanation of why it is unchecked and what happens next. An untouched checkbox is ambiguous: was it not applicable, not done, or not read? Ambiguity in a checklist compounds silently, and with an AI collaborator filling in PR descriptions, an unchecked box will simply stop being noticed. Requiring a reason for N/A keeps the item read even when it is skipped.

---

```markdown
## Block

Build-guide block: <!-- e.g. Block 2.4. Every PR maps to exactly one block. If this PR has no block, explain why it exists. -->

## What this does

<!-- Two or three sentences. What changed and why, in terms a reviewer a month from now will understand. -->

## Scope check

- [ ] The diff stays inside the block's stated scope
- [ ] Out-of-scope touches (if any) are listed below with reasons
- [ ] No architecture changes (anything touching the technical plan cites a decision-log entry: D-___)

Out-of-scope touches:
<!-- List each, one line, with reason. "None" if none. -->

## Guardrails

<!-- Check against guardrails/hard-rules.md. N/A requires a reason. -->

- [ ] HR-1: no user-facing copy in this diff asserts what a rule requires (or N/A: ___)
- [ ] HR-2: no real user text in code, fixtures, or comments; all new fixtures are synthetic (or N/A: ___)
- [ ] HR-3: knowledge base entries touched by this PR all carry source and confidence (or N/A: ___)
- [ ] HR-5: nothing from the protected asset appears in this PR's description, comments, or public artifacts (or N/A: ___)
- [ ] No CI check was modified, skipped, or weakened in this PR (if one was, link the reviewed justification: ___)

## Testing

- [ ] Acceptance criteria for the block pass, as written in the build guide
- [ ] New behavior has tests that fail when the behavior breaks
- [ ] No existing assertion was weakened (any modified test is explained below)
- [ ] If this block is flagged as touching a hard rule: a guardrail-shaped test exists and was shown failing when the rule is deliberately broken

Modified tests:
<!-- "None" or list with reasons. -->

## Records

- [ ] Build log updated (Done / Deviations / Next)
- [ ] Deviations from the build guide, if any, are noted there
- [ ] Any new decision this PR quietly embodies has been extracted into a decision-needed issue or decision-log entry instead of shipping silently
```

---

## Notes on adapting it

- Keep the guardrail items in one-to-one correspondence with your numbered hard rules, and keep the numbers visible. The template is one of the main places rule numbers stay alive.
- The last item under Records is the subtle one. PRs are where undeclared decisions hide: a tie-breaker chosen ad hoc, a default that becomes permanent. The checklist item exists to make the author ask "did I just decide something?" once per PR.
- Resist adding items. Every item you add makes every previous item slightly less read. If an item has been N/A on twenty consecutive PRs, consider whether it belongs in the template at all or only in the blocks flagged for it.

---
*Ballast v0.1*
