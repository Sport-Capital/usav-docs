# USA Volleyball API Documentation

```
This document is a draft and is subject to change. Please use it only as a work-in-progress reference. Access rights will be managed via API keys, and group access will be subject to limitations established by USA Volleyball and its partners.
```

## Introduction

The USA Volleyball API provides programmatic access to volleyball organization data including clubs, teams, member profiles, and memberships. This API enables partners to integrate USA Volleyball data into their applications and services.

## Authentication

The USA Volleyball API uses API keys to authenticate requests. API keys are scoped to specific regions and can be configured with different access levels.

### API Key Format

API keys follow the format: `usav_[64-character-hex-string]`

### Authentication Method

Include your API key in the request header:

```http
Authorization: Bearer usav_abc123...
```

### API Key Types

- **API_KEY**: Standard read-only access to retrieve data
- **WEBHOOK**: Special keys for webhook integrations with callback URLs

### API Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for the API key |
| `name` | string | Descriptive name for the key |
| `type` | string | Type of API key (API_KEY or WEBHOOK) |
| `isActive` | boolean | Whether the key is currently active |
| `createdAt` | timestamp | When the key was created |
| `webhookUrl` | string (nullable) | Callback URL for webhook-type keys |

### Security Notes

- API keys should be kept secure and never exposed in client-side code
- Keys can be scoped to specific regions for data isolation
- Keys can be deactivated without deletion
- Expired keys are automatically rejected

---

## Core Objects

### Club

Clubs are volleyball organizations affiliated with regional governing bodies. They represent the primary organizational unit for teams and member affiliations.

#### The Club Object

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Riverside Volleyball Club",
  "code": "RVSDE",
  "regionId": "550e8400-e29b-41d4-a716-446655440001",
  "region": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "name": "Southern California",
    "code": "SC"
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
|----------|------|-------------|
| `id` | string | Unique identifier for the club |
| `name` | string | Club name |
| `code` | string | Short code identifier for the club (5 characters) |
| `regionId` | string | ID of the region this club belongs to |
| `region` | object | Region details |
| `region.id` | string | Region ID |
| `region.name` | string | Region name |
| `region.code` | string | Region code (2 characters) |
| `affiliationStatus` | string | Current affiliation status: `Active`, `Inactive`, `InProgress`, `Archived` |
| `affiliationStatusUpdatedAt` | timestamp | When affiliation status was last changed |
| `type` | array | Types of volleyball offered (e.g., "Indoor", "Beach", "Grass") |
| `gender` | array | Gender categories served (e.g., "Male", "Female") |
| `address` | object | Primary club address |
| `address.street` | string | Street address |
| `address.addressExtra` | string (nullable) | Additional address information |
| `address.city` | string | City |
| `address.state` | string | State/province code |
| `address.zipCode` | string | Postal code |
| `contact` | object (nullable) | Primary contact information |
| `contact.name` | string | Contact person name |
| `contact.email` | string | Contact email |
| `contact.phone` | string (nullable) | Contact phone number |
| `contact.role` | string (nullable) | Contact's role at the club |
| `location` | object (nullable) | Primary facility location |
| `logoUrl` | string (nullable) | URL to club logo image |
| `createdAt` | timestamp | When the club was created |
| `metadata` | object | Additional metadata (Stripe ID, etc.) |

#### Affiliation Statuses

- **Active**: Club is fully affiliated and in good standing
- **Inactive**: Club affiliation has lapsed
- **InProgress**: Club affiliation is being processed
- **Archived**: Club is no longer active

---

### Club Team

Teams are competitive units within clubs, organized by division and gender. Teams contain member rosters.

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
  "divisionId": "550e8400-e29b-41d4-a716-446655440020",
  "division": {
    "id": "550e8400-e29b-41d4-a716-446655440020",
    "name": "16 and Under",
    "maxAge": 16
  },
  "gender": "Female",
  "members": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440030",
      "profileId": "550e8400-e29b-41d4-a716-446655440031",
      "profile": {
        "id": "550e8400-e29b-41d4-a716-446655440031",
        "memberId": 12345678,
        "firstName": "Sarah",
        "lastName": "Johnson",
        "birthDate": "2008-05-15",
        "gender": "Female"
      },
      "joinedAt": "2024-01-10T00:00:00Z"
    }
  ],
  "createdAt": "2024-01-01T00:00:00Z"
}
```

#### Club Team Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for the team |
| `name` | string | Team name |
| `clubId` | string | ID of the parent club |
| `club` | object | Abbreviated club information |
| `divisionId` | string | ID of the age division |
| `division` | object | Division details including name and max age |
| `gender` | string | Team gender category |
| `members` | array | Array of team member objects |
| `members[].id` | string | Team membership ID |
| `members[].profileId` | string | Profile ID of the member |
| `members[].profile` | object | Basic member profile information |
| `members[].joinedAt` | timestamp | When member joined the team |
| `createdAt` | timestamp | When the team was created |

---

### Profile

Profiles represent individual members in the USA Volleyball system. Each profile contains personal information, membership history, and relationships to guardians.

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
  "suspensions": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440060",
      "type": "Disciplinary",
      "startDate": "2024-03-01T00:00:00Z",
      "endDate": "2024-06-01T00:00:00Z",
      "comment": "Code of conduct violation",
      "affectedRoles": [
        {
          "membershipMembershipRoleId": "550e8400-e29b-41d4-a716-446655440070",
          "roleName": "Player"
        }
      ],
      "createdAt": "2024-02-28T00:00:00Z",
      "createdBy": {
        "id": "550e8400-e29b-41d4-a716-446655440080",
        "email": "admin@usavolleyball.org"
      }
    }
  ],
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
|----------|------|-------------|
| `id` | string | Unique identifier for the profile |
| `memberId` | integer | Sequential member number (unique, auto-generated) |
| `firstName` | string | Member's first name |
| `lastName` | string | Member's last name |
| `middleName` | string (nullable) | Member's middle name |
| `birthDate` | date | Date of birth |
| `gender` | string | Gender identity |
| `phoneNumber` | string | Contact phone number |
| `contactEmail` | string (nullable) | Primary contact email |
| `status` | string | Profile status: `active` or `inactive` |
| `address` | object (nullable) | Primary address |
| `guardians` | array | Array of guardian relationships (for minors) |
| `guardians[].userId` | string | User ID of the guardian |
| `guardians[].email` | string | Guardian's email |
| `guardians[].isMainGuardian` | boolean | Whether this is the primary guardian |
| `suspensions` | array | Array of active or past suspensions |
| `suspensions[].id` | string | Suspension ID |
| `suspensions[].type` | string | Type of suspension |
| `suspensions[].startDate` | timestamp | When suspension starts/started |
| `suspensions[].endDate` | timestamp (nullable) | When suspension ends (null = indefinite) |
| `suspensions[].comment` | string (nullable) | Notes about the suspension |
| `suspensions[].affectedRoles` | array (nullable) | Specific membership roles affected |
| `suspensions[].createdAt` | timestamp | When suspension was created |
| `suspensions[].createdBy` | object | Admin who created the suspension |
| `demographics` | object | Demographic information |
| `demographics.ethnicity` | string (nullable) | Ethnicity |
| `demographics.disability` | boolean (nullable) | Has a disability |
| `demographics.militaryService` | boolean (nullable) | Military service status |
| `preferences` | object | Member preferences and consents |
| `education` | object | Education information |
| `education.graduationYear` | integer (nullable) | Expected graduation year |
| `avatarUrl` | string (nullable) | URL to profile avatar image |
| `createdAt` | timestamp | When the profile was created |

#### Profile Statuses

- **active**: Profile is active and can participate
- **inactive**: Profile has been deactivated

#### Suspension Types

Suspensions can be applied at different scopes:
- **Global**: Affects all membership roles
- **Role-specific**: Affects specific membership roles only

When a suspension has `affectedRoles`, it only impacts those specific roles. When `affectedRoles` is empty or null, the suspension is global.

---

### Membership

Memberships represent a member's registration for a specific season and tier. Each membership can have multiple roles (e.g., Player, Coach, Official) with different requirements and statuses.

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
    "code": "SC"
  },
  "tierId": "550e8400-e29b-41d4-a716-446655440110",
  "tier": {
    "id": "550e8400-e29b-41d4-a716-446655440110",
    "name": "Gold Membership",
    "length": 12,
    "restrictions": {
      "maxRoles": 3,
      "allowedTypes": ["Indoor", "Beach"]
    }
  },
  "status": "Paid",
  "startDate": "2024-09-01",
  "endDate": "2025-08-31",
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
        "assignedAt": "2024-08-15T00:00:00Z"
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
          "requiredFrom": "2024-08-01T00:00:00Z",
          "completedAt": "2024-08-10T15:30:00Z",
          "validUntil": "2025-08-10T00:00:00Z",
          "description": "Annual SafeSport training requirement"
        },
        {
          "id": "550e8400-e29b-41d4-a716-446655440140",
          "credentialId": "550e8400-e29b-41d4-a716-446655440141",
          "credential": {
            "id": "550e8400-e29b-41d4-a716-446655440141",
            "name": "Impact Concussion Training",
            "type": "LITMOS"
          },
          "status": "InProgress",
          "requiredFrom": "2024-08-01T00:00:00Z",
          "completedAt": null,
          "validUntil": null,
          "description": "Required concussion awareness course"
        }
      ],
      "waivers": [
        {
          "id": "550e8400-e29b-41d4-a716-446655440150",
          "waiverVersionId": "550e8400-e29b-41d4-a716-446655440151",
          "waiver": {
            "id": "550e8400-e29b-41d4-a716-446655440152",
            "name": "2024-2025 Liability Waiver",
            "version": 2
          },
          "status": "Signed",
          "signedAt": "2024-08-05T10:15:00Z"
        }
      ],
      "updatedAt": "2024-08-15T00:00:00Z"
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
  "createdAt": "2024-08-01T00:00:00Z"
}
```

#### Membership Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for the membership |
| `profileId` | string | ID of the member's profile |
| `profile` | object | Basic profile information |
| `userId` | string | ID of the user who created/owns the membership |
| `regionId` | string | ID of the region for this membership |
| `region` | object | Region details |
| `tierId` | string | ID of the membership tier |
| `tier` | object | Tier details including name, length, and restrictions |
| `status` | string | Membership status: `Paid`, `Unpaid`, `Canceled`, `Suspended`, `Expired` |
| `startDate` | date | When membership starts |
| `endDate` | date | When membership ends |
| `roles` | array | Array of membership roles assigned to this membership |
| `address` | object | Address associated with this membership |
| `payment` | object (nullable) | Payment information |
| `createdAt` | timestamp | When the membership was created |

#### Membership Statuses

- **Paid**: Membership has been paid and is active
- **Unpaid**: Membership created but payment pending
- **Canceled**: Membership has been canceled
- **Suspended**: Membership is temporarily suspended
- **Expired**: Membership period has ended

#### Membership Role Properties

Each role within a membership represents a specific participation type (e.g., Player, Coach, Official).

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for this membership role assignment |
| `membershipRoleId` | string | ID of the role definition |
| `role` | object | Role definition including name and requirements |
| `status` | string | Role status: `Active`, `Expired`, `Canceled`, `Suspended`, `Pending`, `Blocked` |
| `clubAssignment` | object (nullable) | Club affiliation for this role |
| `clubAssignment.status` | string | Assignment status: `APPROVED`, `REJECTED`, `IN_TRANSFER` |
| `eligibility` | object | Computed eligibility status |
| `eligibility.isEligible` | boolean | Whether role is currently eligible |
| `eligibility.reasons` | array | Array of reasons if not eligible |
| `credentials` | array | Required credentials/certifications for this role |
| `waivers` | array | Required waivers for this role |
| `updatedAt` | timestamp | When the role was last updated |

#### Membership Role Statuses

- **Active**: Role is active and member can participate
- **Pending**: Role is awaiting approval or requirements
- **Blocked**: Role is blocked due to unmet requirements
- **Suspended**: Role is temporarily suspended
- **Expired**: Role has expired
- **Canceled**: Role has been canceled

#### Eligibility

A membership role is considered eligible when ALL of the following conditions are met:

1. Role status is `Active`
2. If club affiliation is required, a club must be assigned and approved
3. Membership must have started (startDate <= current date)
4. Membership must not be expired (endDate >= current date)
5. Membership status must be `Paid`
6. No active global suspensions on the profile
7. No active role-specific suspensions
8. All required credentials must be completed and valid

When `eligibility.isEligible` is `false`, the `eligibility.reasons` array contains specific reasons such as:
- `"Club affiliation required"`
- `"Membership not yet started"`
- `"Membership expired"`
- `"Payment required"`
- `"Profile suspended"`
- `"Required credential incomplete: [credential name]"`

#### Credential Properties

Credentials represent required certifications, training courses, or background checks.

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for this credential assignment |
| `credentialId` | string | ID of the credential definition |
| `credential` | object | Credential details |
| `credential.type` | string | Type: `SAFESPORT`, `LITMOS` |
| `status` | string | Status: `NotStarted`, `InProgress`, `Completed`, `Expired`, `Failed` |
| `requiredFrom` | timestamp | When this credential becomes required |
| `completedAt` | timestamp (nullable) | When credential was completed |
| `validUntil` | timestamp (nullable) | When credential expires |
| `description` | string (nullable) | Description of the requirement |
| `statusDetails` | object (nullable) | Additional status information |

#### Credential Types

- **SAFESPORT**: SafeSport training requirements
- **LITMOS**: Training courses delivered via Litmos LMS

#### Credential Statuses

- **NotStarted**: Credential requirement not yet begun
- **InProgress**: Member is working on completing the credential
- **Completed**: Credential successfully completed
- **Expired**: Previously completed credential has expired
- **Failed**: Credential attempt failed

---

## Data Relationships

### Entity Relationship Overview

![Entity Relationship Overview](https://i.postimg.cc/QMtybq01/Zrzut-ekranu-2026-01-8-o-15-04-08.png)

## Common Use Cases

### Verifying Member Eligibility

To verify if a member is eligible to participate:

1. Retrieve the member's profile
2. Get their active membership for the current season
3. Check each membership role for:
   - `status` is `Active`
   - `eligibility.isEligible` is `true`
   - All `credentials` have `status` of `Completed`
   - No active `suspensions` affecting the role

### Getting Team Rosters

To get a complete team roster with member details:

1. Retrieve the team by ID
2. The response includes all team members with embedded profile information
3. Optionally cross-reference with memberships to verify current eligibility

### Tracking Credential Compliance

To monitor credential completion:

1. Retrieve memberships for a date range
2. Filter `roles` to the desired role type
3. Check `credentials` array for each role
4. Look for credentials where `status` is not `Completed` and `requiredFrom` has passed

### Managing Club Transfers

When a member transfers between clubs:

1. The transfer creates a record tracking:
   - Source club (`fromClubId`)
   - Destination club (`toClubId`)
   - Approval status from both clubs
2. Status values:
   - `Approved`: Both clubs approved
   - `Rejected`: One or both clubs rejected
   - `InTransfer`: Transfer pending approval

---

## Errors

The API uses conventional HTTP response codes:

| Code | Meaning |
|------|---------|
| 200 | Success |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid or missing API key |
| 403 | Forbidden - API key lacks required permissions |
| 404 | Not Found - Resource doesn't exist |
| 429 | Too Many Requests - Rate limit exceeded |
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

---

## Versioning

The API is versioned through the URL path. The current version is `v1`.

Breaking changes will be introduced in new versions (e.g., `v2`). Non-breaking changes may be added to existing versions.

---

## Pagination

List endpoints support page-based pagination:

```http
GET /api/v1/clubs?page=1&limit=100
```

Response includes pagination metadata:

```json
{
  "data": [...],
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
|-----------|------|---------|-------------|
| `page` | integer | 1 | Page number (1-indexed) |
| `limit` | integer | 50 | Number of items per page (max: 100) |

---

## Filtering and Sorting

List endpoints support filtering through query parameters:

```http
GET /api/v1/profiles?status=active&regionId=550e8400-e29b-41d4-a716-446655440001
GET /api/v1/memberships?status=Paid&startDate[gte]=2024-01-01
```

Sorting is available via the `sort` parameter:

```http
GET /api/v1/clubs?sort=-createdAt  # Descending by creation date
GET /api/v1/profiles?sort=lastName  # Ascending by last name
```

---

## Expanding Related Objects

By default, related objects return only IDs. Use the `expand` parameter to include full object details:

```http
GET /api/v1/memberships/123?expand=profile,region,tier,roles.club
```

This reduces the number of API calls needed to fetch related data.

---

## Webhooks

Webhook-type API keys can receive real-time notifications about events:

### Supported Events

- `membership.created`
- `membership.updated`
- `membership.paid`
- `profile.created`
- `profile.updated`
- `profile.suspended`
- `club.affiliated`
- `club.disaffiliated`
- `credential.completed`
- `credential.expired`

### Webhook Payload

```json
{
  "id": "evt_abc123",
  "type": "membership.paid",
  "created": "2024-01-15T10:30:00Z",
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

Webhook requests include the API key in the Authorization header for verification:

```http
Authorization: Bearer usav_abc123...
```

To verify a webhook request:

1. Extract the API key from the `Authorization` header
2. Verify the key matches the API key you configured for this webhook
3. Check that the API key is active and has type `WEBHOOK`
4. Validate the webhook URL matches your registered callback URL

**Important**: Store your webhook API key securely and compare it against incoming requests to ensure authenticity.

