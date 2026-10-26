---
name: claim-check
description: How a marketing claim earns its place in a draft, what to write when no source exists, and how the claim list reaches the reviewer.
---

# Claim check in an open-source Kortix project

Kortix is open source, and this skill decides which claims survive a draft here. A claim is any sentence a reader could ask you to prove: a number, a price, a limit, a release date, a comparison, a superlative, or a statement about what a product does.

## The four rules

**1. A number carries its source and the date it was read.** Write the fact, then the page it came from, then `read 2026-10-26`. A number with no date is a number nobody can audit, because the page may have changed the week after.

**2. A superlative needs a measured comparison in the same paragraph.** "Fastest" or "largest" or "cheapest" is only usable next to the measurement that shows it: what was compared, on which date, from which page. If the comparison is not there, the sentence loses the superlative and keeps the fact.

**3. No source means `[NO SOURCE]`, and the draft stops there.** Put the marker where the sentence would go and say so in the change request. Do not write around the gap, do not hedge the sentence into something weaker, and do not delete it quietly. A reviewer needs to see which sentence the team could not support, because that is the sentence that gets somebody in trouble later.

**4. A competitor's price, limit or plan is re-read from that vendor's own page before reuse.** Open that page yourself: a search result, a summary, a roundup and last month's draft are all second hand. Read the number, and write today's date beside it. If the page has moved or the number changed, the line is rewritten rather than carried over.

## What counts as a source

A page you opened and read, at a URL the reader can open too. Its own words, not a description of them.

Not a source: a search result snippet, a model's memory of a page, another company's blog post about the vendor, a screenshot without a URL, or a document the reader cannot reach.

## The claim list

Every draft arrives with a claim list beside it, in the same change request, at `docs/claims/<asset-slug>.md`. `docs/claims/acme-november-webinar.md` is a worked example:

| Claim | Source | Read on |
| --- | --- | --- |
| One sentence, as it appears in the draft | The page it came from | 2026-10-26 |

The list is what makes the review a diff. A reviewer reads the draft and the list together, and each row says where to check.

## Markers

- `[NO SOURCE]`: no page supports this sentence. The sentence does not ship without one.
- `[STALE: what changed]`: the source moved or changed after the sentence was written. The number is out of date and the line is rewritten by a person.

Both markers are visible in the change request. Neither is a note to be cleaned up before review.

## What to do when the brief supplies a fact

A brief that names a price, a plan or a capability is a pointer, not a source. Open the page it points at, read the sentence, and copy the wording from there. A fact that arrives second hand is the one that turns out a version behind.
