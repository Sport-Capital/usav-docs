# USAV API v2.1: Partner Field Validation

Date prepared: 2026-05-14
Status: Partner validation draft

These definitions are proposed for v2.1 partner validation. They are intended to confirm the launch field structure before the v2.1 API specification is updated.

## Purpose

Define the specific operational fields and generated `legacyTeamCode` that will be added to the v2.1 API to support partner event workflows, team pairing, system matching, and roster pulls.

## Context

The authoritative system identifier for Teams, Clubs, Regions, and related API objects is, and will remain, the UUID.

To support existing partner workflows, USAV will also expose a generated compatibility string alongside the standard UUIDs. This document outlines the proposed launch structure for that compatibility string. Once verified by the working group, these definitions will be used to update the v2.1 API specification. Other structural API changes will be handled separately as documentation updates or future enhancement requests.

## 1. The `legacyTeamCode` Solution

To support legacy team syncing, pairing, and result routing, the v2.1 API will return a generated `legacyTeamCode` string on Team responses.

Structure:

`genderCode` + `ageCode` + `clubCode` + `teamRank` + `regionCode`

Format example:

`G` + `08` + `VBONE` + `01` + `SO` = `G08VBONE01SO`

The `legacyTeamCode` is a convenience string for operational matching and partner compatibility. It is not the authoritative unique identifier. API calls that require a Team identifier should use the Team UUID once known.

## 2. Component Field Definitions

The API will return the following field lengths and formats to support `legacyTeamCode`.

| Component | Length | USAV API Definition | Working Values / Formatting |
|---|---:|---|---|
| `genderCode` | 1 | Division/gender classification used in the legacy team code. | `G` = girls, `B` = boys, `W` = women, `M` = men, `C` = coed. |
| `ageCode` | 2 | Numeric age segment used in the legacy team code. | Two digits, dropping the `U`; for example, `8U` becomes `08`. |
| `clubCode` | 5 | Partner-facing Club Code. | Five-character string. |
| `teamRank` | 2 | Sequential team rank within the club division. | Two-digit string; for example, `01`. |
| `regionCode` | 2 | Partner-facing Region Code. | Two-character string. |

## 3. Roster Pull Clarification

For v2.1 integration, a "team roster pull" is fulfilled by retrieving the `members[]` array using the `expand` parameter on the Team endpoint:

`GET /api/v1/teams/{id}?expand=members`

Workflows regarding event rosters, frozen rosters, and roster-specific webhooks are outside the scope of this v2.1 baseline clarification and will be reviewed separately as future enhancements unless USAV decides to pull them into launch scope.

## 4. Required Verification

To finalize the v2.1 specification, please confirm the following technical details by **[Insert Date - End of Next Week]**:

1. Does the proposed `legacyTeamCode` construction technically fulfill the requirement for team pairing, syncing, and result routing?

   `genderCode + ageCode + clubCode + teamRank + regionCode`

2. Are the defined lengths and formats for `ageCode` and `genderCode` accurate for your current ingestion logic?

   `ageCode`: two digits, dropping the `U`; for example, `8U` becomes `08`.

   `genderCode`: `G`, `B`, `W`, `M`, `C`.

3. Are the defined lengths for `clubCode` (5) and `regionCode` (2) accurate to your current ingestion logic?

4. What rule should determine junior vs adult classification for `genderCode`?

## 5. Acknowledgement Of Historical GitHub & Slack Requests

We have reviewed the historical API feedback provided in late 2025 and early 2026 via Slack and GitHub, including:

- Issue #1: Region Code length
- Issue #2: Club Code length
- Issue #3: string field types and lengths
- Issue #4: non-rostered club members
- Issue #5: credentials that do not pertain to membership
- Issue #7: updated-since timestamp filtering
- Issue #8: credential and eligibility requirement definitions

This document is strictly scoped to finalizing foundational identifiers and Team Code definitions needed to unblock v2.1 event pairing and partner matching.

The other historical requests, including incremental sync, comprehensive string-length dictionaries for all properties, expanded credential/eligibility scope definitions, event roster workflows, frozen roster workflows, and roster-specific webhooks, are recognized and will be tracked separately as documentation updates or future enhancement requests.
