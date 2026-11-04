# Changelog

Changes to the partner documentation in this repository. For the exact edits to any file, open the file and click **History**.

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
