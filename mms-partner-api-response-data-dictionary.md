# MMS Partner API field dictionary

Date: October 13, 2026

Each endpoint lists its delivered record fields. The original field name is preserved, and Qualified name shows the field path when it is nested. Pagination is excluded.

Required applies when the containing record is returned. Nullable means the field can contain null. No maximum defined means the current API contract and MMS database do not enforce a maximum for that field. Maximum not specified means the available source did not provide a limit.

Definitions and constraints use the current API v2.1 documentation and the updated Sports Capital field-length file received October 13, 2026. The longest value observed in staging is not treated as a maximum length.

For the Clubs, Profiles, Teams and Memberships list endpoints, the tables use the supplied object schemas; their list-item shapes still need confirmation.

## GET /api/v1/clubs

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | name | Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| region | region | Related Region details. | Yes | Object | N/A | No | N/A |
| affiliationStatus | affiliationStatus | Current Club affiliation status. | Yes | String | Maximum 11 characters | No | `Active`, `Inactive`, `InProgress`, `Archived`, `Unpaid`, `Pending`, `NotApproved` |
| affiliationStatusUpdatedAt | affiliationStatusUpdatedAt | Date and time the Club affiliation status last changed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| type | type | Type or category of the club. | Yes | Array of strings | Each value: maximum 7 characters | No | `Indoor`, `Outdoor` |
| gender | gender | Gender value returned for the record. | Yes | Array of strings | Each value: maximum 6 characters | No | `Male`, `Female` |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| contact | contact | Primary Club contact details. | Yes | Object | N/A | Yes | N/A |
| location | location | Primary Club facility details. | Yes | Object | N/A | Yes | N/A |
| logoUrl | logoUrl | URL of the Club logo image. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| metadata | metadata | Additional metadata returned with the record. | Yes | Object | N/A | No | N/A |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Contact fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| name | contact.name | Contact name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| email | contact.email | Email address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| phone | contact.phone | Telephone number. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| role | contact.role | Contact person's role or title. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Location fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| name | location.name | Location name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| address | location.address | Address returned for the record. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | location.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | location.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | location.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | location.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## GET /api/v1/clubs/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | name | Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| region | region | Related Region details. | Yes | Object | N/A | No | N/A |
| affiliationStatus | affiliationStatus | Current Club affiliation status. | Yes | String | Maximum 11 characters | No | `Active`, `Inactive`, `InProgress`, `Archived`, `Unpaid`, `Pending`, `NotApproved` |
| affiliationStatusUpdatedAt | affiliationStatusUpdatedAt | Date and time the Club affiliation status last changed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| type | type | Type or category of the club. | Yes | Array of strings | Each value: maximum 7 characters | No | `Indoor`, `Outdoor` |
| gender | gender | Gender value returned for the record. | Yes | Array of strings | Each value: maximum 6 characters | No | `Male`, `Female` |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| contact | contact | Primary Club contact details. | Yes | Object | N/A | Yes | N/A |
| location | location | Primary Club facility details. | Yes | Object | N/A | Yes | N/A |
| logoUrl | logoUrl | URL of the Club logo image. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| metadata | metadata | Additional metadata returned with the record. | Yes | Object | N/A | No | N/A |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Contact fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| name | contact.name | Contact name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| email | contact.email | Email address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| phone | contact.phone | Telephone number. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| role | contact.role | Contact person's role or title. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Location fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| name | location.name | Location name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| address | location.address | Address returned for the record. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | location.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | location.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | location.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | location.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## GET /api/v1/regions

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | name | Region name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## GET /api/v1/profiles

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| middleName | middleName | Member middle name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| birthDate | birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes | `Male`, `Female` |
| genderIdentity | genderIdentity | Preferred display gender identity. | Yes | String | Maximum 10 characters | Yes | `Male`, `Female`, `Non-binary` |
| phoneNumber | phoneNumber | Primary contact telephone number for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| contactEmail | contactEmail | Primary contact email for the Profile. | Yes | String | No maximum defined | Yes | Not specified |
| status | status | Current status of the profile. | Yes | String | Maximum 8 characters | No | `active`, `inactive` |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| guardians | guardians | Guardian relationships associated with the Profile. | Yes | Array of objects | N/A | No | N/A |
| suspensions | suspensions | Suspensions associated with the Profile. | Yes | Array of objects | N/A | No | N/A |
| demographics | demographics | Demographic information associated with the Profile. | Yes | Object | N/A | No | N/A |
| preferences | preferences | Preferences and consent values associated with the Profile. | Yes | Object | N/A | No | N/A |
| education | education | Education information associated with the Profile. | Yes | Object | N/A | No | N/A |
| avatarUrl | avatarUrl | URL of the Profile avatar image. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Guardian fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| userId | guardians[].userId | Related user UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| email | guardians[].email | Email address. | Yes | String | No maximum defined | Yes | Not specified |
| isMainGuardian | guardians[].isMainGuardian | Whether this guardian is the primary guardian. | Yes | Boolean | N/A | No | N/A |

### Suspension fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | suspensions[].id | Suspension UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | suspensions[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | suspensions[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| scope | suspensions[].scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No | `global`, `role-specific` |
| affectedRoles | suspensions[].affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes | N/A |
| reason | suspensions[].reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| startDate | suspensions[].startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| endDate | suspensions[].endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| canceledAt | suspensions[].canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| createdAt | suspensions[].createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | suspensions[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Demographics fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ethnicity | demographics.ethnicity | Ethnicity value recorded for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| disability | demographics.disability | Whether the Profile records a disability indicator. | Yes | Boolean | N/A | Yes | N/A |
| militaryService | demographics.militaryService | Whether the Profile records military service. | Yes | Boolean | N/A | Yes | N/A |

### Preferences fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ncsaShareAgreed | preferences.ncsaShareAgreed | Whether the member agreed to NCSA data sharing. | Yes | Boolean | N/A | Yes | N/A |
| ncsaShareAgreedAt | preferences.ncsaShareAgreedAt | Date and time the NCSA sharing choice was recorded. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Education fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| graduationYear | education.graduationYear | High-school graduation year. | Yes | Integer | N/A | Yes | N/A |

## GET /api/v1/profiles/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| middleName | middleName | Member middle name. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| birthDate | birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes | `Male`, `Female` |
| genderIdentity | genderIdentity | Preferred display gender identity. | Yes | String | Maximum 10 characters | Yes | `Male`, `Female`, `Non-binary` |
| phoneNumber | phoneNumber | Primary contact telephone number for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| contactEmail | contactEmail | Primary contact email for the Profile. | Yes | String | No maximum defined | Yes | Not specified |
| status | status | Current status of the profile. | Yes | String | Maximum 8 characters | No | `active`, `inactive` |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| guardians | guardians | Guardian relationships associated with the Profile. | Yes | Array of objects | N/A | No | N/A |
| suspensions | suspensions | Suspensions associated with the Profile. | Yes | Array of objects | N/A | No | N/A |
| demographics | demographics | Demographic information associated with the Profile. | Yes | Object | N/A | No | N/A |
| preferences | preferences | Preferences and consent values associated with the Profile. | Yes | Object | N/A | No | N/A |
| education | education | Education information associated with the Profile. | Yes | Object | N/A | No | N/A |
| avatarUrl | avatarUrl | URL of the Profile avatar image. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Guardian fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| userId | guardians[].userId | Related user UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| email | guardians[].email | Email address. | Yes | String | No maximum defined | Yes | Not specified |
| isMainGuardian | guardians[].isMainGuardian | Whether this guardian is the primary guardian. | Yes | Boolean | N/A | No | N/A |

### Suspension fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | suspensions[].id | Suspension UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | suspensions[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | suspensions[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| scope | suspensions[].scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No | `global`, `role-specific` |
| affectedRoles | suspensions[].affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes | N/A |
| reason | suspensions[].reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| startDate | suspensions[].startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| endDate | suspensions[].endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| canceledAt | suspensions[].canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| createdAt | suspensions[].createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | suspensions[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Demographics fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ethnicity | demographics.ethnicity | Ethnicity value recorded for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| disability | demographics.disability | Whether the Profile records a disability indicator. | Yes | Boolean | N/A | Yes | N/A |
| militaryService | demographics.militaryService | Whether the Profile records military service. | Yes | Boolean | N/A | Yes | N/A |

### Preferences fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ncsaShareAgreed | preferences.ncsaShareAgreed | Whether the member agreed to NCSA data sharing. | Yes | Boolean | N/A | Yes | N/A |
| ncsaShareAgreedAt | preferences.ncsaShareAgreedAt | Date and time the NCSA sharing choice was recorded. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Education fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| graduationYear | education.graduationYear | High-school graduation year. | Yes | Integer | N/A | Yes | N/A |

## GET /api/v1/teams

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Team UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | name | Team name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| clubId | clubId | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| club | club | Related Club details. | No | Object | N/A | No | N/A |
| ageDefinition | ageDefinition | Junior age division assigned to the Team; may be null for adult Teams. | Yes | String | Maximum 3 characters | Yes | `8U`, `9U`, `10U`, `11U`, `12U`, `13U`, `14U`, `15U`, `16U`, `17U`, `18U` |
| gender | gender | Gender value returned for the record. | Yes | String | Maximum 6 characters | No | `Male`, `Female`, `Co-Ed` |
| division | division | Display label derived from the Team classification. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| type | type | Type or category of the team. | Yes | String | Maximum 6 characters | Yes | `Indoor`, `Beach`, `Grass` |
| status | status | Current status of the team. | Yes | String | Maximum 8 characters | No | `active`, `inactive` |
| teamCode | teamCode | Standardized operational Team Code generated from the Team Code components. | Yes | String | 13 or 14 characters | Yes | `genderCode + ageGroup + playLevel + clubCode + teamRank + regionCode`; unique across USAV; Team UUID remains authoritative |
| genderCode | genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes | `M`, `F`, `C` |
| ageGroup | ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes | `J`, `A` |
| playLevel | playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes | Two uppercase letters or digits; approved play-level values are not fully specified |
| clubCode | clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes | Five uppercase letters or digits; unique within Region |
| teamRank | teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes | Two digits, zero-padded (for example, `01`) |
| regionCode | regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes | Two-character USAV Region Code or three-character foreign code |
| teamCodeComponents | teamCodeComponents | Individual values used to construct Team Code. | Yes | Object | N/A | No | N/A |
| members | members | Team roster returned when member expansion is requested. | No | Array of objects | N/A | No | N/A |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | club.id | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | club.name | Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | club.code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Team Code Components fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genderCode | teamCodeComponents.genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes | `M`, `F`, `C` |
| ageGroup | teamCodeComponents.ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes | `J`, `A` |
| playLevel | teamCodeComponents.playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes | Two uppercase letters or digits; approved play-level values are not fully specified |
| clubCode | teamCodeComponents.clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes | Five uppercase letters or digits; unique within Region |
| teamRank | teamCodeComponents.teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes | Two digits, zero-padded (for example, `01`) |
| regionCode | teamCodeComponents.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes | Two-character USAV Region Code or three-character foreign code |

### Team Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| profileId | members[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profile | members[].profile | Related member Profile details. | Yes | Object | N/A | No | N/A |
| role | members[].role | Member role on the Team. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| joinedAt | members[].joinedAt | Date and time the member joined the Team. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Team Member Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | members[].profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | members[].profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | members[].profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | members[].profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | members[].profile.birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | members[].profile.gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes | `Male`, `Female` |

## GET /api/v1/teams/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Team UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | name | Team name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| clubId | clubId | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| club | club | Related Club details. | No | Object | N/A | No | N/A |
| ageDefinition | ageDefinition | Junior age division assigned to the Team; may be null for adult Teams. | Yes | String | Maximum 3 characters | Yes | `8U`, `9U`, `10U`, `11U`, `12U`, `13U`, `14U`, `15U`, `16U`, `17U`, `18U` |
| gender | gender | Gender value returned for the record. | Yes | String | Maximum 6 characters | No | `Male`, `Female`, `Co-Ed` |
| division | division | Display label derived from the Team classification. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| type | type | Type or category of the team. | Yes | String | Maximum 6 characters | Yes | `Indoor`, `Beach`, `Grass` |
| status | status | Current status of the team. | Yes | String | Maximum 8 characters | No | `active`, `inactive` |
| teamCode | teamCode | Standardized operational Team Code generated from the Team Code components. | Yes | String | 13 or 14 characters | Yes | `genderCode + ageGroup + playLevel + clubCode + teamRank + regionCode`; unique across USAV; Team UUID remains authoritative |
| genderCode | genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes | `M`, `F`, `C` |
| ageGroup | ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes | `J`, `A` |
| playLevel | playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes | Two uppercase letters or digits; approved play-level values are not fully specified |
| clubCode | clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes | Five uppercase letters or digits; unique within Region |
| teamRank | teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes | Two digits, zero-padded (for example, `01`) |
| regionCode | regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes | Two-character USAV Region Code or three-character foreign code |
| teamCodeComponents | teamCodeComponents | Individual values used to construct Team Code. | Yes | Object | N/A | No | N/A |
| members | members | Team roster returned when member expansion is requested. | No | Array of objects | N/A | No | N/A |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | club.id | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | club.name | Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | club.code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Team Code Components fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genderCode | teamCodeComponents.genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes | `M`, `F`, `C` |
| ageGroup | teamCodeComponents.ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes | `J`, `A` |
| playLevel | teamCodeComponents.playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes | Two uppercase letters or digits; approved play-level values are not fully specified |
| clubCode | teamCodeComponents.clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes | Five uppercase letters or digits; unique within Region |
| teamRank | teamCodeComponents.teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes | Two digits, zero-padded (for example, `01`) |
| regionCode | teamCodeComponents.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes | Two-character USAV Region Code or three-character foreign code |

### Team Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| profileId | members[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profile | members[].profile | Related member Profile details. | Yes | Object | N/A | No | N/A |
| role | members[].role | Member role on the Team. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| joinedAt | members[].joinedAt | Date and time the member joined the Team. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Team Member Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | members[].profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | members[].profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | members[].profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | members[].profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | members[].profile.birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | members[].profile.gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes | `Male`, `Female` |

## GET /api/v1/memberships

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Membership UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No | N/A |
| userId | userId | Related user UUID. | Yes | String (UUID) | 36 characters | Yes | UUID |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| region | region | Related Region details. | Yes | Object | N/A | No | N/A |
| status | status | Current status of the membership. | Yes | String | Maximum 9 characters | No | `Paid`, `Unpaid`, `Canceled`, `Suspended`, `Expired` |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| payment | payment | Payment details associated with the membership. | Yes | Object | N/A | Yes | N/A |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Membership Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].id | Membership Role UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| membershipRoleId | roles[].membershipRoleId | Role definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| role | roles[].role | Role code evaluated for eligibility. | Yes | Object | N/A | No | N/A |
| tierId | roles[].tierId | Membership tier UUID. | Yes | String (UUID) | 36 characters | Yes | UUID |
| tier | roles[].tier | Membership tier details. | Yes | Object | N/A | Yes | N/A |
| status | roles[].status | Current status of the membership role. | Yes | String | Maximum 8 characters | No | `Active`, `Removed`, `Unpaid`, `Upgraded` |
| clubAssignment | roles[].clubAssignment | Club assignment associated with the membership role. | Yes | Object | N/A | Yes | N/A |
| eligibility | roles[].eligibility | Eligibility summary stored with the membership role. | Yes | Object | N/A | No | N/A |
| credentials | roles[].credentials | Credential requirements associated with the membership role. | Yes | Array of objects | N/A | No | N/A |
| waivers | roles[].waivers | Waivers associated with the membership role. | Yes | Array of objects | N/A | No | N/A |
| updatedAt | roles[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].role.id | Role UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].role.name | Role name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |

### Tier fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].tier.id | Tier UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].tier.name | Tier name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| length | roles[].tier.length | Human-readable membership tier duration. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| category | roles[].tier.category | Membership tier category. | Yes | String | Maximum 7 characters | Yes | `Full`, `Limited` |
| windowType | roles[].tier.windowType | Method used to determine the membership tier validity window. | Yes | String | Maximum 10 characters | Yes | `FullSeason`, `Days`, `DateRange` |
| durationDays | roles[].tier.durationDays | Number of days in a fixed-duration membership tier. | Yes | Integer | N/A | Yes | N/A |
| windowStart | roles[].tier.windowStart | Start date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| windowEnd | roles[].tier.windowEnd | End date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| restrictions | roles[].tier.restrictions | Restrictions associated with the membership tier. | Yes | Object | N/A | No | N/A |

### Tier Restrictions fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| maxRoles | roles[].tier.restrictions.maxRoles | Maximum number of roles allowed by the membership tier. | Yes | Integer | N/A | Yes | N/A |
| allowedTypes | roles[].tier.restrictions.allowedTypes | Volleyball types allowed by the membership tier. | Yes | Array of strings | Each value: maximum 7 characters | No | `Indoor`, `Outdoor` |

### Club Assignment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| clubId | roles[].clubAssignment.clubId | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| club | roles[].clubAssignment.club | Related Club details. | Yes | Object | N/A | No | N/A |
| status | roles[].clubAssignment.status | Current status of the club assignment. | Yes | String | Maximum 11 characters | No | `APPROVED`, `REJECTED`, `IN_TRANSFER` |
| assignedAt | roles[].clubAssignment.assignedAt | Date and time the Club assignment was made. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Assigned Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].clubAssignment.club.id | Assigned Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].clubAssignment.club.name | Assigned Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | roles[].clubAssignment.club.code | Assigned Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Role Eligibility fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| isEligible | roles[].eligibility.isEligible | Stored eligibility indicator for the membership role. | Yes | Boolean | N/A | No | N/A |
| reasons | roles[].eligibility.reasons | Reasons supporting the stored eligibility result. | Yes | Array of strings | No maximum defined | No | N/A |

### Role Credential fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].id | Role Credential UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | roles[].credentials[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | roles[].credentials[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| credentialId | roles[].credentials[].credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| credential | roles[].credentials[].credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No | N/A |
| status | roles[].credentials[].status | Current status of the role credential. | Yes | String | Maximum 10 characters | No | `Completed`, `InProgress`, `NotStarted`, `Cancelled`, `Submitted`, `Failed` |
| requiredFrom | roles[].credentials[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| completedAt | roles[].credentials[].completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | roles[].credentials[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| updatedAt | roles[].credentials[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].credentials[].credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | roles[].credentials[].credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes | `SAFESPORT`, `LITMOS`, `NCSI` |

### Role Waiver fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].waivers[].id | Role Waiver UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | roles[].waivers[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | roles[].waivers[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | roles[].waivers[].type | Type or category of the role waiver. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| status | roles[].waivers[].status | Current status of the role waiver. | Yes | String | Maximum 9 characters | No | `Published` |
| requiredFrom | roles[].waivers[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| signedAt | roles[].waivers[].signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | roles[].waivers[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| version | roles[].waivers[].version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| updatedAt | roles[].waivers[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Payment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| amount | payment.amount | Payment amount. | Yes | Number | N/A | No | N/A |
| currency | payment.currency | Currency used for the payment. | Yes | String | 3 characters | No | Not specified |
| method | payment.method | Payment method. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| transactionId | payment.transactionId | Payment processor transaction identifier. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## GET /api/v1/memberships/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Membership UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No | N/A |
| userId | userId | Related user UUID. | Yes | String (UUID) | 36 characters | Yes | UUID |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| region | region | Related Region details. | Yes | Object | N/A | No | N/A |
| status | status | Current status of the membership. | Yes | String | Maximum 9 characters | No | `Paid`, `Unpaid`, `Canceled`, `Suspended`, `Expired` |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes | N/A |
| payment | payment | Payment details associated with the membership. | Yes | Object | N/A | Yes | N/A |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Membership Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].id | Membership Role UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| membershipRoleId | roles[].membershipRoleId | Role definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| role | roles[].role | Role code evaluated for eligibility. | Yes | Object | N/A | No | N/A |
| tierId | roles[].tierId | Membership tier UUID. | Yes | String (UUID) | 36 characters | Yes | UUID |
| tier | roles[].tier | Membership tier details. | Yes | Object | N/A | Yes | N/A |
| status | roles[].status | Current status of the membership role. | Yes | String | Maximum 8 characters | No | `Active`, `Removed`, `Unpaid`, `Upgraded` |
| clubAssignment | roles[].clubAssignment | Club assignment associated with the membership role. | Yes | Object | N/A | Yes | N/A |
| eligibility | roles[].eligibility | Eligibility summary stored with the membership role. | Yes | Object | N/A | No | N/A |
| credentials | roles[].credentials | Credential requirements associated with the membership role. | Yes | Array of objects | N/A | No | N/A |
| waivers | roles[].waivers | Waivers associated with the membership role. | Yes | Array of objects | N/A | No | N/A |
| updatedAt | roles[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].role.id | Role UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].role.name | Role name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |

### Tier fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].tier.id | Tier UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].tier.name | Tier name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| length | roles[].tier.length | Human-readable membership tier duration. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| category | roles[].tier.category | Membership tier category. | Yes | String | Maximum 7 characters | Yes | `Full`, `Limited` |
| windowType | roles[].tier.windowType | Method used to determine the membership tier validity window. | Yes | String | Maximum 10 characters | Yes | `FullSeason`, `Days`, `DateRange` |
| durationDays | roles[].tier.durationDays | Number of days in a fixed-duration membership tier. | Yes | Integer | N/A | Yes | N/A |
| windowStart | roles[].tier.windowStart | Start date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| windowEnd | roles[].tier.windowEnd | End date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| restrictions | roles[].tier.restrictions | Restrictions associated with the membership tier. | Yes | Object | N/A | No | N/A |

### Tier Restrictions fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| maxRoles | roles[].tier.restrictions.maxRoles | Maximum number of roles allowed by the membership tier. | Yes | Integer | N/A | Yes | N/A |
| allowedTypes | roles[].tier.restrictions.allowedTypes | Volleyball types allowed by the membership tier. | Yes | Array of strings | Each value: maximum 7 characters | No | `Indoor`, `Outdoor` |

### Club Assignment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| clubId | roles[].clubAssignment.clubId | Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| club | roles[].clubAssignment.club | Related Club details. | Yes | Object | N/A | No | N/A |
| status | roles[].clubAssignment.status | Current status of the club assignment. | Yes | String | Maximum 11 characters | No | `APPROVED`, `REJECTED`, `IN_TRANSFER` |
| assignedAt | roles[].clubAssignment.assignedAt | Date and time the Club assignment was made. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |

### Assigned Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].clubAssignment.club.id | Assigned Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].clubAssignment.club.name | Assigned Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | roles[].clubAssignment.club.code | Assigned Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Role Eligibility fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| isEligible | roles[].eligibility.isEligible | Stored eligibility indicator for the membership role. | Yes | Boolean | N/A | No | N/A |
| reasons | roles[].eligibility.reasons | Reasons supporting the stored eligibility result. | Yes | Array of strings | No maximum defined | No | N/A |

### Role Credential fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].id | Role Credential UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | roles[].credentials[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | roles[].credentials[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| credentialId | roles[].credentials[].credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| credential | roles[].credentials[].credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No | N/A |
| status | roles[].credentials[].status | Current status of the role credential. | Yes | String | Maximum 10 characters | No | `Completed`, `InProgress`, `NotStarted`, `Cancelled`, `Submitted`, `Failed` |
| requiredFrom | roles[].credentials[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| completedAt | roles[].credentials[].completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | roles[].credentials[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| updatedAt | roles[].credentials[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | roles[].credentials[].credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | roles[].credentials[].credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes | `SAFESPORT`, `LITMOS`, `NCSI` |

### Role Waiver fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | roles[].waivers[].id | Role Waiver UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | roles[].waivers[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | roles[].waivers[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | roles[].waivers[].type | Type or category of the role waiver. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| status | roles[].waivers[].status | Current status of the role waiver. | Yes | String | Maximum 9 characters | No | `Published` |
| requiredFrom | roles[].waivers[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| signedAt | roles[].waivers[].signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | roles[].waivers[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| version | roles[].waivers[].version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| updatedAt | roles[].waivers[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No | UUID |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Payment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| amount | payment.amount | Payment amount. | Yes | Number | N/A | No | N/A |
| currency | payment.currency | Currency used for the payment. | Yes | String | 3 characters | No | Not specified |
| method | payment.method | Payment method. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| transactionId | payment.transactionId | Payment processor transaction identifier. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## GET /api/v1/credentials

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Credential assignment UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| credentialId | credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| credential | credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No | N/A |
| status | status | Current status of the credential assignment. | Yes | String | Maximum 10 characters | No | `Completed`, `InProgress`, `NotStarted`, `Cancelled`, `Submitted`, `Failed` |
| requiredFrom | requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| completedAt | completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes | `SAFESPORT`, `LITMOS`, `NCSI` |

## GET /api/v1/waivers

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Composite identifier for the waiver. | Yes | String (composite ID) | No maximum defined | No | Not specified |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| type | type | Type or category of the waiver. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| status | status | Current status of the waiver. | Yes | String | Maximum 9 characters | No | `Published` |
| requiredFrom | requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| signedAt | signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| validUntil | validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC; expiration-boundary meaning remains unresolved |
| version | version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

## GET /api/v1/suspensions

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Suspension UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No | `global`, `role-specific` |
| affectedRoles | affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes | N/A |
| reason | reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| canceledAt | canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

## GET /api/v1/club-transfers

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | id | Club transfer UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No | N/A |
| fromClubId | fromClubId | Source Club UUID for the transfer. | Yes | String (UUID) | 36 characters | Yes | UUID |
| fromClub | fromClub | Source Club details for the transfer. | Yes | Object | N/A | Yes | N/A |
| toClubId | toClubId | Destination Club UUID for the transfer. | Yes | String (UUID) | 36 characters | No | UUID |
| toClub | toClub | Destination Club details for the transfer. | Yes | Object | N/A | No | N/A |
| status | status | Current status of the club transfer. | Yes | String | Maximum 10 characters | No | `Approved`, `Rejected`, `InTransfer` |
| requestedAt | requestedAt | Date and time the Club transfer was requested. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |
| completedAt | completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes | ISO 8601 UTC |
| requestedBy | requestedBy | Identifier of the user who requested the Club transfer. | Yes | String | 36 characters | Yes | UUID |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No | ISO 8601 UTC |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |

### Previous Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | fromClub.id | Previous Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | fromClub.name | Previous Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | fromClub.code | Previous Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

### Destination Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| id | toClub.id | Destination Club UUID. | Yes | String (UUID) | 36 characters | No | UUID |
| name | toClub.name | Destination Club name. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| code | toClub.code | Destination Club short code. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |

## POST /api/v1/eligibility/batch

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | Yes | Boolean | N/A | No | N/A |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes | `Male`, `Female` |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes | N/A |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No | N/A |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes | N/A |

## GET /api/v1/eligibility/club/{clubId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No | N/A |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No | N/A |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes | `Male`, `Female` |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes | N/A |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No | `club`, `team`, `region` |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No | UUID |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No | UUID |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No | UUID |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No | N/A |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes | N/A |

## GET /api/v1/eligibility/team/{teamId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No | N/A |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No | N/A |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes | `Male`, `Female` |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes | N/A |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No | `club`, `team`, `region` |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No | UUID |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No | UUID |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No | UUID |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No | N/A |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes | N/A |

## GET /api/v1/eligibility/region/{regionId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No | N/A |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No | N/A |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes | `Male`, `Female` |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes | N/A |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No | `club`, `team`, `region` |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No | UUID |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No | UUID |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No | UUID |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes | Not specified |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No | N/A |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes | N/A |

## GET /api/v1/eligibility/{memberId}

### Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| memberFound | memberFound | Whether the requested USAV member was found. | Yes | Boolean | N/A | No | N/A |
| firstName | firstName | Member first name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| lastName | lastName | Member last name. | No | String | Minimum 1; no maximum defined | No | Not specified |
| birthDate | birthDate | Member date of birth. | No | String (date) | 10 characters | Yes | `YYYY-MM-DD` |
| gender | gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes | `Male`, `Female` |
| graduationYear | graduationYear | High-school graduation year. | No | Integer | N/A | Yes | N/A |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No | `YYYY-MM-DD` |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No | N/A |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| role | roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| tier | roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes | Not specified |
| eligible | roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| conditions | roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No | N/A |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable | Values / format |
| --- | --- | --- | --- | --- | --- | --- | --- |
| condition | roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No | Not specified |
| passed | roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No | N/A |
| details | roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes | N/A |
