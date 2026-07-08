# Issue template: decision needed (worked example)

The `decision-needed` issue is the escape valve of the whole methodology (see HR-6 in `guardrails/hard-rules.md`). When work surfaces a genuine choice with tradeoffs, whoever hits it (human or AI) stops, files one of these, and does not pick silently. When the issue is resolved, the outcome is written into `DECISION-LOG.md` as a numbered entry, and the issue closes with a link to that entry. The issue is the discussion; the log entry is the record. Issues get lost, the log does not.

To use for real, adapt and place at `.github/ISSUE_TEMPLATE/decision-needed.md`.

---

```markdown
---
name: Decision needed
about: A choice with real tradeoffs that should not be made silently
labels: decision-needed
---

## The decision

<!-- One sentence, phrased as a question. "How should search results be ordered when confidence levels tie?" -->

## What forced this

<!-- Where it came up: block, PR, or session. Why it cannot be quietly defaulted. -->

## Blocked work

<!-- What is waiting on this, if anything. "Blocks Block 2.5" or "Nothing blocked, but the default is spreading". -->

## Options

<!-- For each option: one line of what it is, and its strongest point FOR and AGAINST. Two or three options is right; one option is not a decision, five needs narrowing first. -->

**Option A:**
- For:
- Against:

**Option B:**
- For:
- Against:

## Which hard rules or prior decisions are in play

<!-- Cite by number: HR-2, D-003. "None" is a valid answer but say it explicitly. -->

## Recommendation (optional)

<!-- The filer may recommend, and must still not implement before resolution. -->

---
**On resolution:** write the outcome as a new entry in DECISION-LOG.md (Decision / Context / Rationale / Reversal triggers / Status), comment here with the entry number, and close.
```

---

## Notes on the pattern

- The "which rules are in play" section is what makes this template different from a generic discussion issue. It forces the connection to the standing rules to be checked at filing time, when context is fresh, not at resolution time from memory.
- An AI collaborator should be instructed (in your CLAUDE.md) to file these instead of choosing when it hits a genuine fork. In practice this is one of the highest-value behaviors to establish: the cost of a filed issue is minutes, the cost of a silently embedded decision is finding it six weeks later, load-bearing.
- Watch for decision-needed issues that sit open while the default quietly ships anyway. That is the process failing politely. If an issue blocks nothing for two weeks, either decide it or explicitly log "deferred, default stands until trigger X" in the decision log.
