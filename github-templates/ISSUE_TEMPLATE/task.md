# Issue template: task (worked example)

Tasks are where scope creep enters a small project wearing a lanyard. The template does two things to stop that: it ties every task to a build-guide block (or forces an explicit statement that it belongs to none), and it runs every task through a "does this actually matter" test before the task is allowed to exist.

To use for real, adapt and place at `.github/ISSUE_TEMPLATE/task.md`.

---

```markdown
---
name: Task
about: A unit of work someone intends to do
labels: task
---

## Task

<!-- One sentence, outcome-phrased. "Quarantined legacy entries re-sourced with provenance", not "look into entries". -->

## Build-guide block

<!-- Which block this belongs to, e.g. Block 2.2. If none: this task is proposing new scope. Say so, and expect it to be argued with. New scope normally means a build-guide edit first, then the task. -->

Block: ___

## Does this actually matter?

<!-- Answer at least one concretely, or close the issue yourself now and save everyone the trouble. -->

- What breaks, degrades, or stays risky if this is never done?
- Which hard rule, acceptance criterion, or decision does it serve? (Cite: HR-___, D-___, or block)
- Who notices when it is done?

## Size

- [ ] Under an hour
- [ ] A session
- [ ] Multiple sessions (why is this not a block of its own, with scope and acceptance criteria in the build guide?)

## Done means

<!-- One or two checkable statements. Mirrors build-guide acceptance criteria at task scale. -->
```

---

## A worked example filing

> **Task:** the 12 quarantined legacy knowledge base entries are re-sourced with provenance or deleted.
> **Block:** Block 2.1 follow-up (from build log 2026-03-10, Deviations).
> **Does this actually matter:** yes: HR-3 says no entry without provenance, and the quarantine folder is a standing exception to that rule. Every week it exists, "the knowledge base has full provenance" is false. If never done, 12 entries of the moat are rumors.
> **Size:** a session.
> **Done means:** quarantine folder is empty and deleted; each entry either has source and confidence and passes the CI validation, or was judged unsupportable and removed with a line in the build log.

## Notes on the pattern

- The "does this actually matter" test sounds rude and is the most valuable part of the template. Small teams with an AI collaborator generate task ideas faster than any team in history; the AI will happily propose ten plausible improvements per session. Most fail this test, and it is far cheaper to fail them at filing than to triage them forever. An honest "nothing breaks, nobody notices" answer is a gift: close it.
- The block linkage keeps the task backlog and the build guide from becoming two competing plans. If a task does not fit any block and still matters, the plan is missing something: fix the plan, then file the task.
- The size question's third option is a tripwire. Multi-session work managed as a loose task is how blobs of unreviewable work come back. It should graduate to a block with real scope and acceptance criteria.
