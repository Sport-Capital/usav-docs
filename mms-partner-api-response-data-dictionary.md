# MMS Partner API field dictionary

Date: October 13, 2026

For the complete field dictionary, including allowed values and formats, [view or download the CSV](mms-partner-api-response-data-dictionary.csv).

Each endpoint lists its delivered record fields. The original field name is preserved, and Qualified name shows the field path when it is nested. Pagination is excluded.

Required applies when the containing record is returned. Nullable means the field can contain null. No maximum defined means the current API contract and MMS database do not enforce a maximum for that field. Maximum not specified means the available source did not provide a limit.

Definitions and constraints use the current API v2.1 documentation and the updated Sports Capital field-length file received October 13, 2026. The longest value observed in staging is not treated as a maximum length.

For the Clubs, Profiles, Teams and Memberships list endpoints, the tables use the supplied object schemas; their list-item shapes still need confirmation.

## GET /api/v1/clubs

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | name | Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No |
| region | region | Related Region details. | Yes | Object | N/A | No |
| affiliationStatus | affiliationStatus | Current Club affiliation status. | Yes | String | Maximum 11 characters | No |
| affiliationStatusUpdatedAt | affiliationStatusUpdatedAt | Date and time the Club affiliation status last changed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| type | type | Type or category of the club. | Yes | Array of strings | Each value: maximum 7 characters | No |
| gender | gender | Gender value returned for the record. | Yes | Array of strings | Each value: maximum 6 characters | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| contact | contact | Primary Club contact details. | Yes | Object | N/A | Yes |
| location | location | Primary Club facility details. | Yes | Object | N/A | Yes |
| logoUrl | logoUrl | URL of the Club logo image. | Yes | String | Minimum 1; no maximum defined | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| metadata | metadata | Additional metadata returned with the record. | Yes | Object | N/A | No |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Contact fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| name | contact.name | Contact name. | Yes | String | Minimum 1; no maximum defined | Yes |
| email | contact.email | Email address. | Yes | String | Minimum 1; no maximum defined | Yes |
| phone | contact.phone | Telephone number. | Yes | String | Minimum 1; no maximum defined | Yes |
| role | contact.role | Contact person's role or title. | Yes | String | Minimum 1; no maximum defined | Yes |

### Location fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| name | location.name | Location name. | Yes | String | Minimum 1; no maximum defined | Yes |
| address | location.address | Address returned for the record. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | location.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | location.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | location.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | location.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

## GET /api/v1/clubs/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | name | Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No |
| region | region | Related Region details. | Yes | Object | N/A | No |
| affiliationStatus | affiliationStatus | Current Club affiliation status. | Yes | String | Maximum 11 characters | No |
| affiliationStatusUpdatedAt | affiliationStatusUpdatedAt | Date and time the Club affiliation status last changed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| type | type | Type or category of the club. | Yes | Array of strings | Each value: maximum 7 characters | No |
| gender | gender | Gender value returned for the record. | Yes | Array of strings | Each value: maximum 6 characters | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| contact | contact | Primary Club contact details. | Yes | Object | N/A | Yes |
| location | location | Primary Club facility details. | Yes | Object | N/A | Yes |
| logoUrl | logoUrl | URL of the Club logo image. | Yes | String | Minimum 1; no maximum defined | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| metadata | metadata | Additional metadata returned with the record. | Yes | Object | N/A | No |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Contact fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| name | contact.name | Contact name. | Yes | String | Minimum 1; no maximum defined | Yes |
| email | contact.email | Email address. | Yes | String | Minimum 1; no maximum defined | Yes |
| phone | contact.phone | Telephone number. | Yes | String | Minimum 1; no maximum defined | Yes |
| role | contact.role | Contact person's role or title. | Yes | String | Minimum 1; no maximum defined | Yes |

### Location fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| name | location.name | Location name. | Yes | String | Minimum 1; no maximum defined | Yes |
| address | location.address | Address returned for the record. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | location.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | location.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | location.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | location.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

## GET /api/v1/regions

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Region UUID. | Yes | String (UUID) | 36 characters | No |
| name | name | Region name. | Yes | String | Minimum 1; no maximum defined | No |
| code | code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes |

## GET /api/v1/profiles

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |
| middleName | middleName | Member middle name. | Yes | String | Minimum 1; no maximum defined | Yes |
| birthDate | birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes |
| gender | gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes |
| genderIdentity | genderIdentity | Preferred display gender identity. | Yes | String | Maximum 10 characters | Yes |
| phoneNumber | phoneNumber | Primary contact telephone number for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes |
| contactEmail | contactEmail | Primary contact email for the Profile. | Yes | String | No maximum defined | Yes |
| status | status | Current status of the profile. | Yes | String | Maximum 8 characters | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| guardians | guardians | Guardian relationships associated with the Profile. | Yes | Array of objects | N/A | No |
| suspensions | suspensions | Suspensions associated with the Profile. | Yes | Array of objects | N/A | No |
| demographics | demographics | Demographic information associated with the Profile. | Yes | Object | N/A | No |
| preferences | preferences | Preferences and consent values associated with the Profile. | Yes | Object | N/A | No |
| education | education | Education information associated with the Profile. | Yes | Object | N/A | No |
| avatarUrl | avatarUrl | URL of the Profile avatar image. | Yes | String | Minimum 1; no maximum defined | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Guardian fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| userId | guardians[].userId | Related user UUID. | Yes | String (UUID) | 36 characters | No |
| email | guardians[].email | Email address. | Yes | String | No maximum defined | Yes |
| isMainGuardian | guardians[].isMainGuardian | Whether this guardian is the primary guardian. | Yes | Boolean | N/A | No |

### Suspension fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | suspensions[].id | Suspension UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | suspensions[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | suspensions[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| scope | suspensions[].scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No |
| affectedRoles | suspensions[].affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes |
| reason | suspensions[].reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No |
| startDate | suspensions[].startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| endDate | suspensions[].endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| canceledAt | suspensions[].canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| createdAt | suspensions[].createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | suspensions[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Demographics fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| ethnicity | demographics.ethnicity | Ethnicity value recorded for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes |
| disability | demographics.disability | Whether the Profile records a disability indicator. | Yes | Boolean | N/A | Yes |
| militaryService | demographics.militaryService | Whether the Profile records military service. | Yes | Boolean | N/A | Yes |

### Preferences fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| ncsaShareAgreed | preferences.ncsaShareAgreed | Whether the member agreed to NCSA data sharing. | Yes | Boolean | N/A | Yes |
| ncsaShareAgreedAt | preferences.ncsaShareAgreedAt | Date and time the NCSA sharing choice was recorded. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Education fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| graduationYear | education.graduationYear | High-school graduation year. | Yes | Integer | N/A | Yes |

## GET /api/v1/profiles/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |
| middleName | middleName | Member middle name. | Yes | String | Minimum 1; no maximum defined | Yes |
| birthDate | birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes |
| gender | gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes |
| genderIdentity | genderIdentity | Preferred display gender identity. | Yes | String | Maximum 10 characters | Yes |
| phoneNumber | phoneNumber | Primary contact telephone number for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes |
| contactEmail | contactEmail | Primary contact email for the Profile. | Yes | String | No maximum defined | Yes |
| status | status | Current status of the profile. | Yes | String | Maximum 8 characters | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| guardians | guardians | Guardian relationships associated with the Profile. | Yes | Array of objects | N/A | No |
| suspensions | suspensions | Suspensions associated with the Profile. | Yes | Array of objects | N/A | No |
| demographics | demographics | Demographic information associated with the Profile. | Yes | Object | N/A | No |
| preferences | preferences | Preferences and consent values associated with the Profile. | Yes | Object | N/A | No |
| education | education | Education information associated with the Profile. | Yes | Object | N/A | No |
| avatarUrl | avatarUrl | URL of the Profile avatar image. | Yes | String | Minimum 1; no maximum defined | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Guardian fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| userId | guardians[].userId | Related user UUID. | Yes | String (UUID) | 36 characters | No |
| email | guardians[].email | Email address. | Yes | String | No maximum defined | Yes |
| isMainGuardian | guardians[].isMainGuardian | Whether this guardian is the primary guardian. | Yes | Boolean | N/A | No |

### Suspension fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | suspensions[].id | Suspension UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | suspensions[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | suspensions[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| scope | suspensions[].scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No |
| affectedRoles | suspensions[].affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes |
| reason | suspensions[].reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No |
| startDate | suspensions[].startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| endDate | suspensions[].endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| canceledAt | suspensions[].canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| createdAt | suspensions[].createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | suspensions[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Demographics fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| ethnicity | demographics.ethnicity | Ethnicity value recorded for the Profile. | Yes | String | Minimum 1; no maximum defined | Yes |
| disability | demographics.disability | Whether the Profile records a disability indicator. | Yes | Boolean | N/A | Yes |
| militaryService | demographics.militaryService | Whether the Profile records military service. | Yes | Boolean | N/A | Yes |

### Preferences fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| ncsaShareAgreed | preferences.ncsaShareAgreed | Whether the member agreed to NCSA data sharing. | Yes | Boolean | N/A | Yes |
| ncsaShareAgreedAt | preferences.ncsaShareAgreedAt | Date and time the NCSA sharing choice was recorded. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Education fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| graduationYear | education.graduationYear | High-school graduation year. | Yes | Integer | N/A | Yes |

## GET /api/v1/teams

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Team UUID. | Yes | String (UUID) | 36 characters | No |
| name | name | Team name. | Yes | String | Minimum 1; no maximum defined | No |
| clubId | clubId | Club UUID. | Yes | String (UUID) | 36 characters | No |
| club | club | Related Club details. | No | Object | N/A | No |
| ageDefinition | ageDefinition | Junior age division assigned to the Team; may be null for adult Teams. | Yes | String | Maximum 3 characters | Yes |
| gender | gender | Gender value returned for the record. | Yes | String | Maximum 6 characters | No |
| division | division | Display label derived from the Team classification. | Yes | String | Minimum 1; no maximum defined | Yes |
| type | type | Type or category of the team. | Yes | String | Maximum 6 characters | Yes |
| status | status | Current status of the team. | Yes | String | Maximum 8 characters | No |
| teamCode | teamCode | Standardized operational Team Code generated from the Team Code components. | Yes | String | 13 or 14 characters | Yes |
| genderCode | genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes |
| ageGroup | ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes |
| playLevel | playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes |
| clubCode | clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes |
| teamRank | teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes |
| regionCode | regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes |
| teamCodeComponents | teamCodeComponents | Individual values used to construct Team Code. | Yes | Object | N/A | No |
| members | members | Team roster returned when member expansion is requested. | No | Array of objects | N/A | No |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | club.id | Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | club.name | Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | club.code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Team Code Components fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| genderCode | teamCodeComponents.genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes |
| ageGroup | teamCodeComponents.ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes |
| playLevel | teamCodeComponents.playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes |
| clubCode | teamCodeComponents.clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes |
| teamRank | teamCodeComponents.teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes |
| regionCode | teamCodeComponents.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes |

### Team Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| profileId | members[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| profile | members[].profile | Related member Profile details. | Yes | Object | N/A | No |
| role | members[].role | Member role on the Team. | Yes | String | Minimum 1; no maximum defined | Yes |
| joinedAt | members[].joinedAt | Date and time the member joined the Team. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Team Member Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | members[].profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | members[].profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | members[].profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | members[].profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |
| birthDate | members[].profile.birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes |
| gender | members[].profile.gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes |

## GET /api/v1/teams/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Team UUID. | Yes | String (UUID) | 36 characters | No |
| name | name | Team name. | Yes | String | Minimum 1; no maximum defined | No |
| clubId | clubId | Club UUID. | Yes | String (UUID) | 36 characters | No |
| club | club | Related Club details. | No | Object | N/A | No |
| ageDefinition | ageDefinition | Junior age division assigned to the Team; may be null for adult Teams. | Yes | String | Maximum 3 characters | Yes |
| gender | gender | Gender value returned for the record. | Yes | String | Maximum 6 characters | No |
| division | division | Display label derived from the Team classification. | Yes | String | Minimum 1; no maximum defined | Yes |
| type | type | Type or category of the team. | Yes | String | Maximum 6 characters | Yes |
| status | status | Current status of the team. | Yes | String | Maximum 8 characters | No |
| teamCode | teamCode | Standardized operational Team Code generated from the Team Code components. | Yes | String | 13 or 14 characters | Yes |
| genderCode | genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes |
| ageGroup | ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes |
| playLevel | playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes |
| clubCode | clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes |
| teamRank | teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes |
| regionCode | regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes |
| teamCodeComponents | teamCodeComponents | Individual values used to construct Team Code. | Yes | Object | N/A | No |
| members | members | Team roster returned when member expansion is requested. | No | Array of objects | N/A | No |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | club.id | Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | club.name | Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | club.code | Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Team Code Components fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| genderCode | teamCodeComponents.genderCode | One-character gender component used in Team Code. | Yes | String | 1 character | Yes |
| ageGroup | teamCodeComponents.ageGroup | Team classification used in Team Code: Junior or Adult. | Yes | String | 1 character | Yes |
| playLevel | teamCodeComponents.playLevel | Two-character junior age or adult play-level component used in Team Code. | Yes | String | 2 characters | Yes |
| clubCode | teamCodeComponents.clubCode | Club Code component used in Team Code or the scoped Club code. | Yes | String | 5 characters | Yes |
| teamRank | teamCodeComponents.teamRank | Two-digit Team rank component used in Team Code. | Yes | String | 2 characters | Yes |
| regionCode | teamCodeComponents.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | Yes | String | 2 to 3 characters | Yes |

### Team Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| profileId | members[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| profile | members[].profile | Related member Profile details. | Yes | Object | N/A | No |
| role | members[].role | Member role on the Team. | Yes | String | Minimum 1; no maximum defined | Yes |
| joinedAt | members[].joinedAt | Date and time the member joined the Team. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Team Member Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | members[].profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | members[].profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | members[].profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | members[].profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |
| birthDate | members[].profile.birthDate | Member date of birth. | Yes | String (date) | 10 characters | Yes |
| gender | members[].profile.gender | Member birth gender used for division and eligibility evaluation. | Yes | String | Maximum 6 characters | Yes |

## GET /api/v1/memberships

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Membership UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No |
| userId | userId | Related user UUID. | Yes | String (UUID) | 36 characters | Yes |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No |
| region | region | Related Region details. | Yes | Object | N/A | No |
| status | status | Current status of the membership. | Yes | String | Maximum 9 characters | No |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (date) | 10 characters | No |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (date) | 10 characters | No |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| payment | payment | Payment details associated with the membership. | Yes | Object | N/A | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Membership Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].id | Membership Role UUID. | Yes | String (UUID) | 36 characters | No |
| membershipRoleId | roles[].membershipRoleId | Role definition UUID. | Yes | String (UUID) | 36 characters | No |
| role | roles[].role | Role code evaluated for eligibility. | Yes | Object | N/A | No |
| tierId | roles[].tierId | Membership tier UUID. | Yes | String (UUID) | 36 characters | Yes |
| tier | roles[].tier | Membership tier details. | Yes | Object | N/A | Yes |
| status | roles[].status | Current status of the membership role. | Yes | String | Maximum 8 characters | No |
| clubAssignment | roles[].clubAssignment | Club assignment associated with the membership role. | Yes | Object | N/A | Yes |
| eligibility | roles[].eligibility | Eligibility summary stored with the membership role. | Yes | Object | N/A | No |
| credentials | roles[].credentials | Credential requirements associated with the membership role. | Yes | Array of objects | N/A | No |
| waivers | roles[].waivers | Waivers associated with the membership role. | Yes | Array of objects | N/A | No |
| updatedAt | roles[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].role.id | Role UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].role.name | Role name. | Yes | String | Minimum 1; no maximum defined | No |

### Tier fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].tier.id | Tier UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].tier.name | Tier name. | Yes | String | Minimum 1; no maximum defined | No |
| length | roles[].tier.length | Human-readable membership tier duration. | Yes | String | Minimum 1; no maximum defined | Yes |
| category | roles[].tier.category | Membership tier category. | Yes | String | Maximum 7 characters | Yes |
| windowType | roles[].tier.windowType | Method used to determine the membership tier validity window. | Yes | String | Maximum 10 characters | Yes |
| durationDays | roles[].tier.durationDays | Number of days in a fixed-duration membership tier. | Yes | Integer | N/A | Yes |
| windowStart | roles[].tier.windowStart | Start date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes |
| windowEnd | roles[].tier.windowEnd | End date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes |
| restrictions | roles[].tier.restrictions | Restrictions associated with the membership tier. | Yes | Object | N/A | No |

### Tier Restrictions fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| maxRoles | roles[].tier.restrictions.maxRoles | Maximum number of roles allowed by the membership tier. | Yes | Integer | N/A | Yes |
| allowedTypes | roles[].tier.restrictions.allowedTypes | Volleyball types allowed by the membership tier. | Yes | Array of strings | Each value: maximum 7 characters | No |

### Club Assignment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| clubId | roles[].clubAssignment.clubId | Club UUID. | Yes | String (UUID) | 36 characters | No |
| club | roles[].clubAssignment.club | Related Club details. | Yes | Object | N/A | No |
| status | roles[].clubAssignment.status | Current status of the club assignment. | Yes | String | Maximum 11 characters | No |
| assignedAt | roles[].clubAssignment.assignedAt | Date and time the Club assignment was made. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Assigned Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].clubAssignment.club.id | Assigned Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].clubAssignment.club.name | Assigned Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | roles[].clubAssignment.club.code | Assigned Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Role Eligibility fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| isEligible | roles[].eligibility.isEligible | Stored eligibility indicator for the membership role. | Yes | Boolean | N/A | No |
| reasons | roles[].eligibility.reasons | Reasons supporting the stored eligibility result. | Yes | Array of strings | No maximum defined | No |

### Role Credential fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].id | Role Credential UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | roles[].credentials[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | roles[].credentials[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| credentialId | roles[].credentials[].credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| credential | roles[].credentials[].credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No |
| status | roles[].credentials[].status | Current status of the role credential. | Yes | String | Maximum 10 characters | No |
| requiredFrom | roles[].credentials[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| completedAt | roles[].credentials[].completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | roles[].credentials[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| updatedAt | roles[].credentials[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].credentials[].credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No |
| type | roles[].credentials[].credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes |

### Role Waiver fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].waivers[].id | Role Waiver UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | roles[].waivers[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | roles[].waivers[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| type | roles[].waivers[].type | Type or category of the role waiver. | Yes | String | Minimum 1; no maximum defined | No |
| status | roles[].waivers[].status | Current status of the role waiver. | Yes | String | Maximum 9 characters | No |
| requiredFrom | roles[].waivers[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| signedAt | roles[].waivers[].signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | roles[].waivers[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| version | roles[].waivers[].version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes |
| updatedAt | roles[].waivers[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Payment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| amount | payment.amount | Payment amount. | Yes | Number | N/A | No |
| currency | payment.currency | Currency used for the payment. | Yes | String | 3 characters | No |
| method | payment.method | Payment method. | Yes | String | Minimum 1; no maximum defined | No |
| transactionId | payment.transactionId | Payment processor transaction identifier. | Yes | String | Minimum 1; no maximum defined | Yes |

## GET /api/v1/memberships/{id}

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Membership UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No |
| userId | userId | Related user UUID. | Yes | String (UUID) | 36 characters | Yes |
| regionId | regionId | Region UUID. | Yes | String (UUID) | 36 characters | No |
| region | region | Related Region details. | Yes | Object | N/A | No |
| status | status | Current status of the membership. | Yes | String | Maximum 9 characters | No |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (date) | 10 characters | No |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (date) | 10 characters | No |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |
| address | address | Address returned for the record. | Yes | Object | N/A | Yes |
| payment | payment | Payment details associated with the membership. | Yes | Object | N/A | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |

### Region fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | region.id | Region UUID. | Yes | String (UUID) | 36 characters | No |
| name | region.name | Region name. | Yes | String | Minimum 1; no maximum defined | No |
| code | region.code | Region short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Membership Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].id | Membership Role UUID. | Yes | String (UUID) | 36 characters | No |
| membershipRoleId | roles[].membershipRoleId | Role definition UUID. | Yes | String (UUID) | 36 characters | No |
| role | roles[].role | Role code evaluated for eligibility. | Yes | Object | N/A | No |
| tierId | roles[].tierId | Membership tier UUID. | Yes | String (UUID) | 36 characters | Yes |
| tier | roles[].tier | Membership tier details. | Yes | Object | N/A | Yes |
| status | roles[].status | Current status of the membership role. | Yes | String | Maximum 8 characters | No |
| clubAssignment | roles[].clubAssignment | Club assignment associated with the membership role. | Yes | Object | N/A | Yes |
| eligibility | roles[].eligibility | Eligibility summary stored with the membership role. | Yes | Object | N/A | No |
| credentials | roles[].credentials | Credential requirements associated with the membership role. | Yes | Array of objects | N/A | No |
| waivers | roles[].waivers | Waivers associated with the membership role. | Yes | Array of objects | N/A | No |
| updatedAt | roles[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Role fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].role.id | Role UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].role.name | Role name. | Yes | String | Minimum 1; no maximum defined | No |

### Tier fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].tier.id | Tier UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].tier.name | Tier name. | Yes | String | Minimum 1; no maximum defined | No |
| length | roles[].tier.length | Human-readable membership tier duration. | Yes | String | Minimum 1; no maximum defined | Yes |
| category | roles[].tier.category | Membership tier category. | Yes | String | Maximum 7 characters | Yes |
| windowType | roles[].tier.windowType | Method used to determine the membership tier validity window. | Yes | String | Maximum 10 characters | Yes |
| durationDays | roles[].tier.durationDays | Number of days in a fixed-duration membership tier. | Yes | Integer | N/A | Yes |
| windowStart | roles[].tier.windowStart | Start date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes |
| windowEnd | roles[].tier.windowEnd | End date of an explicit membership tier validity window. | Yes | String (date) | 10 characters | Yes |
| restrictions | roles[].tier.restrictions | Restrictions associated with the membership tier. | Yes | Object | N/A | No |

### Tier Restrictions fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| maxRoles | roles[].tier.restrictions.maxRoles | Maximum number of roles allowed by the membership tier. | Yes | Integer | N/A | Yes |
| allowedTypes | roles[].tier.restrictions.allowedTypes | Volleyball types allowed by the membership tier. | Yes | Array of strings | Each value: maximum 7 characters | No |

### Club Assignment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| clubId | roles[].clubAssignment.clubId | Club UUID. | Yes | String (UUID) | 36 characters | No |
| club | roles[].clubAssignment.club | Related Club details. | Yes | Object | N/A | No |
| status | roles[].clubAssignment.status | Current status of the club assignment. | Yes | String | Maximum 11 characters | No |
| assignedAt | roles[].clubAssignment.assignedAt | Date and time the Club assignment was made. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |

### Assigned Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].clubAssignment.club.id | Assigned Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].clubAssignment.club.name | Assigned Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | roles[].clubAssignment.club.code | Assigned Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Role Eligibility fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| isEligible | roles[].eligibility.isEligible | Stored eligibility indicator for the membership role. | Yes | Boolean | N/A | No |
| reasons | roles[].eligibility.reasons | Reasons supporting the stored eligibility result. | Yes | Array of strings | No maximum defined | No |

### Role Credential fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].id | Role Credential UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | roles[].credentials[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | roles[].credentials[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| credentialId | roles[].credentials[].credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| credential | roles[].credentials[].credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No |
| status | roles[].credentials[].status | Current status of the role credential. | Yes | String | Maximum 10 characters | No |
| requiredFrom | roles[].credentials[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| completedAt | roles[].credentials[].completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | roles[].credentials[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| updatedAt | roles[].credentials[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].credentials[].credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| name | roles[].credentials[].credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No |
| type | roles[].credentials[].credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes |

### Role Waiver fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | roles[].waivers[].id | Role Waiver UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | roles[].waivers[].profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | roles[].waivers[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| type | roles[].waivers[].type | Type or category of the role waiver. | Yes | String | Minimum 1; no maximum defined | No |
| status | roles[].waivers[].status | Current status of the role waiver. | Yes | String | Maximum 9 characters | No |
| requiredFrom | roles[].waivers[].requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| signedAt | roles[].waivers[].signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | roles[].waivers[].validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| version | roles[].waivers[].version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes |
| updatedAt | roles[].waivers[].updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Address fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | address.id | Address UUID. | No | String (UUID) | 36 characters | No |
| name | address.name | Address name. | No | String | Minimum 1; no maximum defined | No |
| street | address.street | Street portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| addressExtra | address.addressExtra | Additional address information, such as a unit or suite. | No | String | Minimum 1; no maximum defined | Yes |
| city | address.city | City portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| state | address.state | State or province portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |
| zipCode | address.zipCode | Postal code portion of the address. | Yes | String | Minimum 1; no maximum defined | Yes |

### Payment fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| amount | payment.amount | Payment amount. | Yes | Number | N/A | No |
| currency | payment.currency | Currency used for the payment. | Yes | String | 3 characters | No |
| method | payment.method | Payment method. | Yes | String | Minimum 1; no maximum defined | No |
| transactionId | payment.transactionId | Payment processor transaction identifier. | Yes | String | Minimum 1; no maximum defined | Yes |

## GET /api/v1/credentials

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Credential assignment UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| credentialId | credentialId | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| credential | credential | Credential definition associated with the credential assignment. | Yes | Object | N/A | No |
| status | status | Current status of the credential assignment. | Yes | String | Maximum 10 characters | No |
| requiredFrom | requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| completedAt | completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Credential Definition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | credential.id | Credential definition UUID. | Yes | String (UUID) | 36 characters | No |
| name | credential.name | Credential definition name. | Yes | String | Minimum 1; no maximum defined | No |
| type | credential.type | Type or category of the credential definition. | Yes | String | Maximum 9 characters | Yes |

## GET /api/v1/waivers

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Composite identifier for the waiver. | Yes | String (composite ID) | No maximum defined | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| type | type | Type or category of the waiver. | Yes | String | Minimum 1; no maximum defined | No |
| status | status | Current status of the waiver. | Yes | String | Maximum 9 characters | No |
| requiredFrom | requiredFrom | Date and time the requirement begins to apply. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| signedAt | signedAt | Date and time the member signed the waiver. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| validUntil | validUntil | Credential or waiver expiration timestamp. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| version | version | Waiver version. | Yes | String | Minimum 1; no maximum defined | Yes |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

## GET /api/v1/suspensions

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Suspension UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | String | Maximum 13 characters | No |
| affectedRoles | affectedRoles | Role codes affected by the suspension; empty or null indicates a global suspension. | Yes | Array of strings | Minimum 1; no maximum defined | Yes |
| reason | reason | Reason recorded for the suspension. | Yes | String | Minimum 1; no maximum defined | No |
| startDate | startDate | Start date or start timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| endDate | endDate | End date or end timestamp for the record. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| canceledAt | canceledAt | Date and time the suspension was canceled or lifted. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| createdAt | createdAt | Date and time the record was created. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

## GET /api/v1/club-transfers

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | id | Club transfer UUID. | Yes | String (UUID) | 36 characters | No |
| profileId | profileId | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| profile | profile | Related member Profile details. | Yes | Object | N/A | No |
| fromClubId | fromClubId | Source Club UUID for the transfer. | Yes | String (UUID) | 36 characters | Yes |
| fromClub | fromClub | Source Club details for the transfer. | Yes | Object | N/A | Yes |
| toClubId | toClubId | Destination Club UUID for the transfer. | Yes | String (UUID) | 36 characters | No |
| toClub | toClub | Destination Club details for the transfer. | Yes | Object | N/A | No |
| status | status | Current status of the club transfer. | Yes | String | Maximum 10 characters | No |
| requestedAt | requestedAt | Date and time the Club transfer was requested. | Yes | String (UTC timestamp) | 20 or 24 characters | No |
| completedAt | completedAt | Date and time the credential or transfer was completed. | Yes | String (UTC timestamp) | 20 or 24 characters | Yes |
| requestedBy | requestedBy | Identifier of the user who requested the Club transfer. | Yes | String | 36 characters | Yes |
| updatedAt | updatedAt | Date and time the record was last updated. | Yes | String (UTC timestamp) | 20 or 24 characters | No |

### Profile fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | profile.id | Profile UUID. | Yes | String (UUID) | 36 characters | No |
| memberId | profile.memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| firstName | profile.firstName | Member first name. | Yes | String | Minimum 1; no maximum defined | No |
| lastName | profile.lastName | Member last name. | Yes | String | Minimum 1; no maximum defined | No |

### Previous Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | fromClub.id | Previous Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | fromClub.name | Previous Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | fromClub.code | Previous Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

### Destination Club fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| id | toClub.id | Destination Club UUID. | Yes | String (UUID) | 36 characters | No |
| name | toClub.name | Destination Club name. | Yes | String | Minimum 1; no maximum defined | No |
| code | toClub.code | Destination Club short code. | Yes | String | Minimum 1; no maximum defined | Yes |

## POST /api/v1/eligibility/batch

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | Yes | Boolean | N/A | No |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes |

## GET /api/v1/eligibility/club/{clubId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes |

## GET /api/v1/eligibility/team/{teamId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes |

## GET /api/v1/eligibility/region/{regionId}

### Response fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| scope | scope | Descriptor for the requested eligibility scope. | Yes | Object | N/A | No |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |

### Member Result fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| memberId | data[].memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| memberFound | data[].memberFound | Whether the requested USAV member was found. | No | Boolean | N/A | No |
| firstName | data[].firstName | Member first name. | No | String | Minimum 1; no maximum defined | No |
| lastName | data[].lastName | Member last name. | No | String | Minimum 1; no maximum defined | No |
| birthDate | data[].birthDate | Member date of birth. | No | String (date) | 10 characters | Yes |
| gender | data[].gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes |
| graduationYear | data[].graduationYear | High-school graduation year. | No | Integer | N/A | Yes |
| windowStart | data[].windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | data[].windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| roles | data[].roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |

### Scope fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| type | scope.type | Type of eligibility scope requested. | Yes | String | Maximum 6 characters | No |
| clubId | scope.clubId | Club UUID. | No | String (UUID) | 36 characters | No |
| clubName | scope.clubName | Club display name. | No | String | Minimum 1; no maximum defined | No |
| clubCode | scope.clubCode | Club Code component used in Team Code or the scoped Club code. | No | String | Minimum 1; no maximum defined | Yes |
| teamId | scope.teamId | Team UUID. | No | String (UUID) | 36 characters | No |
| teamName | scope.teamName | Team display name. | No | String | Minimum 1; no maximum defined | No |
| regionId | scope.regionId | Region UUID. | No | String (UUID) | 36 characters | No |
| regionName | scope.regionName | Region display name. | No | String | Minimum 1; no maximum defined | No |
| regionCode | scope.regionCode | Region or foreign-country component used in Team Code, or the scoped Region code. | No | String | Minimum 1; no maximum defined | Yes |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| role | data[].roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No |
| tier | data[].roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes |
| eligible | data[].roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| conditions | data[].roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| condition | data[].roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No |
| passed | data[].roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| details | data[].roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes |

## GET /api/v1/eligibility/{memberId}

### Member fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| memberId | memberId | USAV member identifier. | Yes | String | Minimum 1; no maximum defined | No |
| memberFound | memberFound | Whether the requested USAV member was found. | Yes | Boolean | N/A | No |
| firstName | firstName | Member first name. | No | String | Minimum 1; no maximum defined | No |
| lastName | lastName | Member last name. | No | String | Minimum 1; no maximum defined | No |
| birthDate | birthDate | Member date of birth. | No | String (date) | 10 characters | Yes |
| gender | gender | Member birth gender used for division and eligibility evaluation. | No | String | Maximum 6 characters | Yes |
| graduationYear | graduationYear | High-school graduation year. | No | Integer | N/A | Yes |
| windowStart | windowStart | First day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| windowEnd | windowEnd | Last day included in the eligibility evaluation window. | Yes | String (date) | 10 characters | No |
| roles | roles | Roles returned for the member or membership. | Yes | Array of objects | N/A | No |

### Role Evaluation fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| role | roles[].role | Role code evaluated for eligibility. | Yes | String | Minimum 1; no maximum defined | No |
| tier | roles[].tier | Membership tier details. | Yes | String | Minimum 1; no maximum defined | Yes |
| eligible | roles[].eligible | Whether every eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| conditions | roles[].conditions | Per-condition eligibility results for the role. | Yes | Array of objects | N/A | No |

### Eligibility Condition fields

| Field | Qualified name | Definition | Required | Data type | Length | Nullable |
| --- | --- | --- | --- | --- | --- | --- |
| condition | roles[].conditions[].condition | Eligibility condition that was evaluated. | Yes | String | Minimum 1; no maximum defined | No |
| passed | roles[].conditions[].passed | Whether the eligibility condition passed for the complete evaluation window. | Yes | Boolean | N/A | No |
| details | roles[].conditions[].details | Dates, values, or reasons supporting the eligibility-condition result. | Yes | Object | N/A | Yes |
