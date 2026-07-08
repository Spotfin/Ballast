# One design system, one rule that overrides convenience

## The pattern

A small product built at AI speed needs exactly one design system, adopted early, and **one named rule that beats convenience every time it conflicts with it**. Not a brand book, not a component library you wrote yourself, not taste. One system, one override rule, written down where the AI collaborator reads it.

## Why the UI fragments under speed pressure

An AI collaborator produces interface code the way it produces everything else: fast, locally plausible, globally inconsistent. Each session solves today's screen with today's idea of a button. None of the choices are wrong in isolation; the product just stops looking or behaving like one thing. Human teams drift this way over quarters. An AI-heavy build can do it in two weeks, because the "team member" writing most of the UI has no memory of last week's screens.

The fix is not vigilance ("keep the UI consistent" is exactly the kind of instruction that erodes). The fix is structural, and it is cheap:

1. **Pick one existing design system** and take its defaults wholesale. Which one matters far less than that there is exactly one. Every component question the AI would otherwise improvise on ("what does a destructive action look like?") now has a boring, look-up-able answer.
2. **Write one override rule** derived from who your users actually are and what the product's stakes are. This is the rule that wins when it conflicts with the system's defaults, with a nicer-looking option, or with implementation convenience.
3. **Put both in the AI's orientation document** (your CLAUDE.md), phrased as rules, not preferences.

## Why one rule, and not a style guide

Because one rule gets enforced and twelve get skimmed. The override rule is doing a different job than the design system: the system provides consistency, the rule encodes the single most important thing about *your* users that a generic system does not know. Ruthlessly demoting everything else to "the system's defaults" is what makes the one rule stick, in review, in CI heuristics where possible, and in the AI's behavior.

The rule should be concrete enough to check in a PR. "Accessible" is a value. "Nothing interactive smaller than X" is a rule.

## Worked example

The fictional rulebook-navigator product used throughout this repo serves people who consult it in the field: outdoors, mid-task, on a phone, often gloved or one-handed, deciding whether they may proceed with physical work. Its design decisions:

> **System:** one mainstream component system, defaults accepted wholesale. No custom components without a decision-log entry.
>
> **Override rule (DS-1): Everything must survive the worst realistic viewing conditions.** Concretely: every interactive target at least 48px; body text never below 16px; contrast at the standard's stricter tier, not the minimum; every state distinguishable without color alone; the primary lookup flow fully usable one-handed on a small phone screen. When this conflicts with the design system's default, a prettier layout, or fitting more on screen, DS-1 wins, every time, without a meeting.
>
> *Why this rule for this audience: the user is standing in bad light with dirty hands, about to act on what the screen says, in a domain where acting on a misreading has real consequences. A control too small to hit or a distinction too subtle to see is not a polish issue in this product; it is adjacent to a wrong answer. That makes legibility-under-stress the one property that must never lose a tradeoff.*

Note the shape of the worked example: the rule is derived from a *specific audience in a specific situation*, stated with checkable numbers, and its rationale connects to the product's stakes rather than to design principle in the abstract. Your rule will be different because your users are different. A product for late-night use has a rule about dark-mode and alarm-clock legibility; a product for data-entry professionals has a rule about keyboard-only speed. The method is: find the situation where your user is most vulnerable to bad UI, and write the rule that protects exactly that.

## Keeping it enforced

- The rule gets an ID (DS-1) and appears in the PR checklist for any UI-touching block, same N/A-with-reason mechanics as the hard rules.
- Parts are mechanically checkable: minimum sizes and contrast can be linted or tested. Do it where cheap, per `../docs/ci-spec.md`, fail closed.
- Exceptions go through the decision log. If exceptions become frequent, the rule is either wrong (fix it deliberately) or the product is drifting from its audience (worth knowing early, and this is how you find out).

---
*Ballast v0.1*
