# Changelog

Changes to the partner documentation in this repository. For the exact edits to any file, open the file and click **History**.

## 2026-11-10

### API documentation

- Added `updatedAt` to the Club, Club Team, Profile, Membership, Credential, Waiver, Suspension, and Club Transfer property tables. The API already returns this field: a non-null UTC timestamp of when the record last changed. It is not returned on eligibility responses.

## 2026-11-05

### Repository

- Added a README with guidance on which file to use, how to get access, and how to test.

### API documentation

- Added `GET /api/v1/regions`.
- Added filters:
  - Clubs: `regionCode`, `code`, `name`
  - Teams: `regionCode`, `clubCode`, `teamCode`, `name`
  - Profiles: `memberId`, `regionCode`
  - Memberships: `memberId`, `regionCode`
- Added filter behavior: case handling, partial name matches, empty results, and error responses.
- Added the Sandbox and Production addresses.
- Updated access and API key sections for the Developer Portal, including National key access.
- Webhooks section now points to `webhooks.md`.
- Corrected to match the API:
  - `memberId` and `memberIds` values are strings.
  - Profiles filter: `graduationYear` replaces `graduationYear[eq]`.
  - Team `gender` value is `Co-Ed`. Team `ageDefinition` runs `8U` through `18U`.
  - Team member `role` values are `athlete`, `headCoach`, and `staff`.
  - Membership tier `windowType` value is `Days`, not `FixedDuration`.
  - Credential `status` values are `NotStarted`, `InProgress`, `Completed`, `Cancelled`, `Submitted`, `Failed`, and `Inactive`. `Expired` is not a status.
  - Eligibility filters: `eligibility` is true or false, and `clubAssignmentStatus` adds `REJECTED`.
  - Membership role `status` values are `Active`, `Removed`, `Unpaid`, and `Upgraded`.
  - Credential `type` adds `NCSI`.
  - `expand` is available only on `GET /api/v1/teams/{id}`.
  - `division` is still returned as a display label.
- Added fields: Profile `preferredName`; Team member `jerseyNumber`, `positions`, `frozen`, and `frozenAt`.
- Corrected example values for Region and Club code.
- Removed two resolved open questions: Team value lists and key rotation cadence.

### Webhooks

- Subscriptions are now managed in the Developer Portal.

## 2026-11-04

### Repository

- Added this changelog.
- Added sample data: `sample-data/team-colorado-test-members.csv`.
- Moved older documents to `archive/`.
- Renamed `api-documentation-v2.1.md` to `api-documentation.md`. Git now tracks versions instead of file names. This documents API version 2.1.

### Field dictionary

- Added the filters for each endpoint to the Markdown dictionary, listed before its response fields. Added a contents table with links to both.
- The CSV has a new `Kind` column at the end that separates response fields from filters.
- Added fields: Profile `preferredName`; Team member `jerseyNumber`, `positions`, `frozen`, and `frozenAt`.
- Added allowed values for Team member `role` and `Inactive` for credential `status`.

## Earlier history

| Date | Change |
|---|---|
| 2026-10-13 | Added the MMS Partner API field dictionary |
| 2026-07-14 | Added the eligibility data dictionary working draft |
| 2026-06-12 | Standardized Team Code fields in API v2.1 |
| 2026-06-08 | Added the webhook contract |
| 2026-05-17 | Added the v2.1 partner field validation draft |
| 2026-04-28 | Added API v2.1 documentation |
| 2026-04-01 | Added API v2.0 documentation with the Eligibility API |
| 2025-12-03 | First API documentation |
