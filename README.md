# Ballast

**What keeps a fast AI build upright.**

A guardrail methodology for building IP-protected products with AI, without losing control of the plan.

## Who this is for

Solo founders and small teams building a real product with an AI collaborator. "Real" means the product has actual proprietary knowledge, compliance stakes, or both. If you are hacking together a weekend prototype, you do not need this. If you are building something where a leaked data source, a silently skipped rule, or a quietly rewritten plan would genuinely hurt you, you do.

AI collaborators are fast. That is the point of using them, and it is also the risk. Speed without structure produces a codebase you no longer understand, a plan that drifted while you were not looking, and guardrails that exist only as good intentions. Ballast turns "be careful with AI" into a specific, checkable set of artifacts: documents you write once, keep current, and can point to when you or the AI need to know what the rules are.

## What this is, and is not

Ballast is a documented methodology. Every pattern here was pulled from a real build of a compliance-shaped product with an AI collaborator, then stripped of everything specific to that product and re-illustrated with generic worked examples.

It is not a starter template. There is no code to run, no CLI to scaffold with, nothing to clone and forget. Each document explains a pattern in prose and shows a worked example so you can see what "done" looks like. You are meant to read it, argue with it, and write your own versions by hand. The writing-by-hand part is not incidental. A guardrail you did not think through is a guardrail you will not defend when the AI proposes a shortcut.

It is also not generic AI-coding-tips content. Nothing here is theorized in the abstract. Every artifact earned its place by catching a real problem or preventing one.

## The core idea

Fast AI builds fail in predictable ways: the plan drifts, decisions evaporate, rules erode, and knowledge scatters. Each artifact in this repo exists to stop one of those failure modes.

| Failure mode | Artifact |
|---|---|
| Architecture gets relitigated every session | `docs/technical-plan.md` |
| Work blobs together, nothing is reviewable | `docs/build-guide.md` |
| "It works" replaces "it is done" | `docs/testing-and-definition-of-done.md` |
| Guardrails exist only as intentions | `docs/ci-spec.md`, `guardrails/hard-rules.md` |
| The AI forgets what project it is in | `CLAUDE.md.example` |
| Progress and drift become invisible | `BUILD-LOG.md.example` |
| Decisions get remade, badly, from memory | `DECISION-LOG.md.example` |
| Review becomes a rubber stamp | `github-templates/` |
| Proprietary knowledge scatters or leaks | `knowledge-map/` |
| The UI fragments under speed pressure | `design-system/` |

## What is in this repo

```
Ballast/
├── README.md                    You are here
├── LICENSE                      MIT
├── CLAUDE.md.example            Workspace-level agent orientation doc, worked example
├── BUILD-LOG.md.example         Done / Deviations / Next session log, worked example
├── DECISION-LOG.md.example      Decision record format, worked example
├── docs/
│   ├── technical-plan.md        Settle architecture once, separately from execution
│   ├── build-guide.md           Decompose the plan into PR-sized, reviewable blocks
│   ├── testing-and-definition-of-done.md
│   └── ci-spec.md               Guardrails as fail-closed CI checks
├── guardrails/
│   └── hard-rules.md            Write the non-negotiables down before the features
├── github-templates/            Worked examples of PR and issue templates
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
│       ├── decision-needed.md
│       ├── bug.md
│       └── task.md
├── knowledge-map/
│   ├── README.md                Structuring a proprietary knowledge base
│   └── source-profiles.md       Decide how to handle each source type before ingesting any
└── design-system/
    └── README.md                One design system, one rule that overrides convenience
```

## The worked example

The documents in this repo share a running example: a fictional small product that helps users navigate a body of regulated, jargon-heavy rules by translating plain language into the official terminology. It is generic on purpose. It has the two properties that make this methodology worth the overhead: a proprietary knowledge base that is the actual moat, and compliance-shaped constraints where getting something wrong has consequences beyond a bad user experience. When you adapt these documents, replace the example with your own product. The shape of the artifacts is the point, not the example content.

## How to adapt this repo

1. Read `docs/technical-plan.md` and `guardrails/hard-rules.md` first. Everything else hangs off those two.
2. Write your own hard rules before you write features. If you cannot name your non-negotiables, you are not ready to hand an AI the keyboard.
3. Write a technical plan and get it stable. Then write a build guide that decomposes it into blocks.
4. Copy the `.example` files into your workspace, gut the example content, and fill in your own. Keep the section structure. The structure is what was tested, not the words.
5. Set up the PR and issue templates in your repo's `.github/` folder, adapted to your own guardrails.
6. Keep the build log and decision log current from day one. They are cheap to maintain and expensive to reconstruct.

## A note on scale

This methodology assumes one product, one to a few humans, and one or more AI collaborators doing a large share of the implementation. At larger team sizes you likely have process that covers some of this already. The pieces most worth stealing at any scale: hard rules written before features, fail-closed CI as the enforcement layer, and a decision log with reversal triggers.

## License

MIT. See [LICENSE](LICENSE).
