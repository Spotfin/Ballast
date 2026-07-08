# Source profiles: decide handling before ingesting

## The pattern

For every *type* of source that can feed the knowledge base, write a profile **before the first item of that type is named, stored, or ingested**. The profile decides, in advance: what may be extracted, what must never be stored, how the source is referenced, what confidence its evidence can support, and what the extraction steps are.

The ordering is the whole point. Once a specific source is in hand ("this forum thread is gold, let me just grab it"), every handling question gets answered under enthusiasm, per-item, inconsistently. Profiles answer the questions once, per type, coldly. Ingestion then becomes mechanical: identify the type, apply the profile, no per-item judgment about the rules themselves.

This is also where sensitive-data handling lives. A knowledge base built partly from human-generated material (forum posts, interviews, user research) is one careless paste away from storing something you must not hold: a name, an identifying detail, a verbatim quote with personal context. The profile is where "we never store that" becomes a concrete, checkable extraction rule rather than a vibe.

## What a profile contains

- **Source type**: what kind of material this covers.
- **Reliability class**: what the type can support on the confidence ladder (see `README.md`).
- **Extract**: exactly what fields may be pulled out.
- **Never store**: what must be stripped or refused, stated concretely.
- **Reference format**: how a source record cites this type without storing the forbidden parts.
- **Steps**: the extraction procedure, short enough to actually follow.

## Worked example profiles

From the fictional rulebook-navigator product, whose knowledge base maps informal phrasing to official terminology.

### Profile: public forum thread

- **Reliability class:** low. Evidence from this type alone caps an entry at `provisional`.
- **Extract:** informal phrasings people actually use; the apparent intended meaning; recurring points of confusion between terms.
- **Never store:** usernames or handles; verbatim sentences longer than the phrase itself; any personal narrative or identifying context surrounding the phrase; links to specific posts by specific users. Store the *phrasing pattern*, not the person's sentence.
- **Reference format:** forum name, thread topic paraphrased, month and year, number of distinct participants observed using the phrasing. No URLs to individual posts.
- **Steps:** read the whole thread before extracting anything; list candidate phrasings; strip each to the minimal reusable phrase; check each against Never-store; create or update entries with Supports/Complicates links; log the source record.

### Profile: published study or official guidance document

- **Reliability class:** high. Can support promotion to `supported`, and official documents can support `validated`.
- **Extract:** official terminology and definitions; documented usage distinctions; findings about how non-experts misread specific terms.
- **Never store:** nothing categorical, but respect quoting limits: store findings in the entry's own words with a citation, not wholesale reproduced text.
- **Reference format:** full citation, section or page for each linked claim.
- **Steps:** cite first, extract second (create the source record before any entry edits, so nothing gets in ahead of its provenance); link each affected entry individually rather than batch-linking the whole document to everything it vaguely touches.

### Profile: practitioner interview

- **Reliability class:** medium. Supports `supported` when combined with an independent source of any type.
- **Extract:** terminology distinctions practitioners draw; phrases they hear from non-experts; corrections of entries shown to them.
- **Never store:** the practitioner's name or employer in the entry data (consent for a source record is separate and explicit); anything they mention about specific clients or cases, at all, even anonymized. If a rule of thumb was illustrated with a case, store the rule, refuse the illustration.
- **Reference format:** role-and-context descriptor ("inspector, 15 years, large-city jurisdiction"), interview date, consent note.
- **Steps:** written consent covering the reference format before the interview; extraction from notes within 48 hours; the Never-store pass happens at note-taking time, not at ingestion time, so the raw notes are already clean.

### Profile: founder's own research notes

- **Reliability class:** low, deliberately. Your own notes feel authoritative and are the least checkable source in the system. Caps at `provisional` until corroborated by any independent source.
- **Extract:** hypotheses about phrasing, observed patterns, candidate entries.
- **Never store:** anything remembered from a private conversation that would fail the interview profile's rules; if you would strip it there, strip it here.
- **Reference format:** note date and context of observation.
- **Steps:** same template as everything else. The point of this profile existing is that "it's just my own notes" is not an exemption from the system.

## Notes on the pattern

- The last profile is the one people skip and should not. The founder's own knowledge is usually the first thing ingested and the least documented, and six months later nobody can tell which entries were verified and which were remembered.
- Profiles are versioned documents. When a profile changes, existing source records keep the profile version they were ingested under, so an audit can always reconstruct what rules applied.
- A new source type with no profile means ingestion stops until the profile is written. That rule belongs in your hard rules or your CLAUDE.md, stated exactly that bluntly, because the tempting moment ("just this one podcast") is precisely when it applies.

---
*Ballast v0.1*
