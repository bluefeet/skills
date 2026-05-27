---
name: home-buyer-profile
description: Build and maintain a durable markdown home buyer profile for someone buying a residential home to live in. Use this skill whenever the user wants to create, review, modify, replace, or refine a home buyer profile; define home search criteria; clarify budget readiness, locations, dealbreakers, preferences, or tradeoffs; or apply approved buyer-profile updates discovered from listing feedback. Prefer this skill before any buyer-centered listing scoring or comparison when no profile exists.
---

# Home Buyer Profile

Use this skill to create and maintain `~/home-search/buyer-profile.md`.

The profile is a durable decision model for a home buyer. It should capture what the buyer explicitly said, what the agent inferred, what remains uncertain, and what signals the buyer has rejected so those suggestions do not keep resurfacing.

## Scope

This skill is for buyers looking for a residential home to live in.

In scope:

- detached single-family homes
- condos
- townhomes
- manufactured homes if the buyer is open to them
- multi-family only when owner-occupant intent is explicit

Out of scope unless the user explicitly changes the project scope:

- rentals
- apartment communities
- land-only purchases
- commercial property
- pure investment-property searches

## File Behavior

Default file:

```text
~/home-search/buyer-profile.md
```

If the file does not exist, interview the user and write it.

If the file exists and the user asks to create a profile, ask whether they want to:

- review the existing profile
- modify it
- replace it

Do not replace an existing profile without explicit confirmation. Directly write ordinary modifications and user-approved updates once intent is clear.

## Interview Style

Start with high-level calibration before beds, baths, locations, and price. Calibration tells you whether the buyer needs a quick criteria capture or a more exploratory interview.

Ask one question at a time. Prefer multiple-choice questions with an `Other / freeform` option. Use the user's answer to choose the next most useful question. Do not dump the entire questionnaire at once.

Good question pattern:

```text
How would you describe the buyer's search confidence?

A. Very clear on what they want
B. Somewhat clear, still sorting tradeoffs
C. Early and exploratory
D. Other / freeform
```

If the user gives a freeform answer, translate it into profile-ready language and ask a focused follow-up when needed.

## Intake Calibration

Capture these early:

- Buying Experience: first-time, bought before but not recently, experienced
- Search Confidence: very clear, somewhat clear, still exploring
- Search Stage: just starting, browsing listings, actively touring, ready to offer
- Financing Stage: not started, rough estimate, lender conversation, pre-approved, cash/mostly cash
- Budget Confidence: low, medium, high
- Market Reality: comfortable match, stretch, unknown, likely mismatch
- Decision Support Needed: validate known criteria, discover priorities, keep realistic, compare tradeoffs, avoid risky choices
- Timeline Pressure: low, moderate, high, fixed deadline

Use calibration to adapt:

- Confident, experienced, pre-approved buyers need fewer basics and more tradeoff/dealbreaker precision.
- First-time or uncertain buyers need more discovery, more uncertainty capture, and less overconfident scoring language.
- Buyers with likely area/budget mismatch need aspiration areas, fallback areas, and explicit tradeoff questions.

## Financial Privacy Rule

Do not store income, debts, account balances, lender contact details, identity documents, exact financial account information, or similarly sensitive financial details in the profile.

It is okay to discuss sensitive financial details conversationally if the user brings them up. Before writing durable files, convert that discussion into non-sensitive search signals.

Allowed durable signals include:

- Financing Stage
- Budget Confidence
- Target Price
- Comfortable Price, if the user volunteers it and wants it stored
- Stretch Price
- Hard Ceiling
- HOA / Condo Fee Tolerance
- Property Tax Sensitivity
- Repair Budget Flexibility
- Cash Reserves: adequate, limited, unknown
- Affordability Unknowns

Example conversion:

```markdown
Budget Confidence: Medium Target Price: $725,000 Stretch Price: $775,000 Hard Ceiling: $825,000 Cash Reserves: Adequate Affordability Unknowns:

- Needs lender confirmation before treating ceiling as firm.
```

Do not write the income, debt, or account-balance details that led to those signals.

## Adaptive Question Bank

Use these topics as the standard coverage set. Ask only what is needed for a useful profile, and leave unknowns explicit.

Search setup:

- Target cities, neighborhoods, and avoid zones
- Commute anchors and acceptable commute patterns
- School, transit, walkability, and lifestyle needs
- Timeline and urgency

Home criteria:

- Home types: detached, condo, townhome, manufactured, owner-occupant multi-family
- Bedroom range and whether bedroom layout/function matters more than count
- Bathroom needs
- Square footage or "feels spacious enough" preferences
- Lot, yard, stairs, parking, garage, storage, and accessibility
- Work-from-home, guest, pet, hobby, or household-specific needs

Budget:

- Target Price
- Stretch Price
- Hard Ceiling
- HOA / Condo Fee Tolerance
- Tax sensitivity
- Repair or renovation budget flexibility
- Whether price expectations are lender-confirmed or speculative

Preferences:

- Dealbreakers
- Strong Preferences
- Nice To Haves
- Condition and risk tolerance
- Noise, light, privacy, street feel, safety, and neighborhood feel
- Remodel quality, permits, foundation, roof, systems, and inspection concerns

Wrap-up:

- Ask for any other feedback the user has to help define the buyer profile.
- Summarize what will be written before writing only if the user seems uncertain or the profile would replace existing content.

## Profile Template

Write `~/home-search/buyer-profile.md` using this structure. Keep sections even when sparse so later updates have a stable home.

```markdown
# Home Buyer Profile

## Search Calibration

Buying Experience: Search Confidence: Search Stage: Financing Stage: Budget Confidence: Market Reality: Decision Support Needed: Timeline Pressure:

## Financial Picture

Target Price: Comfortable Price: Stretch Price: Hard Ceiling: HOA / Condo Fee Tolerance: Property Tax Sensitivity: Repair Budget Flexibility: Cash Reserves: Affordability Unknowns:

## Target Locations

Preferred Areas: Fallback Areas: Avoid Areas: Commute Anchors: Location Notes:

## Home Types

Open To: Not Open To: Notes:

## Size And Layout

Bedrooms: Bathrooms: Square Footage: Layout Requirements: Accessibility / Stairs: Parking / Garage: Outdoor Space:

## Dealbreakers

## Strong Preferences

## Nice To Haves

## Lifestyle And Use

## Condition And Risk Tolerance

## Stated Preferences

## Interpreted Decision Rules

## Assumptions And Uncertainties

## Rejected Signals

## Open Notes

## Change Log
```

## Stated Versus Interpreted

Keep the provenance of profile content clear:

- `Stated Preferences`: what the user directly said.
- `Interpreted Decision Rules`: useful synthesis, such as "will trade cosmetic polish for better layout and location."
- `Assumptions And Uncertainties`: anything that should lower confidence in later listing scoring.
- `Rejected Signals`: suggestions the user declined, so they are not repeatedly proposed.

Do not present an inference as if the buyer explicitly said it.

## Applying Approved Profile Updates

Listing analysis may produce proposed updates like:

```markdown
## Proposed Buyer Profile Updates

1. Add "functional bedroom layout matters more than listed bedroom count." Evidence: Realtor feedback noted a nominal bedroom located at the entrance with French doors. Section: Strong Preferences
```

Apply only updates the user approves. If the user edits the wording, use the user's wording.

When applying updates:

1. Read the current profile.
2. Place each approved update in its named `Section:`.
3. Preserve supporting evidence when it will help future interpretation.
4. Add declined suggestions to `Rejected Signals` when they might otherwise recur.
5. Add a dated `Change Log` entry.

Example change log:

```markdown
## Change Log

- 2026-05-23 16:45 America/Los_Angeles: Added functional bedroom layout as a strong preference based on approved listing feedback from 10249 N Tyler Ave.
```

## Completion Response

After writing or updating the profile, briefly tell the user:

- where the profile was written
- what major sections were updated
- any important unknowns or next questions

Do not include sensitive financial details in the response unless the user just provided them and it is necessary to confirm conversational understanding.
