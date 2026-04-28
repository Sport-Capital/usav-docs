# USA Volleyball API Documentation

**Author:** Alex Wilson / USAV Engineering
**Date:** 2026-04-27
**Version:** 2.1
**Database:** CortIQ
**Status:** Current partner-facing version. Supersedes v2.0 (`api-documentation-v2.0.md`).

---

## Introduction

The USA Volleyball API provides programmatic access to volleyball organization data including clubs, teams, member profiles, memberships, credentials, waivers, suspensions, club transfers, and eligibility verification. Version 2.1 expands the v2.0 surface with twelve new endpoints, a redesigned Eligibility API that supports date-range queries and returns every role a member holds in a single call, and a new authentication model that distinguishes multi-region operator keys from single-region admin keys.

---

## Authentication

The USA Volleyball API uses bearer-token API keys to authenticate requests. v2.1 introduces a two-tier key model that distinguishes broad operator integrations from narrower region-scoped admin integrations.

### API Key Format

API keys follow the format: `usav_[64-character-hex-string]`

### Authentication Method

Include your API key in the request header:

```http
Authorization: Bearer usav_abc123...
```

### API Key Types

| Key type | Scope | Typical caller |
|---|---|---|
| **Operator** | Multi-region, read-only | Tournament operators and integrators working across regions |
| **Admin** | Single region (sub-region scoping deferred) | Club-level integrations and region-scoped tools |

Sub-region scoping (club, team, or role-level) is deferred. v2.1 scope is region-level only.

### Issuance and Rotation

USAV creates, rotates, and revokes keys in v2.1. Self-service key management is not available in this version. Partners contact USAV to request a key, request rotation, or report a suspected compromise.

Proposed rotation cadence: 2 years. Final cadence is being confirmed with USAV; partners should design integrations to allow rotation at least every 2 years.

### Security Notes

- API keys should be kept secure and never exposed in client-side code.
- Operator and Admin keys are both bearer tokens; protect them as you would a password.
- Keys can be deactivated without deletion.
- Expired keys are automatically rejected.

---

## Eligibility API

The Eligibility API answers one question: **is this person eligible to participate, and on which day or window?**

In v2.1 the Eligibility API was redesigned to be person-centric rather than role-centric. A single call returns every role the member currently holds, with per-role pass/fail details, evaluated either on a single day or across a date range. Eligibility is computed at query time against live data; results are not cached.

### Check Eligibility (single member)

Returns every current role the member holds, with eligibility evaluated over a single day or a window.

```http
GET /api/v1/eligibility/{memberId}?startDate=2026-09-15&endDate=2026-09-17
```

#### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `memberId` | integer | Yes | The USAV member ID |

#### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `startDate` | date (YYYY-MM-DD) | No | today | First day of the evaluation window |
| `endDate` | date (YYYY-MM-DD) | No | today | Last day of the evaluation window |

If only one of `startDate` or `endDate` is supplied, the other defaults to match it (single-day evaluation). The maximum window length is 90 days.

#### Response (Eligible)

```json
{
  "memberId": 12345678,
  "memberFound": true,
  "firstName": "Sarah",
  "lastName": "Johnson",
  "birthDate": "2008-05-15",
  "windowStart": "2026-09-15",
  "windowEnd": "2026-09-17",
  "roles": [
    {
      "role": "player",
      "eligible": true,
      "conditions": [
        {
          "condition": "Role status is Active",
          "passed": true,
          "details": null
        },
        {
          "condition": "Membership has started",
          "passed": true,
          "details": { "startDate": "2025-09-01" }
        },
        {
          "condition": "Membership is not expired",
          "passed": true,
          "details": { "endDate": "2026-08-31" }
        },
        {
          "condition": "Membership status is Paid",
          "passed": true,
          "details": { "status": "Paid" }
        },
        {
          "condition": "No active global suspensions",
          "passed": true,
          "details": null
        },
        {
          "condition": "No active role-specific suspensions",
          "passed": true,
          "details": null
        },
        {
          "condition": "All required credentials completed and valid",
          "passed": true,
          "details": {
            "credentials": [
              {
                "name": "SafeSport Training",
                "status": "Completed",
                "validUntil": "2026-08-10"
              }
            ]
          }
        }
      ]
    }
  ]
}
```

#### Response (Pre-purchased Membership)

A membership purchased before its `startDate` returns `eligible: false` with condition 2 failing. The `details` block carries `membership.startDate` so partners can see when the member becomes eligible:

```json
{
  "memberId": 12345678,
  "memberFound": true,
  "firstName": "Sarah",
  "lastName": "Johnson",
  "birthDate": "2008-05-15",
  "windowStart": "2026-04-27",
  "windowEnd": "2026-04-27",
  "roles": [
    {
      "role": "player",
      "eligible": false,
      "conditions": [
        {
          "condition": "Role status is Active",
          "passed": true,
          "details": null
        },
        {
          "condition": "Membership has started",
          "passed": false,
          "details": {
            "membership": { "startDate": "2026-09-01" }
          }
        },
        {
          "condition": "Membership is not expired",
          "passed": true,
          "details": { "endDate": "2027-08-31" }
        },
        {
          "condition": "Membership status is Paid",
          "passed": true,
          "details": { "status": "Paid" }
        },
        {
          "condition": "No active global suspensions",
          "passed": true,
          "details": null
        },
        {
          "condition": "No active role-specific suspensions",
          "passed": true,
          "details": null
        },
        {
          "condition": "All required credentials completed and valid",
          "passed": true,
          "details": {
            "credentials": [
              {
                "name": "SafeSport Training",
                "status": "Completed",
                "validUntil": "2027-08-10"
              }
            ]
          }
        }
      ]
    }
  ]
}
```

This is by design - the membership is valid, it has simply not begun yet. Pass `startDate = membership.startDate` (or later) to verify future eligibility.

#### Response Properties

| Property | Type | Description |
|---|---|---|
| `memberId` | integer | The USAV member ID |
| `memberFound` | boolean | True when the member exists. Always true on a 200 response (404 is returned otherwise). |
| `firstName` | string | Member's first name (for identity confirmation) |
| `lastName` | string | Member's last name (for identity confirmation) |
| `birthDate` | date | Date of birth (for identity confirmation) |
| `windowStart` | date | First day evaluated |
| `windowEnd` | date | Last day evaluated |
| `roles` | array | One entry per role the member currently holds. Includes roles on memberships whose period intersects the window AND roles on memberships with a future `startDate`. Empty array if none. |
| `roles[].role` | string | Role code (e.g., `player`, `coach`, `referee`) |
| `roles[].eligible` | boolean | True only if every condition passed on every day in the window |
| `roles[].conditions` | array | Per-condition pass/fail details |
| `roles[].conditions[].condition` | string | Condition name |
| `roles[].conditions[].passed` | boolean | Whether the condition holds for the entire window |
| `roles[].conditions[].details` | object (nullable) | Relevant dates and reasons (e.g., credential `validUntil`, membership `startDate`, suspension `endDate`). Includes `requiredDueTo` and `requirementActivatedOn` for DOB-driven credential requirements. |

#### Error Responses

| Status | Code | When |
|---|---|---|
| 400 | `INVALID_DATE_RANGE` | `startDate > endDate` or window > 90 days |
| 404 | `MEMBER_NOT_FOUND` | The supplied `memberId` does not exist |

#### Notes

- Mid-window flips (for example, a credential expiring between two days inside the window) are not surfaced as flip events in v2.1. Callers needing per-day detail should loop single-day calls or read `conditions[].details` for relevant dates such as `validUntil` and `endDate`.
- `Blocked` and `Suspended` roles appear in the `roles[]` array with `eligible: false` rather than being hidden.
- Future memberships (purchased before their `startDate`) are returned in `roles[]` with `eligible: false` and condition 2 (`Membership has started`) failing. Condition 2 `details` carries `membership.startDate` so callers can see when the member becomes eligible. Pass `startDate = membership.startDate` (or later) to verify future eligibility.

### Check Eligibility by Club

Paginated eligibility for every current member in a club, evaluated over a single day or a window.

```http
GET /api/v1/eligibility/club/{clubId}?startDate=2026-09-15&endDate=2026-09-17&role=player&eligibility=eligible
```

#### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `clubId` | string | Yes | Club UUID |

#### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `startDate` | date | No | today | First day of evaluation window |
| `endDate` | date | No | today | Last day of evaluation window |
| `role` | string | No | all | Restrict to a single role code |
| `eligibility` | string | No | all | `eligible` or `ineligible` |
| `clubAssignmentStatus` | string | No | all | `APPROVED` or `IN_TRANSFER` (narrows the population pre-filter) |
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page (max 200) |

#### Response Properties

| Property | Type | Description |
|---|---|---|
| `scope` | object | Descriptor of the call scope |
| `scope.type` | string | Always `club` |
| `scope.clubId` | string | Club UUID echoed from path |
| `scope.clubName` | string | Club display name |
| `scope.clubCode` | string | Short code |
| `windowStart` | date | First day evaluated |
| `windowEnd` | date | Last day evaluated |
| `data[]` | array | Per-member entries in the same shape as the single-member response (without `memberFound`) |
| `pagination` | object | `page`, `limit`, `total`, `totalPages`, `hasNextPage`, `hasPreviousPage` |

The maximum window length for scope-pull endpoints is 14 days. Members are pre-filtered to those with a membership intersecting the window, `clubAssignment.status IN (APPROVED, IN_TRANSFER)`, and `profile.status = active`.

### Check Eligibility by Team

```http
GET /api/v1/eligibility/team/{teamId}?startDate=2026-09-15&endDate=2026-09-17
```

Same query parameters and response shape as the club-scope endpoint, with `scope.type = "team"` and additional fields `scope.teamId`, `scope.teamName`, `scope.clubId`, `scope.clubName`.

### Check Eligibility by Region

```http
GET /api/v1/eligibility/region/{regionId}?startDate=2026-09-15&endDate=2026-09-17
```

Same query parameters and response shape as the club-scope endpoint, with `scope.type = "region"` and additional fields `scope.regionId`, `scope.regionName`, `scope.regionCode`. Regions can be 10,000+ members; pagination is required.

### Eligibility Batch

Returns eligibility for an explicit list of member IDs supplied by the caller.

```http
POST /api/v1/eligibility/batch
Content-Type: application/json
```

#### Request Body

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `memberIds` | array&lt;integer&gt; | Yes | - | List of USAV member IDs. Max 100 per request. |
| `startDate` | date (YYYY-MM-DD) | No | today | First day of evaluation window |
| `endDate` | date (YYYY-MM-DD) | No | today | Last day of evaluation window |

#### Response Properties

| Property | Type | Description |
|---|---|---|
| `windowStart` | date | First day evaluated |
| `windowEnd` | date | Last day evaluated |
| `data[]` | array | Per-member entries, ordered to match the request. Each entry uses the single-member response shape. |
| `data[].memberId` | integer | Echoed from request |
| `data[].memberFound` | boolean | False when the memberId does not exist; true otherwise |
| `data[].roles[]` | array | Populated when `memberFound` is true |

#### Error Responses

| Status | Code | When |
|---|---|---|
| 400 | `TOO_MANY_IDS` | More than 100 IDs supplied |
| 400 | `INVALID_DATE_RANGE` | `startDate > endDate` or window > 90 days |

Unknown member IDs are returned as entries with `memberFound: false` rather than as a 400.

### List Valid Roles

A lightweight reference endpoint that lists valid role codes for use with the eligibility check. The response can be cached client-side.

```http
GET /api/v1/eligibility/roles
```

#### Response

```json
{
  "data": [
    { "code": "player", "name": "Indoor Player" },
    { "code": "coach", "name": "Coach" },
    { "code": "referee", "name": "Referee" }
  ]
}
```

The full list of roles and per-role configuration is being finalized with USAV national staff (see Open Questions).

### Eligibility Conditions

Every role evaluation runs against the same condition set. All conditions must pass on every day of the requested window for the role to be eligible.

| # | Condition | Description |
|---|---|---|
| 1 | Role status is Active | The membership role must have an `Active` status |
| 2 | Membership has started | `membership.startDate <= evaluation date` |
| 3 | Membership is not expired | `membership.endDate >= evaluation date` |
| 4 | Membership status is Paid | Membership payment must be completed |
| 5 | No active global suspensions | No profile-level suspensions that affect all roles |
| 6 | No active role-specific suspensions | No suspensions targeting the specific role |
| 7 | All required credentials completed and valid | All credentials assigned to the role must be `Completed` and not expired |

The authoritative numbered list and any role-specific overrides are still being confirmed with USAV national staff (see Open Questions). The list above reflects the current implementation.

**Note:** Club affiliation is not an eligibility condition. Club assignments exist for organizational purposes but do not gate eligibility.

### Eligibility Design Principles

**Fail closed.** If any data source is unavailable, the answer is "ineligible" rather than a partial or optimistic result.

**No caching of eligibility results.** Eligibility is computed from live data on every request. Caching the output would create a window in which a suspended person could appear eligible.

**Person-centric responses.** A single call returns every role the member currently holds. Callers do not have to know the member's roles in advance.

**Date-range support.** Multi-day events (tournaments, training camps) can be evaluated in one call. Eligibility is reported as a single boolean per role across the window; condition `details` carry the dates that drive any failure.

**Suspensions override everything.** Global suspensions (affect all roles) and role-specific suspensions (affect only certain roles) both block eligibility.

---

## Core Objects

### Region

Regions are the geographic governing bodies for USA Volleyball. Clubs, members, and memberships all belong to a region. Region details appear inline on Club and Membership responses.

| Property | Type | Description |
|---|---|---|
| `id` | string | Region UUID |
| `name` | string | Region name (e.g., "Southern California") |
| `code` | string | Region short code (e.g., "SCVA") |

---

### Club

Clubs are volleyball organizations affiliated with regional governing bodies. They represent the primary organizational unit for teams and member affiliations.

#### List Clubs

```http
GET /api/v1/clubs
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page (max 100) |
| `sort` | string | No | - | Field to sort by; prefix with `-` for descending |
| `regionId` | string | No | - | Filter by region |

#### Get Club

```http
GET /api/v1/clubs/{id}
```

Returns the full Club object below. No `expand` parameter in v2.1.

| Status | Code | When |
|---|---|---|
| 404 | `CLUB_NOT_FOUND` | Club does not exist |

#### The Club Object

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Riverside Volleyball Club",
  "code": "RVC",
  "regionId": "550e8400-e29b-41d4-a716-446655440001",
  "region": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "name": "Southern California",
    "code": "SCVA"
  },
  "affiliationStatus": "Active",
  "affiliationStatusUpdatedAt": "2024-09-01T00:00:00Z",
  "type": ["Indoor", "Beach"],
  "gender": ["Male", "Female"],
  "address": {
    "street": "123 Main St",
    "addressExtra": "Suite 100",
    "city": "Riverside",
    "state": "CA",
    "zipCode": "92501"
  },
  "contact": {
    "name": "John Smith",
    "email": "john@riversidevolleyball.com",
    "phone": "+1-555-123-4567",
    "role": "Club Director"
  },
  "location": {
    "name": "Main Facility",
    "address": "123 Main St",
    "addressExtra": "Suite 100",
    "city": "Riverside",
    "state": "CA",
    "zipCode": "92501"
  },
  "logoUrl": "https://storage.example.com/logos/club123.png",
  "createdAt": "2023-01-15T10:30:00Z",
  "metadata": {
    "stripeCustomerId": "cus_abc123"
  }
}
```

#### Club Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Club UUID |
| `name` | string | Club name |
| `code` | string | Short code identifier |
| `regionId` | string | Region UUID |
| `region` | object | Region details (id, name, code) |
| `affiliationStatus` | string | `Active`, `Inactive`, `InProgress`, or `Archived` |
| `affiliationStatusUpdatedAt` | timestamp | When affiliation status was last changed |
| `type[]` | array | Volleyball types offered (`Indoor`, `Beach`, `Grass`) |
| `gender[]` | array | Gender categories served (`Male`, `Female`) |
| `address` | object | Primary club address |
| `contact` | object (nullable) | Primary contact |
| `location` | object (nullable) | Primary facility |
| `logoUrl` | string (nullable) | URL to club logo image |
| `createdAt` | timestamp | When the club was created |
| `metadata` | object | Additional metadata (e.g., `stripeCustomerId`) |

#### Affiliation Statuses

- **Active**: Club is fully affiliated and in good standing.
- **Inactive**: Club affiliation has lapsed.
- **InProgress**: Club affiliation is being processed.
- **Archived**: Club is no longer active.

---

### Club Team

Teams are competitive units within clubs, organized by age definition and gender. Teams contain member rosters.

In v2.1 the legacy `division` field is replaced by two structured fields: `ageDefinition` and `gender`. "Division" is treated as the composite display label combining the two.

#### List Teams

```http
GET /api/v1/teams
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `clubId` | string | No | all | Filter by parent club |
| `regionId` | string | No | all | Filter by region |
| `ageDefinition` | string | No | all | `7U`, `8U`, `9U`, `10U`, `11U`, `12U`, `13U`, `14U`, `15U`, `16U`, `17U`, `18U` |
| `gender` | string | No | all | `Male`, `Female`, `Coed` |
| `type` | string | No | all | `Indoor`, `Beach`, `Grass` |
| `status` | string | No | all | `active`, `inactive` |
| `sort` | string | No | - | e.g., `sort=name`, `sort=-createdAt` |

#### Get Team

```http
GET /api/v1/teams/{id}?expand=members,club
```

| Parameter | Type | Required | Description |
|---|---|---|---|
| `id` | path | Yes | Team UUID |
| `expand` | query | No | Comma list. Supported values: `members`, `club`. |

Roster is opt-in via `expand=members` to keep the default response small.

| Status | Code | When |
|---|---|---|
| 404 | `TEAM_NOT_FOUND` | Team does not exist |

#### The Club Team Object

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440010",
  "name": "Girls 16U Elite",
  "clubId": "550e8400-e29b-41d4-a716-446655440000",
  "club": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Riverside Volleyball Club",
    "code": "RVC"
  },
  "ageDefinition": "16U",
  "gender": "Female",
  "division": "Girls 16U",
  "type": "Indoor",
  "status": "active",
  "members": [
    {
      "profileId": "550e8400-e29b-41d4-a716-446655440031",
      "profile": {
        "id": "550e8400-e29b-41d4-a716-446655440031",
        "memberId": 12345678,
        "firstName": "Sarah",
        "lastName": "Johnson",
        "birthDate": "2008-05-15",
        "gender": "Female"
      },
      "role": "player",
      "joinedAt": "2024-01-10T00:00:00Z"
    }
  ],
  "createdAt": "2024-01-01T00:00:00Z"
}
```

#### Club Team Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Team UUID |
| `name` | string | Team name |
| `clubId` | string | Parent club UUID |
| `club` | object | Parent club details (full when `expand=club`) |
| `ageDefinition` | string | Age definition: `7U` through `18U` |
| `gender` | string | `Male`, `Female`, or `Coed` |
| `division` | string (derived) | Display label combining `gender` and `ageDefinition` (e.g., `"Girls 16U"`) |
| `type` | string | `Indoor`, `Beach`, or `Grass` |
| `status` | string | `active` or `inactive` |
| `members[]` | array | Roster, only when `expand=members` |
| `members[].profileId` | string | Member profile UUID |
| `members[].profile` | object | Member profile details (full when fully expanded) |
| `members[].role` | string | Per-team role: `player` or `coach` |
| `members[].joinedAt` | timestamp | When the member joined the team |
| `createdAt` | timestamp | When the team was created |

The `gender` and `ageDefinition` enum values above are pending final confirmation with USAV national staff (see Open Questions).

---

### Profile

Profiles represent individual members in the USA Volleyball system. Each profile contains personal information, demographic data, suspensions, and education information.

In v2.1 the Profile object exposes both `gender` (biological/birth gender, used for division and age-group eligibility) and `genderIdentity` (preferred display identity) as separate fields. Single-profile lookup is now available, and `graduationYear` is filterable on the list endpoint.

#### List Profiles

```http
GET /api/v1/profiles
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `sort` | string | No | - | e.g., `sort=lastName` |
| `status` | string | No | - | `active` or `inactive` |
| `regionId` | string | No | - | Filter by region |
| `graduationYear[eq]` | integer | No | - | Exact match on `education.graduationYear` |
| `graduationYear[gte]` | integer | No | - | Greater than or equal |
| `graduationYear[lte]` | integer | No | - | Less than or equal |

#### Get Profile

```http
GET /api/v1/profiles/{id}
```

Returns the full Profile object. No `expand` parameter in v2.1; fetch memberships separately via the Memberships endpoints.

| Status | Code | When |
|---|---|---|
| 404 | `PROFILE_NOT_FOUND` | Profile does not exist |

#### The Profile Object

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440031",
  "memberId": 12345678,
  "firstName": "Sarah",
  "lastName": "Johnson",
  "middleName": "Marie",
  "birthDate": "2008-05-15",
  "gender": "Female",
  "genderIdentity": "Female",
  "phoneNumber": "+1-555-987-6543",
  "contactEmail": "sarah.johnson@example.com",
  "status": "active",
  "address": {
    "id": "550e8400-e29b-41d4-a716-446655440040",
    "name": "Home",
    "street": "456 Oak Avenue",
    "addressExtra": "Apt 2B",
    "city": "Riverside",
    "state": "CA",
    "zipCode": "92501"
  },
  "guardians": [
    {
      "userId": "550e8400-e29b-41d4-a716-446655440050",
      "email": "parent@example.com",
      "isMainGuardian": true
    }
  ],
  "suspensions": [],
  "demographics": {
    "ethnicity": "Hispanic/Latino",
    "disability": false,
    "militaryService": false
  },
  "preferences": {
    "ncsaShareAgreed": true,
    "ncsaShareAgreedAt": "2023-06-15T00:00:00Z"
  },
  "education": {
    "graduationYear": 2026
  },
  "avatarUrl": "https://storage.example.com/avatars/profile123.jpg",
  "createdAt": "2023-06-01T00:00:00Z"
}
```

#### Profile Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Profile UUID |
| `memberId` | integer | Sequential member number (unique, auto-generated) |
| `firstName` | string | Member's first name |
| `lastName` | string | Member's last name |
| `middleName` | string (nullable) | Member's middle name |
| `birthDate` | date | Date of birth |
| `gender` | string | Biological / birth gender. Enum: `Male`, `Female`. Used for division and age-group eligibility. |
| `genderIdentity` | string | Preferred display gender identity. Enum: `Male`, `Female`, `Non-binary`. Used for display only. |
| `phoneNumber` | string | Contact phone number |
| `contactEmail` | string (nullable) | Primary contact email |
| `status` | string | `active` or `inactive` |
| `address` | object (nullable) | Primary address |
| `guardians[]` | array | Guardian relationships (for minors) |
| `suspensions[]` | array | Active or past suspensions (see Suspensions section) |
| `demographics` | object | Demographic information (ethnicity, disability, militaryService) |
| `preferences` | object | Member preferences and consents (e.g., NCSA share consent) |
| `education` | object | Education information |
| `education.graduationYear` | integer (nullable) | High-school graduation year. Required when the profile is created via the MMS child-add flow (Junior Athlete intake); nullable on all other profiles. |
| `avatarUrl` | string (nullable) | URL to profile avatar |
| `createdAt` | timestamp | When the profile was created |

#### Profile Statuses

- **active**: Profile is active and can participate.
- **inactive**: Profile has been deactivated.

---

### Membership

Memberships represent a member's registration for a specific season and tier. Each membership can carry multiple roles (e.g., Player, Coach, Official) with their own statuses, credentials, and waivers.

In v2.1 the Memberships List endpoint is formally documented and the Tier object is extended with `category`, `windowType`, `durationDays`, `windowStart`, and `windowEnd` to capture the different shapes of tier validity (full season, fixed duration, explicit date range).

#### List Memberships

```http
GET /api/v1/memberships
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `profileId` | string | No | - | Filter by profile UUID (all memberships for one person) |
| `regionId` | string | No | - | Filter by region UUID |
| `tierId` | string | No | - | Filter by specific tier UUID |
| `tier.category` | string | No | - | `Full` or `Limited` |
| `tier.windowType` | string | No | - | `FullSeason`, `FixedDuration`, or `DateRange` |
| `status` | string | No | - | `Paid`, `Unpaid`, `Canceled`, `Suspended`, `Expired` |
| `startDate[gte]` | date | No | - | `startDate >= value` |
| `startDate[lte]` | date | No | - | `startDate <= value` |
| `endDate[gte]` | date | No | - | `endDate >= value` |
| `endDate[lte]` | date | No | - | `endDate <= value` |
| `sort` | string | No | - | e.g., `sort=-startDate` |
| `expand` | string | No | - | Comma list: `profile`, `region`, `tier`, `roles.club` |

#### Get Membership

```http
GET /api/v1/memberships/{id}?expand=profile,region,tier,roles.club
```

#### The Membership Object

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440100",
  "profileId": "550e8400-e29b-41d4-a716-446655440031",
  "profile": {
    "id": "550e8400-e29b-41d4-a716-446655440031",
    "memberId": 12345678,
    "firstName": "Sarah",
    "lastName": "Johnson"
  },
  "userId": "550e8400-e29b-41d4-a716-446655440050",
  "regionId": "550e8400-e29b-41d4-a716-446655440001",
  "region": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "name": "Southern California",
    "code": "SCVA"
  },
  "tierId": "550e8400-e29b-41d4-a716-446655440110",
  "tier": {
    "id": "550e8400-e29b-41d4-a716-446655440110",
    "name": "Gold Membership",
    "length": "Season",
    "category": "Full",
    "windowType": "FullSeason",
    "durationDays": null,
    "windowStart": null,
    "windowEnd": null,
    "restrictions": {
      "maxRoles": 3,
      "allowedTypes": ["Indoor", "Beach"]
    }
  },
  "status": "Paid",
  "startDate": "2025-09-01",
  "endDate": "2026-08-31",
  "roles": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440120",
      "membershipRoleId": "550e8400-e29b-41d4-a716-446655440121",
      "role": {
        "id": "550e8400-e29b-41d4-a716-446655440121",
        "name": "Indoor Player"
      },
      "status": "Active",
      "clubAssignment": {
        "clubId": "550e8400-e29b-41d4-a716-446655440000",
        "club": {
          "id": "550e8400-e29b-41d4-a716-446655440000",
          "name": "Riverside Volleyball Club",
          "code": "RVC"
        },
        "status": "APPROVED",
        "assignedAt": "2025-08-15T00:00:00Z"
      },
      "eligibility": {
        "isEligible": true,
        "reasons": []
      },
      "credentials": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440130",
          "credentialId": "550e8400-e29b-41d4-a716-446655440131",
          "credential": {
            "id": "550e8400-e29b-41d4-a716-446655440131",
            "name": "SafeSport Training",
            "type": "SAFESPORT"
          },
          "status": "Completed",
          "requiredFrom": "2025-08-01T00:00:00Z",
          "completedAt": "2025-08-10T15:30:00Z",
          "validUntil": "2026-08-10T00:00:00Z"
        }
      ],
      "waivers": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440150",
          "type": "liability-2025-2026",
          "status": "Published",
          "requiredFrom": "2025-08-01T00:00:00Z",
          "signedAt": "2025-08-05T10:15:00Z",
          "validUntil": "2026-08-31T00:00:00Z",
          "version": "2"
        }
      ],
      "updatedAt": "2025-08-15T00:00:00Z"
    }
  ],
  "address": {
    "id": "550e8400-e29b-41d4-a716-446655440040",
    "name": "Home",
    "street": "456 Oak Avenue",
    "city": "Riverside",
    "state": "CA",
    "zipCode": "92501"
  },
  "payment": {
    "amount": 299.99,
    "currency": "USD",
    "method": "card",
    "transactionId": "pi_abc123"
  },
  "createdAt": "2025-08-01T00:00:00Z"
}
```

#### Membership Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Membership UUID |
| `profileId` | string | Profile UUID |
| `profile` | object | Basic profile information |
| `userId` | string | User who created/owns the membership |
| `regionId` | string | Region UUID |
| `region` | object | Region details |
| `tierId` | string | Tier UUID |
| `tier` | object | Tier details (see Tier Properties) |
| `status` | string | `Paid`, `Unpaid`, `Canceled`, `Suspended`, or `Expired` |
| `startDate` | date | When the membership starts |
| `endDate` | date | When the membership ends |
| `roles[]` | array | Membership roles assigned |
| `address` | object | Address associated with this membership |
| `payment` | object (nullable) | Payment information |
| `createdAt` | timestamp | When the membership was created |

#### Tier Properties

| Property | Type | Description |
|---|---|---|
| `tier.id` | string | Tier UUID |
| `tier.name` | string | Display name |
| `tier.length` | string | Human-readable duration (e.g., "Season", "1 Year", "1 Day") |
| `tier.category` | string | `Full` or `Limited` |
| `tier.windowType` | string | `FullSeason`, `FixedDuration`, or `DateRange` |
| `tier.durationDays` | integer (nullable) | Populated when `windowType = FixedDuration` (e.g., 1 for a one-day pass, 365 for a 1-year pass) |
| `tier.windowStart` | date (nullable) | Populated when `windowType = DateRange` |
| `tier.windowEnd` | date (nullable) | Populated when `windowType = DateRange` |
| `tier.restrictions` | object | `maxRoles`, `allowedTypes` |

#### Tier Window Types

- **FullSeason**: Tied to the USAV competition season. Indoor memberships use this pattern.
- **FixedDuration**: A fixed number of days from the membership's `startDate`. `tier.durationDays` carries the length. One-day memberships are `FixedDuration` with `durationDays = 1`.
- **DateRange**: Explicit `tier.windowStart` and `tier.windowEnd` set on the tier itself.

Regardless of `windowType`, the eligibility check evaluates against the membership's `startDate` and `endDate`.

#### Membership Statuses

- **Paid**: Membership has been paid and is active.
- **Unpaid**: Membership created but payment pending.
- **Canceled**: Membership has been canceled.
- **Suspended**: Membership is temporarily suspended.
- **Expired**: Membership period has ended.

#### Membership Role Properties

Each role within a membership represents a specific participation type.

| Property | Type | Description |
|---|---|---|
| `id` | string | Role assignment UUID |
| `membershipRoleId` | string | Role definition UUID |
| `role` | object | Role definition (id, name) |
| `status` | string | `Active`, `Pending`, `Blocked`, `Suspended`, `Expired`, `Canceled` |
| `clubAssignment` | object (nullable) | `clubId`, `status` (`APPROVED`, `REJECTED`, `IN_TRANSFER`), `assignedAt` |
| `eligibility` | object | Computed eligibility (`isEligible`, `reasons[]`); may drift, use the Eligibility API for live answers |
| `credentials[]` | array | Per-role credential requirements (see Credential Properties) |
| `waivers[]` | array | Per-role waivers (see Waiver Properties) |
| `updatedAt` | timestamp | When the role was last updated |

#### Membership Role Statuses

- **Active**: Role is active and the member can participate.
- **Pending**: Role is awaiting approval or requirements.
- **Blocked**: Role is blocked due to unmet requirements.
- **Suspended**: Role is temporarily suspended.
- **Expired**: Role has expired.
- **Canceled**: Role has been canceled.

---

### Credentials

Credentials represent required certifications, training courses, and background checks. Credentials remain embedded inside Membership Role responses, and v2.1 also exposes a standalone list endpoint for compliance dashboards and expiry watches.

#### List Credentials

```http
GET /api/v1/credentials
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `profileId` | string | No | all | Filter by profile UUID |
| `memberId` | integer | No | all | Filter by USAV member ID |
| `type` | string | No | all | `SAFESPORT` or `LITMOS` |
| `status` | string | No | all | `NotStarted`, `InProgress`, `Completed`, `Expired`, `Failed` |
| `validUntil[gte]` | date | No | - | Range filter |
| `validUntil[lte]` | date | No | - | Range filter |
| `sort` | string | No | - | e.g., `sort=validUntil` |

#### Credential Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Credential assignment UUID |
| `profileId` | string | Owning profile |
| `memberId` | integer | USAV member ID |
| `credentialId` | string | Credential definition UUID |
| `credential` | object | Credential details |
| `credential.name` | string | Credential name |
| `credential.type` | string | `SAFESPORT` or `LITMOS` |
| `status` | string | `NotStarted`, `InProgress`, `Completed`, `Expired`, `Failed` |
| `requiredFrom` | timestamp | When the credential becomes required for this member |
| `completedAt` | timestamp (nullable) | When completed |
| `validUntil` | timestamp (nullable) | When the credential expires |

#### Credential Types

- **SAFESPORT**: SafeSport training requirements.
- **LITMOS**: Training courses delivered via the Litmos LMS.

#### Credential Statuses

- **NotStarted**: Requirement not yet begun.
- **InProgress**: Member is working on the credential.
- **Completed**: Credential successfully completed.
- **Expired**: Previously completed credential has expired.
- **Failed**: Credential attempt failed.

---

### Waivers

Waivers are documents members must acknowledge for a given membership role. Waivers remain embedded inside Membership Role responses, and v2.1 also exposes a standalone list endpoint.

The Waiver schema is intentionally simple in v2.1: there is no multi-state status enum from the API perspective. Waiver documents are exposed in the `Published` state; whether a member has signed a specific waiver is inferred from `signedAt` (non-null = signed).

#### List Waivers

```http
GET /api/v1/waivers
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `profileId` | string | No | all | Filter by profile UUID |
| `memberId` | integer | No | all | Filter by USAV member ID |
| `type` | string | No | all | Waiver type identifier |
| `status` | string | No | all | Always `Published` (filtering by status is effectively a no-op until additional states are introduced) |
| `validUntil[gte]` | date | No | - | Range filter |
| `validUntil[lte]` | date | No | - | Range filter |
| `sort` | string | No | - | e.g., `sort=validUntil` |

#### Waiver Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Waiver UUID |
| `profileId` | string | Owning profile |
| `memberId` | integer | USAV member ID |
| `type` | string | Waiver type identifier |
| `status` | string | Always `Published` from the API perspective in v2.1 |
| `requiredFrom` | timestamp | When the waiver became required |
| `signedAt` | timestamp (nullable) | When the member signed (null = not signed) |
| `validUntil` | timestamp (nullable) | Expiry |
| `version` | string (nullable) | Waiver version signed |

Whether an unsigned waiver should block eligibility is an open question (see Open Questions). v2.1 does not include a waiver condition in the Eligibility API; if that resolves to "yes," a future revision will add one.

---

### Suspensions

Suspensions can be global (affect all roles) or role-specific. Suspensions remain readable inline on Profile responses, and v2.1 exposes a standalone list endpoint for audit and compliance use.

#### List Suspensions

```http
GET /api/v1/suspensions
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `profileId` | string | No | all | Filter by profile UUID |
| `memberId` | integer | No | all | Filter by USAV member ID |
| `scope` | string | No | all | `global` or `role-specific` |
| `active` | boolean | No | all | `true` returns suspensions in effect now; `false` returns resolved or upcoming |
| `startDate[gte]` | date | No | - | Range filter |
| `startDate[lte]` | date | No | - | Range filter |
| `endDate[gte]` | date | No | - | Range filter |
| `endDate[lte]` | date | No | - | Range filter |
| `sort` | string | No | - | e.g., `sort=-startDate` |

#### Suspension Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Suspension UUID |
| `profileId` | string | Affected profile |
| `memberId` | integer | USAV member ID |
| `scope` | string | `global` or `role-specific` (computed from `affectedRoles`) |
| `affectedRoles[]` | array (nullable) | Role codes affected. Null or empty = global suspension. |
| `reason` | string | Short reason |
| `startDate` | timestamp | When the suspension takes effect |
| `endDate` | timestamp (nullable) | When the suspension ends. Null = indefinite. |
| `createdAt` | timestamp | When the suspension was created |

`active` is computed as `startDate <= now AND (endDate IS NULL OR endDate >= now)`.

---

### Club Transfers

Club transfers represent the movement of a member between two clubs. v2.1 exposes a list endpoint and four new webhook events so that downstream systems can react to transfer state changes rather than polling.

#### List Club Transfers

```http
GET /api/v1/club-transfers
```

##### Query Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `page` | integer | No | 1 | Page number (1-indexed) |
| `limit` | integer | No | 50 | Items per page |
| `profileId` | string | No | all | Filter by member profile UUID |
| `fromClubId` | string | No | all | Filter by source club UUID |
| `toClubId` | string | No | all | Filter by destination club UUID |
| `status` | string | No | all | `Approved`, `Rejected`, or `InTransfer` |
| `requestedAt[gte]` | date | No | - | Range filter |
| `requestedAt[lte]` | date | No | - | Range filter |
| `sort` | string | No | - | e.g., `sort=-requestedAt` |

#### Club Transfer Properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Transfer UUID |
| `profileId` | string | Member being transferred |
| `profile` | object | Member profile (when expanded) |
| `fromClubId` | string | Source club UUID |
| `fromClub` | object | Source club details |
| `toClubId` | string | Destination club UUID |
| `toClub` | object | Destination club details |
| `status` | string | `Approved`, `Rejected`, or `InTransfer` |
| `requestedAt` | timestamp | When the transfer was initiated |
| `completedAt` | timestamp (nullable) | When the transfer resolved |
| `requestedBy` | string | User ID that initiated the transfer |

**Transfer SLA:** Transfers are expected to be processed within a small number of minutes of approval. The exact SLA is being finalized with USAV. Tournament operators relying on current club assignment should subscribe to `club.transfer.completed` rather than poll.

---

## Data Relationships

### Entity Relationship Overview

The diagram below shows the full data model. Entities marked with `*` are read by the Eligibility API to compute real-time eligibility.

```
Region
  ├─ Clubs
  │   ├─ Teams
  │   │   └─ Team Members (Profiles)
  │   └─ Club Contacts
  └─ Memberships *
      ├─ Profile *
      │   ├─ Guardians (Users)
      │   ├─ Addresses
      │   └─ Suspensions *
      └─ Membership Roles *
          ├─ Club Assignment
          ├─ Credentials *
          └─ Waivers
```

### Key Relationships

1. **Profiles and Guardians**: Minors can have multiple guardians with varying permission levels.
2. **Memberships and Profiles**: One profile can have multiple memberships across seasons.
3. **Membership Roles and Clubs**: Roles can be assigned to clubs for organizational purposes; club assignment does not affect eligibility.
4. **Teams and Profiles**: Profiles join teams through their active club affiliations.
5. **Credentials and Roles**: Different roles require different credentials. Credential pass/fail status surfaces in the Eligibility API response.
6. **Suspensions and Roles**: Suspensions can be global or role-specific. Both block eligibility.
7. **Club Transfers and Roles**: A transfer is reflected on the membership role's `clubAssignment` (the assignment status moves to `IN_TRANSFER` and then to `APPROVED` against the new club).

---

## Common Use Cases

### Verifying Member Eligibility

Use the Eligibility API. A single call returns every role the member currently holds, with full per-condition detail, evaluated either on a single day or across a window:

```http
GET /api/v1/eligibility/12345678?startDate=2026-09-15&endDate=2026-09-17
```

For pre-registration eligibility checks, pass a future window. For audit purposes, pass a past window.

### Pulling a Club Roster With Eligibility

Use the club-scope endpoint to retrieve every current member of a club with eligibility evaluated in one paginated response:

```http
GET /api/v1/eligibility/club/{clubId}?startDate=2026-09-15&endDate=2026-09-17&eligibility=ineligible
```

Apply `role`, `eligibility`, or `clubAssignmentStatus` filters to narrow the population.

### Pulling a Tournament Region's Roster

Region-scope eligibility is paginated to handle 10,000+ members:

```http
GET /api/v1/eligibility/region/{regionId}?startDate=2026-09-15&endDate=2026-09-17&page=1&limit=200
```

### Tracking Credential Compliance

Use the standalone Credentials list endpoint, or read credentials embedded inside Membership Role responses for inline use:

```http
GET /api/v1/credentials?type=SAFESPORT&status=Expired
GET /api/v1/credentials?validUntil[lte]=2026-12-31&status=Completed   # expiring this year
```

### Watching for Club Transfers in Tournament Windows

Subscribe to the `club.transfer.completed` webhook event so that tournament tools react to transfer changes immediately, rather than polling the list endpoint.

---

## Open Questions

These items are still being finalized with USAV national staff and partners. They are tracked here so partners can see what may evolve in subsequent revisions.

| ID | Question | Impact |
|---|---|---|
| OQ-01 | What is the full list of roles and their per-role eligibility configuration? | The condition set above is the current implementation; additional roles or per-role overrides may be added. |
| OQ-02 | Do any credential types constitute a hard eligibility requirement beyond what is already enforced? | The current design treats credentials assigned to a role as blocking conditions. Confirmation pending. |
| OQ-03 | Do unsigned waivers block eligibility? | Waivers are present in the data model but are not currently included as an eligibility condition. If confirmed as blocking, they will be added. |
| OQ-04 | What is the authoritative numbered list of the 7 conditions? | The list above reflects the implementation; the canonical wording is being confirmed with USAV national staff. |
| OQ-05 | Final enum values for Team `gender` and `ageDefinition`. | Current values (`Male`, `Female`, `Coed`; `7U`-`18U`) are pending USAV confirmation. |
| OQ-06 | Final Club Transfer SLA value. | A small-minutes SLA is being finalized; partners can subscribe to `club.transfer.completed` in the meantime. |
| OQ-07 | Final API key rotation cadence. | Proposed at 2 years, pending confirmation. |

---

## Errors

The API uses conventional HTTP response codes:

| Code | Meaning |
|---|---|
| 200 | Success |
| 400 | Bad Request - invalid parameters |
| 401 | Unauthorized - invalid or missing API key |
| 403 | Forbidden - API key lacks required permissions |
| 404 | Not Found - resource does not exist |
| 429 | Too Many Requests - rate limit exceeded |
| 500 | Internal Server Error |

### Error Response Format

```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid membership ID provided",
    "param": "membershipId",
    "code": "resource_not_found"
  }
}
```

### Eligibility-Specific Error Codes

| Code | Status | When |
|---|---|---|
| `MEMBER_NOT_FOUND` | 404 | The supplied `memberId` does not exist |
| `CLUB_NOT_FOUND` | 404 | The supplied `clubId` does not exist |
| `TEAM_NOT_FOUND` | 404 | The supplied `teamId` does not exist |
| `REGION_NOT_FOUND` | 404 | The supplied `regionId` does not exist |
| `INVALID_DATE_RANGE` | 400 | `startDate > endDate` or window exceeds the maximum |
| `TOO_MANY_IDS` | 400 | Batch request supplied more than 100 IDs |

---

## Versioning

The API is versioned through the URL path. The current version is `v1`. v2.1 of this documentation describes the surface of the `v1` API as it stands at this revision. Breaking changes will be introduced in a new URL version (e.g., `v2`); non-breaking changes may be added to existing versions.

---

## Pagination

List endpoints support page-based pagination:

```http
GET /api/v1/clubs?page=1&limit=100
```

Response includes pagination metadata:

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 100,
    "total": 1250,
    "totalPages": 13,
    "hasNextPage": true,
    "hasPreviousPage": false
  }
}
```

### Pagination Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `page` | integer | 1 | Page number (1-indexed) |
| `limit` | integer | 50 | Number of items per page (max 100 for most endpoints; max 200 for eligibility scope-pulls) |

---

## Filtering and Sorting

List endpoints support filtering through query parameters:

```http
GET /api/v1/profiles?status=active&regionId=550e8400-e29b-41d4-a716-446655440001
GET /api/v1/memberships?status=Paid&startDate[gte]=2025-01-01&tier.windowType=FullSeason
```

Range comparison operators (`[gte]`, `[lte]`, `[eq]`) are supported on date and integer fields where indicated.

Sorting is available via the `sort` parameter:

```http
GET /api/v1/clubs?sort=-createdAt   # descending by creation date
GET /api/v1/profiles?sort=lastName  # ascending by last name
```

---

## Expanding Related Objects

By default, related objects are returned as IDs. Use the `expand` parameter to include full object details where supported:

```http
GET /api/v1/memberships/123?expand=profile,region,tier,roles.club
GET /api/v1/teams/456?expand=members,club
```

This reduces the number of round-trips needed to assemble a complete view.

---

## Webhooks

Webhooks deliver real-time notifications for entity changes. Webhook subscriptions are managed by USAV in v2.1; partners contact USAV with a callback URL and the event types they want, and USAV registers the subscription and returns a shared secret. Self-service subscription management may be added in a future revision.

### Supported Events

| Event | Entity | Fires when |
|---|---|---|
| `membership.created` | Membership | A new membership is created |
| `membership.updated` | Membership | Any membership field changes (high volume) |
| `membership.paid` | Membership | Payment settles - key signal for an eligibility flip |
| `profile.created` | Profile | A new profile is created |
| `profile.updated` | Profile | Any profile field changes |
| `profile.suspended` | Profile | A suspension is added to a profile |
| `club.affiliated` | Club | Club moves to `Active` affiliation |
| `club.disaffiliated` | Club | Club moves off `Active` |
| `credential.completed` | Credential | A required credential is completed |
| `credential.expired` | Credential | A credential passes its `validUntil` |
| `club.transfer.initiated` | Club Transfer | A member transfer is requested |
| `club.transfer.approved` | Club Transfer | A transfer is approved |
| `club.transfer.rejected` | Club Transfer | A transfer is denied |
| `club.transfer.completed` | Club Transfer | A transfer is fully processed |

### Webhook Payload

```json
{
  "id": "evt_abc123",
  "type": "membership.paid",
  "created": "2025-08-15T10:30:00Z",
  "data": {
    "object": {
      "id": "550e8400-e29b-41d4-a716-446655440100",
      "profileId": "550e8400-e29b-41d4-a716-446655440031",
      "status": "Paid"
    }
  }
}
```

### Webhook Security

Each webhook subscription has a shared secret issued by USAV when the subscription is registered. Webhook requests carry the shared secret in the `Authorization` header so the receiver can verify the request is from USAV:

```http
Authorization: Bearer usav_abc123...
```

To verify a webhook request:

1. Extract the bearer token from the `Authorization` header.
2. Compare it against the shared secret you received when the subscription was registered.
3. Validate the destination URL matches your registered callback URL.

Store the shared secret securely. If a secret is compromised, contact USAV immediately to rotate the subscription.
