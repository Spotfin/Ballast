# The knowledge map: structuring a proprietary knowledge base

## The pattern

When a product's moat is knowledge rather than code, that knowledge needs the same engineering discipline as the code: structure decided up front, provenance on everything, and change control. The knowledge map pattern has three parts: a **Sources / Entries / Links** structure, a **confidence ladder**, and **typed links** that let evidence disagree with itself honestly.

The worked example throughout is the fictional rulebook-navigator product: its knowledge base maps informal, everyday phrasings to official terminology from a regulated rulebook, and that mapping is the entire competitive asset.

## Sources / Entries / Links

Three record types, kept separate:

**Sources** are where knowledge came from: a published study, a public forum thread, a practitioner interview, an official document. One record per source, holding the reference, the date collected, the source type, and which handling profile was applied (see `source-profiles.md`). Sources are append-only; you do not edit history.

**Entries** are the knowledge itself. In the worked example, an entry is one informal phrase mapped to one official term: the phrase, the term, a note on nuance ("people say this when they mean X, but sometimes they mean the narrower Y"), a confidence level, and at least one source reference. The cardinal rule, enforced in CI (see `../docs/ci-spec.md`): **no entry without a source**. An entry that cites nothing is a rumor stored in the moat.

**Links** are typed relationships between a source and an entry, or between entries. This is where most homegrown knowledge bases go wrong: they store only agreement. Evidence that complicates or contradicts an entry gets silently dropped, and the knowledge base becomes a collection of things that felt true, with no record of the arguments against them. So links carry a type:

- **Supports**: this source gives evidence for the entry as stated.
- **Complicates**: this source neither confirms nor denies, but adds a condition, a subpopulation, or a context where the entry reads differently. Most real evidence is this.
- **Contradicts**: this source gives evidence against the entry as stated.

A Contradicts link does not delete an entry. It sits on the record, visible, and factors into whether the entry can climb the confidence ladder. Two entries can also link to each other (e.g. "narrower-than", "commonly-confused-with"), but the source-to-entry link types above are the load-bearing ones.

## The confidence ladder

Every entry carries a confidence level from a small fixed vocabulary. The worked example uses three rungs:

- **provisional**: one source, or sources of low-reliability types only. The entry exists, is searchable internally, but is flagged and does not ship in user-facing output.
- **supported**: multiple independent sources, at least one from a higher-reliability type, and no unresolved Contradicts link.
- **validated**: confirmed against the authoritative source itself or by a qualified expert review, documented in a source record like any other evidence.

Three rules make the ladder real rather than decorative:

1. **Promotion is an event, not a mood.** An entry moves up when it meets the written criteria, and the promotion is a reviewable change (in a flat-file knowledge base, a diff) citing the links that justify it.
2. **Contradicts blocks promotion.** An entry with an unresolved Contradicts link cannot climb, no matter how many Supports links it collects. Resolving means a note explaining why the contradiction does not hold, or a rewrite of the entry to accommodate it.
3. **Confidence gates usage.** Somewhere downstream, a product behavior depends on the level: in the worked example, only `supported` and above ship to users. This is what gives the ladder teeth; if all rungs get treated the same, the ladder is decoration and will rot.

Adapt the number of rungs to your domain, but keep the shape: defined criteria per rung, promotion as a reviewed change, and at least one real consequence attached to the levels.

## Scaling: index plus small files

Store the knowledge base as one small file per entry plus an index that describes what exists, rather than as a few large files that grow forever. A monolithic file has to be read in full to find one entry, and with an AI collaborator that cost is paid in context window on every lookup: a five-hundred-entry file bills you its entire length to answer a one-entry question. With an index, a session reads the index, then pulls only the entries it needs, and the cost of a lookup stays flat no matter how large the collection grows.

Two rules make the pattern stick. The index is created when the collection is created, not retroactively once the folder feels big. And the index describes rather than duplicates: one line per entry with its identifier, gist, and confidence level, nothing more. The index also hands CI another cheap fail-closed target: an entry file missing from the index, or an index line pointing at a file that does not exist, fails the build (see `../docs/ci-spec.md`).

## Why this much structure

Because an AI collaborator makes knowledge-base corruption fast and pleasant. An AI ingesting a source will cheerfully produce fifty well-formatted entries in a minute, each looking exactly as authoritative as a hand-verified one. Without mandatory provenance, typed links, and confidence gating, a month of that produces something that looks like a moat and is actually a landfill with good formatting. The structure is what lets you use the AI's speed for ingestion while keeping the judgment human and auditable.

The companion rule: decide how each source type is handled *before* ingesting anything from it. That is `source-profiles.md`.

---
*Ballast v0.1*
