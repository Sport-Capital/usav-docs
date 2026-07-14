| API Field | Definition | Data Type | Length | Format / Allowed Values | Example | Status | Notes / Questions |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | USAV member number. | integer | Not applicable | Whole number | 12345678 | Current v2.1 | All eligibility results |
| profileId | UUID identifying the member's Profile across USAV API resources. | string | 36 characters | UUID | 550e8400-e29b-41d4-a716-446655440031 | Proposed addition | Not currently documented in the Eligibility response; field name and placement must be confirmed |
| memberFound | Whether the requested member exists. | boolean | Not applicable | true \| false | true | Current v2.1 | Single and batch checks |
| firstName | Member's first name. | string | Not specified | Text | Sarah | Current v2.1 |  |
| lastName | Member's last name. | string | Not specified | Text | Johnson | Current v2.1 |  |
| birthDate | Date of birth used for identity and eligibility context. | date | 10 characters | YYYY-MM-DD | 2008-05-15 | Current v2.1 | Current API uses full DOB; Daniel raised reduced-PII display as a question |
| graduationYear | Expected high-school graduation year, when available. | integer (nullable) | Not applicable | 4-digit year or null | 2026 | Proposed addition | Daniel requested this in club eligibility results; response path must be confirmed |
| birthGender | Birth gender used for eligibility rules. | string | Not specified | Proposed: Male \| Female | Female | Proposed addition | Daniel requested this value; current Profile field is gender, so the final field name must be confirmed |
| windowStart | First date evaluated. | date | 10 characters | YYYY-MM-DD | 2026-09-15 | Current v2.1 | Defaults to today |
| windowEnd | Last date evaluated. | date | 10 characters | YYYY-MM-DD | 2026-09-17 | Current v2.1 | All conditions must pass throughout the window |
| roles[] | Roles evaluated for the member. | array | Variable | Zero or more records | player, coach | Current v2.1 |  |
| roles[].role | Code for the evaluated role. | string | Not specified | Eligibility Roles value | player | Current v2.1 | Full role list is still being finalized |
| roles[].roleName | Display name for the role. | string | Not specified | Text | Indoor Player | Proposed addition | Daniel requested role name; field name and response path must be confirmed |
| roles[].tierName | Membership tier associated with the role. | string | Not specified | Text | Junior Full Season | Proposed addition | Daniel requested tier name; field name and response path must be confirmed |
| roles[].eligible | Whether every condition passed for the full window. | boolean | Not applicable | true \| false | true | Current v2.1 |  |
| roles[].conditions[] | Checks used to calculate eligibility. | array | Variable | Condition records |  | Current v2.1 | Seven conditions in current implementation |
| roles[].conditions[].condition | Plain-language condition name. | string | Not specified | Reference Values list | Membership status is Paid | Current v2.1 |  |
| roles[].conditions[].passed | Whether the condition passed for the full window. | boolean | Not applicable | true \| false | true | Current v2.1 |  |
| roles[].conditions[].details | Dates or reasons supporting the result. | object (nullable) | Variable | Varies by condition | { status: Paid } | Current v2.1 |  |
| details.membership.startDate | Date membership becomes active. | date | 10 characters | YYYY-MM-DD | 2026-09-01 | Current v2.1 | Explains future membership failure |
| details.endDate | Relevant end or expiration date. | date (nullable) | 10 characters | YYYY-MM-DD or null | 2026-08-31 | Current v2.1 | Meaning depends on condition |
| details.status | Status supporting the result. | string | Not specified | Condition-specific value | Paid | Current v2.1 |  |
| details.credentials[] | Credentials evaluated by the credential condition. | array | Variable | Credential summaries |  | Current v2.1 | Assigned credentials are currently treated as required; confirmation pending |
| details.credentials[].name | Credential name. | string | Not specified | Text | SafeSport Training | Current v2.1 |  |
| details.credentials[].status | Credential completion status. | string | Not specified | NotStarted \| InProgress \| Completed \| Expired \| Failed | Completed | Current v2.1 |  |
| details.credentials[].validUntil | Date credential remains valid through. | date (nullable) | 10 characters | YYYY-MM-DD or null | 2026-08-10 | Current v2.1 |  |
| details.requiredDueTo | Reason a DOB-driven requirement applies. | string (nullable) | Not specified | Reason code | age_threshold_18 | Current v2.1 | Populates when source logic supplies it |
| details.requirementActivatedOn | Date a DOB-driven requirement activated. | date (nullable) | 10 characters | YYYY-MM-DD or null | 2026-05-15 | Current v2.1 |  |
| scope.type | Group evaluated. | string | Not specified | club \| team \| region | club | Current v2.1 | Scope endpoints |
| scope.clubId | UUID for the evaluated Club. | string | 36 characters | UUID | 550e8400-e29b-41d4-a716-446655440000 | Current v2.1 | Club and Team scope responses |
| scope.teamId | UUID for the evaluated Team. | string | 36 characters | UUID | 550e8400-e29b-41d4-a716-446655440010 | Current v2.1 | Team scope response |
| scope.regionId | UUID for the evaluated Region. | string | 36 characters | UUID | 550e8400-e29b-41d4-a716-446655440001 | Current v2.1 | Region scope response |
| scope.clubName / teamName / regionName | Display name for the evaluated group. | string | Not specified | Text | Riverside Volleyball Club | Current v2.1 | Field name depends on scope |
| scope.clubCode / regionCode | Code returned for the applicable scope. | string | Not specified | Current API value | RVC | Current v2.1 | Not necessarily the pending Team Code component format |
| pagination.page | Current page. | integer | Not applicable | 1 or greater | 1 | Current v2.1 | Scope endpoints |
| pagination.limit | Records per page. | integer | Not applicable | 1-200 | 50 | Current v2.1 | Scope endpoints |
| pagination.total | Total matching records. | integer | Not applicable | 0 or greater | 245 | Current v2.1 | Scope endpoints |
| pagination.totalPages | Total result pages. | integer | Not applicable | 0 or greater | 5 | Current v2.1 | Scope endpoints |
| pagination.hasNextPage / hasPreviousPage | Whether a later or earlier page exists. | boolean | Not applicable | true \| false | true | Current v2.1 | Scope endpoints |
| teamCode | Operational code generated from Team components. | string | 13-14 characters (pending) | genderCode + ageGroup + playLevel + clubCode + teamRank + regionCode | FJ16VBONE01SO | Pending standard | Final uniqueness enforcement and component model remain unresolved |
| id | Official unique Team identifier. | string | 36 characters | UUID | 550e8400-e29b-41d4-a716-446655440010 | Current direction | Use when guaranteed uniqueness is required |
| genderCode | One-character gender/classification component. | string | 1 character | 1 character; working M \| F \| C | F | Pending standard | Confirm official values |
| ageGroup | Junior or adult classification. | string | 1 character | 1 character; working J \| A | J | Pending standard | Newer model replaces ageCode-only structure |
| playLevel | Junior age or adult play-level component. | string | 2 characters | 2 characters; junior 08-18; adult list pending | 16 | Pending standard | Daniel requested adult levels/codes: Open, AA, A, BB, B, Recreational, age divisions, Co-Ed, SONA |
| clubCode | Club component in Team Code. | string | 5 characters | 5 uppercase letters/numbers | VBONE | Pending standard | Working rule: unique within Region |
| teamRank | Rank within the applicable club classification. | string | 2 characters | 2 digits with leading zero | 01 (two digits) | Pending standard | Confirm uniqueness scope |
| regionCode | Region or foreign-country component. | string | 2 or 3 characters | 2-char USAV Region; proposed 3-char IOC foreign code | SO | Pending standard | Confirm foreign rule and final Team Code length |
