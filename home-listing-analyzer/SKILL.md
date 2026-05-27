---
name: home-listing-analyzer
description: Manage markdown records for residential home-for-purchase listings in a buyer's home search. Use this skill whenever the user wants to import a listing URL or pasted listing content, update an existing listing, add initial impressions, post-tour notes, realtor/advisor feedback, compare listings, score listings against a buyer profile, report on active listings, or identify buyer-profile signals from listing feedback. This skill depends on the home buyer profile; if no buyer profile exists, use the buyer profile workflow first.
---

# Home Listing Analyzer

Use this skill to manage residential home-for-purchase listing records under `~/home-search/listings/`.

The listing analyzer is a durable memory and analysis workflow, not a scraper. It should preserve source evidence, current listing facts, buyer/realtor feedback, status history, unknowns, fit analysis, and proposed buyer profile updates.

## Buyer Profile Dependency

Before importing, updating, scoring, comparing, or reporting on listings, check for:

```text
~/home-search/buyer-profile.md
```

When no buyer profile exists, stop and defer to the `home-buyer-profile` workflow first. Listings are interpreted through the buyer profile, so the profile is required foundation for this skill.

## Scope

Only import residential homes for purchase.

In scope:

- detached single-family homes
- condos
- townhomes
- manufactured homes when the buyer profile allows them
- multi-family only when owner-occupant intent is clear or the user explicitly confirms it

Out of scope:

- rentals
- apartment communities
- land-only listings
- commercial property
- pure investment properties
- records without a residential home

Refuse out-of-scope imports plainly:

```text
This appears to be a rental apartment community, not a home-for-purchase listing. This skill is scoped to buying residential homes, so I won't import it.
```

## Hard Stops

Do not create or update listing files when a hard stop applies.

Hard stops:

- no confirmed street address is available
- source appears to be a rental, apartment community, land-only listing, commercial property, or investment-only property
- multi-unit source does not identify which unit or owner-occupant record should be tracked
- URL cannot be accessed and the user has not pasted source content
- same address already exists and it is unclear whether this is an update or a different unit

If the address is missing, ask the user to provide or confirm the full street address. Do not create unknown-address, unknown address, placeholder-address, or partial-address files.

## Workspace And Filenames

Default directory:

```text
~/home-search/listings/
```

Each imported listing has two files:

```text
YYYY-MM-DD-<street-number>-<street-name>-<city-or-area>.md
YYYY-MM-DD-<street-number>-<street-name>-<city-or-area>-source.md
```

The date is the import/add date, not the market list date.

Examples:

```text
2026-05-23-10249-n-tyler-ave-portland.md
2026-05-23-10249-n-tyler-ave-portland-source.md
2026-05-23-123-main-st-unit-4-portland.md
```

Filename rules:

- include the street number
- include unit number when applicable
- include city or area
- use lowercase words separated by hyphens
- do not create unknown-address fallbacks

## Duplicate And Update Policy

Before creating a listing, search existing listing files for the same address.

Default behavior:

- same address and same unit: update the existing listing
- same address but different unit: ask whether to create a separate listing
- same address but uncertain identity: ask before writing

For updates, keep current facts current and preserve history in `Listing Events` and `Activity Log`.

## Import Workflow

1. Confirm the buyer profile exists.
2. Gather source content from URL, pasted listing text, MLS-style report, or user-provided facts.
3. Determine whether the source is in scope.
4. Confirm a full street address.
5. Search for duplicate existing listings.
6. Create or update the main listing file.
7. Create or append the companion source file when source content is provided.
8. Add an `Imported` or `Updated From Source` activity log entry.
9. Extract facts with confidence labels when useful.
10. Score the listing against the buyer profile when enough profile context exists.
11. Create unknowns, verification items, and questions for agent/seller.

If a URL is inaccessible, ask the user to paste the listing content. Do not pretend the page was read.

## Main Listing File

Use this structure for the main listing file. Keep sections stable even when sparse.

```markdown
# [Address]

Source File: [filename-source.md]

## Listing Facts

Address: Market Status: Buyer Status: Price: Beds: Baths: Sq Ft: Lot Size: Year Built: Property Type: Style: Garage/Parking: HOA: Property Tax: MLS Number: List Date: Last Source Update:

## Source Import Notes

Source URL: Source Type: Imported From: Extraction Confidence: Fields Needing Confirmation:

## Rooms And Layout

## Financial And Terms

## Fit Assessment

Fit Score: Confidence: Recommendation:

## Scorecard

| Category           | Score | Notes |
| ------------------ | ----: | ----- |
| Location           |       |       |
| Budget Fit         |       |       |
| Home Size / Layout |       |       |
| Condition / Risk   |       |       |
| Lifestyle Fit      |       |       |
| Dealbreakers       |       |       |
| Confidence         |       |       |

## Pros

## Cons

## Unknowns And Verification

## Questions For Agent/Seller

## Profile Signals

## Proposed Buyer Profile Updates

## Advisor Feedback

## Activity Log

## Listing Events
```

Sparse or missing source data should usually lower `Confidence` and create verification questions before lowering the fit score. Known risks, dealbreaker violations, or strong buyer/realtor concerns can lower the score directly.

## Source File

Create a companion `-source.md` file for imports and source-content updates.

The source file preserves minimally formatted source content. It should be readable markdown, but it should not contain analysis, scoring, or buyer interpretation.

Use:

```markdown
# Source Content: [Address]

Listing File: [filename.md]

## Source Snapshot: [timestamp]

Source URL: Source Type: Captured At: Access Result:

## Normalized Source Text

[Minimally formatted source content]
```

When new source content arrives for an existing listing, append a new `## Source Snapshot: ...` section to the same source file. Do not overwrite previous snapshots.

For HTML or dense raw text, clean just enough to make valid readable markdown:

- preserve headings when obvious
- add line breaks between distinct fields
- convert obvious field/value pairs to readable lines
- preserve listing descriptions
- do not add analysis to the source file

## Status Fields

Use two independent status fields.

`Market Status` describes the property's public market lifecycle.

Allowed purchase statuses:

- Coming Soon
- Active
- Active Under Contract
- Contingent
- Pending
- Sold
- Off Market
- Withdrawn
- Expired
- Canceled
- Unknown

`Buyer Status` describes the buyer's posture toward the listing.

Allowed values:

- New
- Needs Review
- Interested
- Paused
- Tour Candidate
- Tour Scheduled
- Toured
- Follow-Up Needed
- Offer Candidate
- Rejected
- Archived

Use `Listing Events` to preserve status history:

```markdown
## Listing Events

- 2026-05-23 16:03 America/Los_Angeles: Price changed from $479,000 to $459,000.
- 2026-05-23 16:03 America/Los_Angeles: Market Status changed from Active to Pending.
```

## Updating Existing Listings

Support direct update requests such as:

- "Update 10249 N Tyler Ave; price dropped to $459k."
- "Mark Tyler Ave as Pending."
- "Add the new RMLS text to this listing."
- "The open house is Sunday."
- "Mark this rejected because the bedrooms are too small."

For each update:

1. Identify the listing by address, filename, or unambiguous user shorthand.
2. Update current facts in `Listing Facts`.
3. Add durable changes to `Listing Events`.
4. Add source content to the source file if the user provided new listing/source text.
5. Revisit fit score, confidence, unknowns, and questions when the update affects analysis.

If the user clearly says to change `Buyer Status`, update it directly. If the status implication is inferred, suggest it instead.

## Feedback Workflow

Support feedback at any time:

- initial impressions
- post-tour notes
- rejection reasons
- reconsideration notes
- agent/seller follow-up
- general listing notes

When the user starts providing feedback, gather all feedback for that interaction. Ask "Anything else before I save this feedback?" when the user appears to be in note-taking mode. Once the user is done, write one timestamped entry to `Activity Log`, update listing interpretation, and then offer any buyer-profile updates.

Preserve raw feedback verbatim or near-verbatim. Keep interpretation separate.

```markdown
## Activity Log

### 2026-05-23 16:20 America/Los_Angeles - Initial Impression

Raw feedback:

> Cute remodel and I like the one-level layout, but I am nervous it is a flip. The bedrooms may be too small.

Interpretation:

- Remodel quality and permits may matter for confidence.
- Bedroom usability may matter more than listed bedroom count.
```

The first activity log entry is the durable record of when the listing was added. Do not duplicate it with an `Added At` field.

## Advisor Feedback

Use `Advisor Feedback` for realtor or trusted advisor observations. Preserve the advisor's words separately from interpretation.

```markdown
## Advisor Feedback

### 2026-05-23 15:12 America/Los_Angeles - Realtor - Foundation / Near-Term Repairs Concern

Raw feedback:

> Lovely house but I feel pretty confident the foundation will rule it out. There are other things to note that could either be immediate or within a short time that would need to be addressed.

Interpretation:

- Significant structural risk.
- Possible near-term repair burden.
- May conflict with low-maintenance preference.

Impact:

- Affected categories: Condition / Risk, Budget Fit, Confidence.
- Buyer Status may become Follow-Up Needed or Rejected depending on buyer risk tolerance.

Follow-up:

- Ask what visible signs led to the foundation concern.
- Review disclosures for foundation, drainage, and structural repairs.
```

Advisor feedback may carry strong signal, but it is evidence, not automatically confirmed fact.

## Fit Assessment

Use a transparent scorecard:

```markdown
Fit Score: 78 / 100 Confidence: Medium Recommendation: Tour Candidate
```

Default scorecard categories:

- Location
- Budget Fit
- Home Size / Layout
- Condition / Risk
- Lifestyle Fit
- Dealbreakers
- Confidence

Use recommendations such as:

- Needs Review
- Interested
- Paused
- Tour Candidate
- Follow-Up Needed
- Offer Candidate
- Rejected

Do not make sparse listings look certain. If the buyer profile is early-stage, financially uncertain, or missing key criteria, lower confidence and say what would improve confidence.

## Unknowns And Questions

Use `Unknowns And Verification` for source gaps, contradictions, and things to check on tour, in disclosures, or during inspection.

Use `Questions For Agent/Seller` for actionable follow-up:

```markdown
## Questions For Agent/Seller

- Were permits pulled for the remodel?
- What are the ages of the roof, electrical panel, plumbing, and HVAC?
- Is there cooling, or only forced-air heat?
```

Preserve disagreement. If source text says "turnkey" but advisor feedback raises foundation risk, keep both and ask verification questions.

## Proposed Buyer Profile Updates

After feedback is saved, offer profile updates when there is a plausible profile-level signal. Do not apply them without user approval.

Use exactly this heading and line style:

```markdown
## Proposed Buyer Profile Updates

1. Add "functional bedroom layout matters more than listed bedroom count." Evidence: Realtor feedback noted a nominal bedroom located at the entrance with French doors. Section: Strong Preferences
```

If the user approves updates, pass the approved changes to the `home-buyer-profile` workflow. If the user declines a recurring signal, suggest recording it under `Rejected Signals`.

## Reports And Comparisons

Do not create or maintain a listing index file. Do not create `listing-index.md`. Reports are generated in chat from the saved listing files.

Supported chat reports:

- report on all active listings
- report on buyer-active listings
- top matches
- tour candidates
- listings needing follow-up
- rejected listings and reasons
- profile signals across listings
- compare selected listings
- changes since last report, when activity logs support it

When the user says "active listings," default to listings where `Market Status` is `Active`, `Contingent`, `Active Under Contract`, or `Unknown`, and `Buyer Status` is not `Rejected` or `Archived`. State the filter used.

Example:

```text
I am treating active listings as homes where Market Status is Active, Contingent, Active Under Contract, or Unknown, and Buyer Status is not Rejected or Archived.
```

## Write And Confirmation Rules

Write directly without extra confirmation when the user clearly requests:

- import a new valid listing
- append a source snapshot
- add feedback after the user is done giving feedback
- update facts for an existing listing
- apply a user-explicit status change
- apply a user-approved buyer profile update through the buyer profile workflow

Confirm before writing when:

- an existing listing may already exist
- the same address may refer to a different unit
- the address cannot be confirmed
- the source appears out of scope
- no buyer profile exists
- a profile update is inferred from feedback
- an action would replace or remove durable content

## Completion Response

After importing or updating, briefly tell the user:

- which listing was created or updated
- whether a source snapshot was created or appended
- the current `Market Status` and `Buyer Status`
- the recommendation and confidence, if scored
- any critical unknowns or proposed buyer profile updates

Do not surface storage paths in refusal messages unless the user asks where files go.
