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

![Entity Relationship Overview](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABHIAAAOgCAYAAABRGlFGAAAAAXNSR0IArs4c6QAAAGJlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAABJKGAAcAAAASAAAAUKABAAMAAAABAAEAAKACAAQAAAABAAAEcqADAAQAAAABAAADoAAAAABBU0NJSQAAAFNjcmVlbnNob3QMUly/AAAB12lUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iWE1QIENvcmUgNi4wLjAiPgogICA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPgogICAgICA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIgogICAgICAgICAgICB4bWxuczpleGlmPSJodHRwOi8vbnMuYWRvYmUuY29tL2V4aWYvMS4wLyI+CiAgICAgICAgIDxleGlmOlBpeGVsWURpbWVuc2lvbj45Mjg8L2V4aWY6UGl4ZWxZRGltZW5zaW9uPgogICAgICAgICA8ZXhpZjpQaXhlbFhEaW1lbnNpb24+MTEzODwvZXhpZjpQaXhlbFhEaW1lbnNpb24+CiAgICAgICAgIDxleGlmOlVzZXJDb21tZW50PlNjcmVlbnNob3Q8L2V4aWY6VXNlckNvbW1lbnQ+CiAgICAgIDwvcmRmOkRlc2NyaXB0aW9uPgogICA8L3JkZjpSREY+CjwveDp4bXBtZXRhPgo1hfIKAABAAElEQVR4AezdX6wc1X048PFPjmwq0tpSVdu8xKVJDApSgUQCqqY2T0AUJ43UFEIeiiMlIQ9JCaCrJqmUy0MT6SoglDwUghTTB0JCKyWCKASpEkaoMkgNIdKiOKFN8Qu2pVZ2BVtwFcm/fJeey8zs/bczu7Mzs58jrXdnd86/z5k9u/7eM7PbhsPhhUwiQIAAAQIECBAgQIAAAQIECBBovcD/a30LNZAAAQIECBAgQIAAAQIECBAgQGAkIJDjQCBAgAABAgQIECBAgAABAgQIdERAIKcjA6WZBAgQIECAAAECBAgQIECAAAGBHMcAAQIECBAgQIAAAQIECBAgQKAjAgI5HRkozSRAgAABAgQIECBAgAABAgQICOQ4BggQIECAAAECBAgQIECAAAECHREQyOnIQGkmAQIECBAgQIAAAQIECBAgQEAgxzFAgAABAgQIECBAgAABAgQIEOiIgEBORwZKMwkQIECAAAECBAgQIECAAAECAjmOAQIECBAgQIAAAQIECBAgQIBARwTGAjmDwSCLW9UkPz/Hj/eP+aOagPnT/Gn+NH9Wmz2y0Xc3x4/jx/FTTcDnr89f86f5s9rsMb/P37FATtUOyEeAAAECBAgQIECAAAECBAgQIDBbgW3D4fDCbKtQOgECBAgQIECAAAECBAgQIECAwDQErMiZhqIyCBAgQIAAAQIECBAgQIAAAQINCAjkNICsCgIECBAgQIAAAQIECBAgQIDANAQEcqahqAwCBAgQIECAAAECBAgQIECAQAMCAjkNIKuCAAECBAgQIECAAAECBAgQIDANAYGcaSgqgwABAgQIECBAgAABAgQIECDQgIBATgPIqiBAgAABAgQIECBAgAABAgQITENAIGcaisogQIAAAQIECBAgQIAAAQIECDQgIJDTALIqCBAgQIAAAQIECBAgQIAAAQLTEBDImYaiMggQIECAAAECBAgQIECAAAECDQgI5DSArAoCBAgQIECAAAECBAgQIECAwDQEBHKmoagMAgQIECBAgAABAgQIECBAgEADAgI5DSCrggABAgQIECBAgAABAgQIECAwDYHt0yhEGQQIECBAgMDbAidPnsxOnDgxup09e/btFzzqjcDevXuzuB06dCjbtWtXb/qlIwQIECBAgED7BbYNh8ML+WYOBoPR5hVXXJF/esuP5ecXB4vjx/tny5NGbkfzh/mj6/PHb37zm+zMmTPZ8ePHc0e2h30XuO6667I9e/Zk27dv9/nn+2Olw93nn8+/OHB8f/b9ucoEYv5YzPnDipwq7xZ5CBAgQIBASSCCOMeOHcvOnTtXesVm3wUicBercmJ1jkSAAAECBAgQmLXA2IqcWVeofAIECBAg0EeBp556ykqcPg7sBH2KlTk33HDDBDnsSoAAAQIECBCYXMCKnMnN5CBAgAABAgWBWIVTPp1q9+7d2c0335xdcskl2c6dOwv72+i+wEsvvZQ9/vjjWf4aSHEMHDhwINu/f3/3O6gHBAgQIECAQGsF/GpVa4dGwwgQIECgKwJxSlU+RRDni1/8YnbppZcK4uRhevT4fe97X3bHHXdkMdb5FBe5lggQIECAAAECsxQQyJmlrrIJECBAYCEETp8+Xejn4cOHBXAKIv3cuOiii0arrvK9E8jJa3hMgAABAgQIzEJAIGcWqsokQIAAgYUSKAdyqv7yyEKh9aSz+/btK/TExa4LHDYIECBAgACBGQgI5MwAVZEECBAgQIDAYgjEqhyJAAECBAgQINCkgEBOk9rqIkCAAAECBAgQIECAAAECBAjUEBDIqYEnKwECBAgQIECAAAECBAgQIECgSQGBnCa11UWAAAECBAgQIECAAAECBAgQqCEgkFMDT1YCBAgQIECAAAECBAgQIECAQJMCAjlNaquLAAECBAgQIECAAAECBAgQIFBDQCCnBp6sBAgQIECAAAECBAgQIECAAIEmBQRymtRWFwECBAgQIECAAAECBAgQIECghoBATg08WQkQIECAAAECBAgQIECAAAECTQqMBXIGg0EWt6pJfn6OH+8f80c1AfNnt+fPaqMuF4G3BLz/u/3+N37Gz/df33+rfp6ZP8wfVeaPsUBO1QNQPgIECBAgQIAAAQIECBAgQIAAgdkKbBsOhxdmW4XSCRAgQIBAvwWWl5cLHVxZWSls2+i3wNLSUqGD5eOh8KINAgQIECBAgEBNAStyagLKToAAAQIECBAgQIAAAQIECBBoSkAgpylp9RAgQIAAAQIECBAgQIAAAQIEagoI5NQElJ0AAQIECBAgQIAAAQIECBAg0JSAQE5T0uohQIAAAQIECBAgQIAAAQIECNQUEMipCSg7AQIECBAgQIAAAQIECBAgQKApAYGcpqTVQ4AAAQIECBAgQIAAAQIECBCoKSCQUxNQdgIECBAgQIAAAQIECBAgQIBAUwICOU1Jq4cAAQIECBAgQIAAAQIECBAgUFNAIKcmoOwECBAgQIAAAQIECBAgQIAAgaYEBHKaklYPAQIECBAgQIAAAQIECBAgQKCmgEBOTUDZCRAgQIAAAQIECBAgQIAAAQJNCQjkNCWtHgIECBAgQIAAAQIECBAgQIBATQGBnJqAshMgQIAAAQIECBAgQIAAAQIEmhIYC+QMBoMsblWT/PwcP94/5o9qAubPbs+f1UZdLgJvCXj/d/v9b/yMn++/vv9W/Twzf5g/qswfY4GcqgegfAQIECBAgAABAgQIECBAgAABArMV2DYcDi/MtgqlEyBAgACBfgssLy8XOriyslLYttFvgaWlpUIHy8dD4UUbBAgQIECAAIGaAlbk1ASUnQABAgQIECBAgAABAgQIECDQlIBATlPS6iFAgAABAgQIECBAgAABAgQI1BQQyKkJKDsBAgQIECBAgAABAgQIECBAoCkBgZympNVDgAABAgQIECBAgAABAgQIEKgpIJBTE1B2AgQIECBAgAABAgQIECBAgEBTAgI5TUmrhwABAgQIECBAgAABAgQIECBQU0Agpyag7AQIECBAgAABAgQIECBAgACBpgQEcpqSVg8BAgQIECBAgAABAgQIECBAoKaAQE5NQNkJECBAgAABAgQIECBAgAABAk0JCOQ0Ja0eAgQIECBAgAABAgQIECBAgEBNAYGcmoCyEyBAgAABAgQIECBAgAABAgSaEhDIaUpaPQQIECBAgAABAgQIECBAgACBmgICOTUBZSdAgAABAgQIECBAgAABAgQINCUwFsgZDAZZ3Kom+fk5frx/zB/VBMyf3Z4/q426XATeEvD+7/b73/gZP99/ff+t+nlm/jB/VJk/xgI5VQ9A+QgQIECAAAECBAgQIECAAAECBGYrsG04HF6YbRVKJ0CAAAEC/RZYXl4udHBlZaWwbaPfAktLS4UOlo+Hwos2CBAgQIAAAQI1BazIqQkoOwECBAgQIECAAAECBAgQIECgKQGBnKak1UOAAAECBAgQIECAAAECBAgQqCkgkFMTUHYCBAgQIECAAAECBAgQIECAQFMCAjlNSauHAAECBAgQIECAAAECBAgQIFBTQCCnJqDsBAgQIECAAAECBAgQIECAAIGmBARympJWDwECBAgQIECAAAECBAgQIECgpoBATk1A2QkQIECAAAECBAgQIECAAAECTQkI5DQlrR4CBAgQIEBgywKvvfbalve1IwECBAgQIEBgkQS2L1Jn9ZUAAQIECHRZIIIbjz76aParX/0qi8fvfOc7s0OHDmUf/vCHZ9qtn/70p9mPfvSj7NOf/nR2ySWXzLSuKPzVV1/NPvKRj2Tvf//7swcffHBL9aU2hkXkkwgQIECAAAECfRWwIqevI6tfBAgQINArgQjeRHDj29/+9mog59ixY9ny8nJ27733zrSvp06dyp544oks7qedIgAT/Yq+pBQBqve+972jW3pus/tZtnGzur1OgAABAgQIEGhSwIqcJrXV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lZXCto1+CywtLRU6WD4eCi/aIECAAAECBAjUFLAipyag7AQIECBAgAABAgQIECBAgACBpgQEcpqSVg8BAgQIECBAgAABAgQIECBAoKaAQE5NQNkJECBAgAABAgQIECBAgAABAk0JCOQ0Ja0eAgQIECBAgAABAgQIECBAgEBNAYGcmoCyEyBAgAABAgQIECBAgAABAgSaEhDIaUpaPQQIECBAgAABAgQIECBAgACBmgICOTUBZSdAgAABAgQIECBAgAABAgQINCUgkNOUtHoIECBAgAABAgQIECBAgAABAjUFBHJqAspOgAABAgQIECBAgAABAgQIEGhKQCCnKWn1ECBAgAABAgQIECBAgAABAgRqCgjk1ASUnQABAgQIECBAgAABAgQIECDQlIBATlPS6iFAgAABAgQIECBAgAABAgQI1BQQyKkJKDsBAgQIECBAgAABAgQIECBAoCmBsUDOYDDI4lY1yc/P8eP9Y/6oJmD+7Pb8WW3U5SLwloD3f7ff/8bP+Pn+6/tv1c8z84f5o8r8MRbIqXoAykeAAAECBAgQIECAAAECBAgQIDBbgW3D4fDCbKtQOgECBAgQ6LfA8vJyoYMrKyuFbRv9FlhaWip0sHw8FF60QYAAAQIECBCoKWBFTk1A2QkQIECAAAECBAgQIECAAAECTQkI5DQlrR4CBAgQIECAAAECBAgQIECAQE0BgZyagLITIECAAAECBAgQIECAAAECBJoSEMhpSlo9BAgQIECAAAECBAgQIECAAIGaAgI5NQFlJ0CAAAECBAgQIECAAAECBAg0JSCQ05S0eggQIECAAAECBAgQIECAAAECNQUEcmoCyk6AAAECBAgQIECAAAECBAgQaEpAIKcpafUQIECAAAECBAgQIECAAAECBGoKCOTUBJSdAAECBAgQIECAAAECBAgQINCUgEBOU9LqIUCAAAECBAgQIECAAAECBAjUFBDIqQkoOwECBAgQIECAAAECBAgQIECgKQGBnKak1UOAAAECBAgQIECAAAECBAgQqCkgkFMTUHYCBAgQIECAAAECBAgQIECAQFMCY4GcwWCQxa1qkp+f48f7x/xRTcD82e35s9qoy0XgLQHv/26//42f8fP91/ffqp9n5g/zR5X5YyyQU/UAlI8AAQIECBAgQIAAAQIECBAgQGC2AtuGw+GF2VahdAIECBAg0G+B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88dYIGeyw87eBAgQIECAAAECBAgQIECAAAECTQlsGw6HF5qqTD0ECBAgQIBA/wSWl5cLnSpvF160QYAAAQIECBAgUEvAipxafDITIECAAAECBAgQIECAAAECBJoTEMhpzlpNBAgQIECAAAECBAgQIECAAIFaAgI5tfhkJkCAAAECBPbu3buKsHv37tXHHhAgQIAAAQIECExfwDVypm+qRAIECBAgsFAC586dy37yk5+M+nzjjTdmu3btWqj+6ywBAgQIECBAoEkBgZwmtdVFgAABAgQIECBAgAABAgQIEKgh4NSqGniyEiBAgAABAgQIECBAgAABAgSaFBDIaVJbXQQIECBAgAABAgQIECBAgACBGgICOTXwZCVAgAABAgQIECBAgAABAgQINCkgkNOktroIECBAgAABAgQIECBAgAABAjUEBHJq4MlKgAABAgQIECBAgAABAgQIEGhSQCCnSW11ESBAgAABAgQIECBAgAABAgRqCAjk1MCTlQABAgQIECBAgAABAgQIECDQpIBATpPa6iJAgAABAgQIECBAgAABAgQI1BAYC+QMBoMsblWT/PwcP94/5o9qAuZP86f50/xZbfbIRt/dHD+OH8dPNQGfvz5/zZ/mz2qzx/w+f8cCOVU7IB8BAgQIECBAgAABAgQIECBAgMBsBbYNh8MLs61C6QQIECBAgAABAgQIECBAgAABAtMQsCJnGorKIECAAAECPRY4depU9uMf/zj72c9+1uNe6hoBAgQIECBAoBsC27vRTK0kQIAAAQIE5iHwne98J3vsscey119/Pbvpppuyq666ah7NUCcBAgQIECBAgMD/CViR41AgQIAAAQIE1hQ4cuRIFoGcD37wg2u+7kkCBAgQIECAAIHmBazIad5cjQQIECBAoDMC3/rWt7K9e/dmTz755FibX3755eziiy/O9u3bN3otgj7vec97sne/+93ZP/7jP46e+/jHPz56PfZNZcTKnthPIkCAAAECBAgQmFzAxY4nN5ODAAECBAgslEBcIycCMhGA+cpXvjLq+7PPPpt96UtfGp1qFcGeSH/6p3862k4BntOnT4+e/8IXvpB985vfHAWE0nNf//rXrfQZ6fiHAAECBAgQIDCZgFOrJvOyNwECBAgQIPBbgViJE7fyypq4IHIEdv7pn/5pdB9YEcSJFTrx3NGjR0d+cfFkiQABAgQIECBAYHIBp1ZNbiYHAQIECBBYeIG46PFPfvKTMYe4nk4K7qQLI8d9Ov0qXotTteLiyRIBAgQIECBAgMDkAlbkTG4mBwECBAgQILCOQKzSkQgQIECAAAECBGYnIJAzO1slEyBAgAABAgQIECBAgAABAgSmKiCQM1VOhREgQIAAAQIECBAgQIAAAQIEZicgkDM7WyUTIECAAIHeCsQvWf3FX/xF9thjj/W2jzpGgAABAgQIEGijgEBOG0dFmwgQIECAQMsF/u3f/i2LnxJ/8sknW95SzSNAgAABAgQI9Etg23A4vNCvLukNAQIECBAgME2B+IWpl19+efRrU+nXp6L8Z599NotfpEoXOI6fHo9fpMrvE8+Vf6Z8reem2V5lESBAgAABAgT6LDAWyBkMBqP+XnHFFZX6LT+/OHAcP94/VSYQ84f5w/xh/vT54fPD58fkAj4/fX76/PT56fNzsT4/nVo1+WelHAQIECBAgAABAgQIECBAgACBuQiMrciZSytUSoAAAQIECBAgQIAAAQIECBAgsKmAFTmbEtmBAAECBAgQIECAAAECBAgQINAOAYGcdoyDVhAgQIAAAQIECBAgQIAAAQIENhUQyNmUyA4ECBAgQIAAAQIECBAgQIAAgXYICOS0Yxy0ggABAgQIECBAgAABAgQIECCwqYBAzqZEdiBAgAABAgQIECBAgAABAgQItENAIKcd46AVBAgQIECAAAECBAgQIECAAIFNBQRyNiWyAwECBAgQIECAAAECBAgQIECgHQICOe0YB60gQIAAAQIECBAgQIAAAQIECGwqIJCzKZEdCBAgQIAAgc0Ezpw5k8VNIkCAAAECBAgQmK3A9tkWr3QCBAgQIECg7wLPPPNM9vTTT4+6ef3112cHDx7se5f1jwABAgQIECAwNwErcuZGr2ICBAgQINAPgRTEid4cP368H53SCwIECBAgQIBASwUEclo6MJpFgAABAgS6KPDmm292sdnaTIAAAQIECBDojIBATmeGSkMJECBAgAABAgQIECBAgACBRRcYC+QMBoMsblWT/PwcP94/5o9qAuZP82eX589qR/3buRz/jv8uH/+OX8ev49f337c/0SZ7ZP4wf1SZP8YCOZMddvYmQIAAAQIECBAgQIAAAQIECBBoSmDbcDi80FRl6iFAgAABAgT6J7C8vFzoVHm78KINAgQI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iNWPEcz58pe/PJsKlEqAAIEeCViR06PB7FtX4roFEcSJFTZ33XXXavfSRTBj1U78tTf2i2sobJTiuhJRVpzGFat1Dh8+PHaBy1RufIlIK3riV1MkAlUF4ieNv/CFL2R/+Zd/mf34xz8eKyaOtSeffDK76aabRsdcLCX/5je/Odo/HkeeOBYjCPSe97xndHpWei7y+EvlGGntJ1Iwp3yaVfo1q62cZlW7EQqYukD+M2TqhSuQAIGpCMRnWnzexfe7copTlOPz74Mf/ODoczFejz+UxArX+Kx99tlnR7/M9/GPf3z02RinZ6XnIo8/ppRFbRMg0HUBgZyuj+CCtT+ucxBBmbh+zoEDB0aPb7311iyuT7HeL49EgCZukSduv/zlL0f7xxeFdPpVXFcnAj0RuIl94ldN4iKdDz744Gi7D8w///nPR0GDuBaINB2B66+/Pjt48OC6hUXwJW7rpe985zujMYkvqBHwiftYxRP38cU08sYX0S996UujL6vxfHou9osgz0anaZ07dy773ve+t3pa13rt8PzWBARztubUxr02C/bPus3m31kLK7/tApdffnn253/+59mOHTvWbWp8DkZaK5ATn3nxh47448fRo0dH+6XPy/ijSHwWxn06NStei+fiPj5rI20UzDl//vzoe+Arr7wy2tc/BNoqsHv37uz3fu/3Rt8Bf/d3fze79NJLsz179rS1udo1Q4GxQM5gMBhVd8UVV1SqVv75+lUatJZmiuBLfJinVTERXIkgTqzASb8yEq/Hz+xGICb+Qx3XPsinODUryoky4ldVUoogTeSJ4E/kSUGctE8EeOJXX9b6MpHK6Np9nK4miDPdUQvTyy67bPUDdNL5L/76GMGZ+AtipH//93/P3njjjezd73539pWvfGX0XPxVMb68RmDn61//+ui5+GIaf4lMX1RHT/72n3L9P/nJTwRxEs6U7iOYE+7XXnvtlEpUzCIImH8XYZT1cSOBX/ziF9m73vWudefO8udXuaz4XIzATKyuyaf4nhaBnf/6r//K/vM//zP727/929EcHasq43MzgjuxkjJWuW6U/vmf/zkTxNlIyGttETh79mwWt/zx+ju/8zvZH/zBH4xWeFcJ6mz2/tus7/LP5///Y4GczQbK6wRmJRDBlAjWRIoP5rjFqVPpp3aPHTs2ei2/8iaCMLEdwZo4bSpW0+TT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CBAgQIBATkAgJ4fhIQECBAgQIECAAAECBAgQIECgzQICOW0eHW0jQIAAAQIECBAgQIAAAQIECOQEBHJyGB4SIECAAAECBAgQIECAAAECBNosIJDT5tHRNgIECBAgQIAAAQIECBAgQIBATkAgJ4fhIQECBAgQIECAAAECBAgQIECgzQICOW0eHW0jQIAAAQIECBAgQIAAAQIECOQEBHJyGB4SIECAAAECBAgQIECAAAECBNosIJDT5tHRNgIECBAgQIAAAQIECBAgQIBATkAgJ4fhIQECBAgQIECAAAECBAgQIECgzQICOW0eHW0jQIAAAQIECBAgQIAAAQIECOQEBHJyGB4SIECAAAECBAgQIECAAAECBNosMBbIGQwGWdyqJvn5OX68f8wf1QTMn+ZP86f5s9rskY2+uzl+HD+On2oCPn99/i7y/FntXfN2Lu+f+bx/xgI5bw+JRwQIECBAgAABAgQIECBAgAABAm0S2DYcDi+0qUHaQoAAAQIEygLLy8uFp8rbhRdtECBAgAABAgQIbCpQ/j5V3t60ADvMTcCKnLnRq5gAAQIECBAgQIAAAQIECBAgMJmAQM5kXvYmQIAAAQIECBAgQIAAAQIECMxNQCBnbvQqJkCAAAECBAgQIECAAAECBAhMJiCQM5mXvQkQIECAAAECBAgQIECAAAECcxMQyJkbvYoJECBAgAABAgQIECBAgAABApMJCORM5mVvAgQIECBAgAABAgQIECBAgMDcBARy5kavYgIECBAgQIAAAQIECBAgQIDAZAICOZN52ZsAAQIECBAgQIAAAQIECBAgMDcBgZy50auYAAECBAgQIECAAAECBAgQIDCZgEDOZF72JkCAAAECBAgQIECAAAECBAjMTUAgZ270KiZAgAABAgQIECBAgAABAgQITCYgkDOZl70JECBAgAABAgQIECBAgAABAnMTEMiZG72KCRAgQIAAAQIECBAgQIAAAQKTCYwFcgaDQRa3qkl+fo4f7x/zRzUB86f50/xp/qw2e2Sj726OH8eP46eagM9fn7+LPH9We9e8ncv7Zz7vn7FAzttD4hEBAgQIECBAgAABAgQIECBAgECbBLYNh8MLbWqQthAgQIAAgbLA8vJy4anyduFFGwQIECBAgAABApsKlL9Plbc3LcAOcxOwImdu9ComQIAAAQIECBAgQIAAAQIECEwmIJAzmZe9CRAgQIAAAQIECBAgQIAAAQJzExDImRu9igkQIEBgqwL79+9f3fXyyy9ffewBAQIECBAgQIAAgUUT2L5oHdZfAgQIEOiewC233JKdOHEie+ONN7Krr766ex3QYgIECBAgQIAAAQJTEhDImRKkYggQIEBgdgI7d+7MrrzyytlVoGQCBAgQIECAAAECHRFwalVHBkozCRAgQIAAAQIECBAgQIAAAQICOY4BAgQIECBAgAABAgQIECBAgEBHBARyOjJQmkmAAAECBAgQIECAAAECBAgQEMhxDBAgQIAAAQIECBAgQIAAAQIEOiIgkNORgdJMAgQIECBAgAABAgQIECBAgIBAjmOAAAECBAgQIECAAAECBAgQINARAYGcjgyUZhIgQIAAAQIECBAgQIAAAQIEBHIcAwQIECBAgAABAgQIECBAgACBjgiMBXIGg0EWt6pJfn6OH+8f80c1AfOn+dP8af6sNntko+9ujh/Hj+OnmoDPX5+/izx/VnvXvJ3L+2c+75+xQM7bQ+IRAQIECBAgQIAAAQIECBAgQIBAmwS2DYfDC21qkLYQIECAAAECBAgQIECAAAECsxVYXl4uVFDeLrxoo1UC21vVGo0hQIAAgYUUeOyxx7KXX345u+qqq7IPfehDC2mg0wQIECBAgAABAgS2IiCQsxUl+xAgQIDATAROnTqVfe1rX8t+9rOfrZYvkLNK4QEBAgQIECBAgACBMQHXyBkj8QQBAgQINCVw5MiRLII5X/7yl5uqUj0ECBAgQIAAAQIEOi1gRU6nh0/jCRAg0G2BWH3zqU99KnvttdfGOvL6669nP/7xj7MPfvCD2b59+0avf/7znx+dfnXxxRdnzz77bBb7fPzjHx+djhWnZ6XnIk+UKxEgQIAAAQIECBDom4BATt9GVH8IECDQIYEvfOELo9auFciJ062++c1vZk8++WR29OjR0X7xXARvYhVPXE8nf2pWvBbPxf13vvOd0f6COR06GDSVAAECBAgQIEBgSwICOVtishMBAgQINC3w7ne/exSYidU1+RRBnwjsxCqdCOTEipwI3jz88MNZrNSJ5+KUrbh4skSAAAECBAgQIECgbwICOX0bUf0hQIBATwQiUPOtb31rrDfvec97Vk+1SqdcxX0EcSKlx7FyRyJAgAABAgQIECDQNwEXO+7biOoPAQIEei6QAjY976buESBAgAABAgQIEFhTQCBnTRZPEiBAgAABAgQIECBAgAABAgTaJyCQ074x0SICBAgQ+D+BuN6NRIAAAQIECBAgQIDA2wICOW9beESAAAECLRKInxKPCxnHT45LBAgQIECAAAECBAi8JSCQ40ggQIAAgVYKpGvhxMWNJQIECBAgQIAAAQIE3hLYNhwOL8AgQIAAAQJtFIhfnkoBnTa2T5sIECBAgAABAl0VWF5eLjS9vF140UarBMZW5AwGgyxuVZP8/Bw/3j/mj2oC5s/x+XOSIA6/cb9JjkR+/Hx++/yeZM7I72v+MH+YP7o7f+Tfy1Uee//P5/0/FsipMnjyECBAgAABAgQIECBAgAABAgQIzF7AqVWzN1YDAQIECBAgQIAAAQIECBBolUD5VKrydqsaqzEFAStyChw2CBAgQIAAAQIECBAgQIAAAQLtFRDIae/YaBkBAgQIECBAgAABAgQIECBAoCAgkFPgsEGAAAECBAgQIECAAAECBAgQaK+AQE57x0bLCBAgQIAAAQIECBAgQIAAAQIFAYGcAocNAgQIECBAgAABAgQIECBAgEB7BQRy2js2WkaAAAECBAgQIECAAAECBAgQKAgI5BQ4bBAgQIAAAQIECBAgQIAAAQIE2isgkNPesdEyAgQIECBAgAABAgQIECBAgEBBYHthywYBAgQIEGihwPnz57MXXnghu+iii7LLL78827FjRwtbqUkECBAgQIAAAQIEZi8gkDN7YzUQIECAQE2BH/zgB9mJEydGpbz44ovZbbfdVrNE2QkQIECAAAECBAh0U8CpVd0cN60mQIDAQgmkIE50+pVXXlmovussAQIECBAgQIAAgbyAQE5ew2MCBAgQIECAAAECBAgQIECAQIsFBHJaPDiaRoAAAQIECBAgQIAAAQIECBDIC4wFcgaDQRa3qkl+fo4f7x/zRzUB86f50/xp/qw2e2Sj726OH8eP46eagM9fn7+LPH9We9e8ncv7Zz7vn7FAzttD4hEBAgQIECBAgAABAgQIECBAgECbBLYNh8MLbWqQthAgQIAAgbLA8vJy4anyduFFGwQIECBAgAABApsKlL9Plbc3LcAOcxOwImdu9ComQIAAAQIECBAgQIAAAQIECEwmIJAzmZe9CRAgQIAAAQIECBAgQIAAAQJzExDImRu9igkQIECAAAECBAgQIECAAAEC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FgICOS2QLEKAAAECBAgQIECAAAECBAgQ6IKAQE4XakEZCBAgQIAAAQIECBAgQIAAAQItBARyWiBZhAABAgQIECBAgAABAgQIECDQBQGBnC7UgjIQIECAAAECBAgQIECAAAECBFoICOS0QLIIAQIECBAgQIAAAQIECBAgQKALAgI5XagFZSBAgAABAgQIECBAgAABAgQItBAQyGmBZBECBAgQIECAAAECBAgQIECAQBcENgVy1tfXqzxmTdbn5/jp7//PrP/31iMQAe2/9l/7v7z2XytEYDcC2m/t9yq337v537EugWW1n5sCOaqCAAECBAgQIECAAAECBAgQIECgmwL7NjY2znezaEpFgMBeCqytrY1t7sSJE2PTJoYtcPz48bEdbB4PY2+aIECgUwLN/1ftd6eqZ+GF0X4vnNgGBiyg/Rxw5bbYtT63n0bktKhgixAgQIAAAQIECBAgQIAAAQIEuiAgkNOFWlAGAgQIECBAgAABAgQIECBAgEALAYGcFkgWIUCAAAECBAgQIECAAAECBAh0QUAgpwu1oAwECBAgQIAAAQIECBAgQIAAgRYCAjktkCxCgAABAgQIECBAgAABAgQIEOiCgEBOF2pBGQgQIECAAAECBAgQIECAAAECLQQEclogWYQAAQIECBAgQIAAAQIECBAg0AUBgZwu1IIyECBAgAABAgQIECBAgAABAgRaCAjktECyCAECBAgQIECAAAECBAgQIECgCwICOV2oBWUgQIAAAQIECBAgQIAAAQIECLQQEMhpgWQRAgQIECBAgAABAgQIECBAgEAXBARyulALykCAAAECBAgQIECAAAECBAgQaCEgkNMCySIECBAgQIAAAQIECBAgQIAAgS4IbArkrK+vV3nMmqzPz/HT3/+fWf/vrUcgAtp/7b/2f3ntv1aIwG4EtN/a71Vuv3fzv2NdAstqPzcFclQFAQIECBAgQIAAAQIECBAgQIBANwX2bWxsnO9m0ZSKAIG9FFhbWxvb3IkTJ8amTQxb4Pjx42M72Dwext40QYBApwSa/6/a705Vz8ILo/1eOLENDFhA+zngym2xa31uP43IaVHBFiFAgAABAgQIECBAgAABAgQIdEFAIKcLtaAMBAgQIECAAAECBAgQIECAAIEWAgI5LZAsQoAAAQIECBAgQIAAAQIECBDogoBAThdqQRkIECBAgAABAgQIECBAgAABAi0EBHJaIFmEAAECBAgQIECAAAECBAgQINAFAYGcLtSCMhAgQIAAAQIECBAgQIAAAQIEWggI5LRAsggBAgQIECBAgAABAgQIECBAoAsCAjldqAVlIECAAAECBAgQIECAAAECBAi0EBDIaYFkEQKrKHD27NlV3G37TIAAgd4LaL97X4V2gACBJQloP5cEb7NTCwjkTE1mBQLDFDh06NDYjj377LNj0yaGK7C+vj62cxdffPHYtAkCBLotoP3udv0ssnTa70XqynsVBLSfq1DLk/ex7+2nQM7kejWXwMoJXH755WP7fN9991W+lRgjGeRE6viBBx4Y27fDhw+PTZsgQKDbAtrvbtfPokqn/V6UrHxXSUD7uUq1/eK+DqH9FMh5sT69IrDSAldcccXY/j/33HPVXXfdVf385z8fm29iGAI5gf36178e1XHqup6uu+66+qTXBAh0XED73fEKmnPxtN9zBpXdSgtoP1er+ofUfu7b2Ng4v1rVZ28JENhK4MEHH6wef/zxrd42fwUEjh07Vt1www0rsKd2kcCwBLTfw6rPWfZG+z2LmnUIVJX201HQx/Zz04icXCvWvF5smqq1Pj/Hz/j9Rvr0/3PkyJHq4MGD0xTZsgMSSN3nGJg1af+1/9r/5bX/2u9ZW65hrKf91v5qf2dvf7Wfw2gHZ92LvrafF826w9YjQGB4Ai996Uura6+9tjp16pSROcOr3m33KN9EpCNz0UVOC9tCeZNARwW03x2tmD0olvZ7D5BtYtAC2s9BV++2O9fn9tOlVdtWrTcJrK7AmTNnqoceeqg6ffr0KLCzuhLD3fP8UkNu8pfrw48ePTrcHbVnBFZMQPs9/ArXfg+/ju3hcgS0n8tx38utDqX9FMjZy6PGtggQIECAAAECBAgQIECAAAECuxDYdI+cXeRlVQIECBAgQIAAAQIECBAgQIAAgQUKCOQsEFfWBAgQIECAAAECBAgQIECAAIF5CgjkzFNTXgQIECBAgAABAgQIECBAgACBBQr4eZIF4sqaAIH+CDzxxBPVgw8+WB04cKC6/vrrq6uvvro/hVdSAgQIECBAgAABAlMK6P9OCdahxY3I6VBlKAoBAssTyC90nTt3rsqvFSSgIxEgQIAAAQIECBAYsoD+b39rVyCnv3Wn5AQIzFEgAZySEtCRCBAgQIAAAQIECAxZQP+3v7UrkNPfulNyAgQIECBAgAABAgQIECBAYMUEBHJWrMLtLgECBAgQIECAAAECBAgQINBfAYGc/tadkhMgQIAAAQIECBAgQIAAAQIrJiCQs2IVbncJECBAgAABAgQIECBAgACB/goI5PS37pScAAECBAgQIECAAAECBAgQWDEBgZwVq3C7S4AAAQIECBAgQIAAAQIECPRXYFMgZ319vcpj1mR9fo4f/z99bT9mLXdZT/un/dP+af9KezDts/ZD+6H90H5M226U5bUf2o/dtB/lOJr12fG3nONvUyBn1gq0HgECBAgQIECAAAECBAgQIECAwGIF9m1sbJxf7CbkToAAge4LrK2tjRWyOT32pgkCBAgQIECAAAECPRdo9neb0z3fvUEX34icQVevnSNAgAABAgQIECBAgAABAgSGJCCQM6TatC8ECBAgQIAAAQIECBAgQIDAoAUEcgZdvXaOAAECBAgQIECAAAECBAgQGJKAQM6QatO+ECBAgAABAgQIECBAgAABAoMWEMgZdPXaOQIECBAgQIAAAQIECBAgQGBIAgI5Q6pN+0KAAAECBAgQIECAAAECBAgMWkAgZ9DVa+cIECBAgAABAgQIECBAgACBIQkI5AypNu0LAQIECBAgQIAAAQIECBAgMGgBgZxBV6+dI0CAAAECBAgQIECAAAECBIYkIJAzpNq0LwQIECBAgAABAgQIECBAgMCgBQRyBl29do4AAQIECBAgQIAAAQIECBAYkoBAzpBq074QIECAAAECBAgQIECAAAECgxYQyBl09do5AgQIECBAgAABAgQIECBAYEgCmwI56+vrVR6zJuvzc/z4/+lr+zFruct62j/tn/ZP+1fag2mftR/aD+2H9mPadqMsr/3Qfuym/SjH0azPjr/lHH+bAjmzVqD1CBAgQIAAAQIECBAgQIAAAQIEFiuwb2Nj4/xiNyF3AgQIdF9gbW1trJDN6bE3TRAgQIAAAQIECBDouUCzv9uc7vnuDbr4RuQMunrtHAECBAgQIECAAAECBAgQIDAkAYGcIdWmfSFAgAABAgQIECBAgAABAgQGLSCQM+jqtXMECBAgQIAAAQIECBAgQIDAkAQEcoZUm/aFAAECBAgQIECAAAECBAgQGLSAQM6gq9fOESBAgAABAgQIECBAgAABAkMSEMgZUm3aFwIECBAgQIAAAQIECBAg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CBAgQIAAAQKrLbApkLO+vl7lMWvq+/qz7rf1CESg78e/8mv//CcTmFVA+7Ha7Yf6V/85BmZNjh/HzzKPn1mPW+sRiMCy2q9NgRzVQYAAAQIECBAgQIAAAQIECBAg0E2BfRsbG+e7WbS9L9Xa2trYRk+cODE2bWLYAsePHx/bwebxMPamCQIDE2ge79q/gVXwDruj/dsByNsECBAgMEgB/Z9BVmvrnepz/8eInNbVbEECBAgQIECAAAECBAgQIECAwHIFBHKW62/rBAgQIECAAAECBAgQIECAAIHWAgI5raksSIAAAQIECBAgQIAAAQIECBBYroBAznL9bZ0AAQIECBAgQIAAAQIECBAg0FpAIKc1lQUJECBAgAABAgQIECBAgAABAssVEMhZrr+tEyBAgAABAgQIECBAgAABAgRaCwjktKayIAECBAgQIECAAAECBAgQIEBguQICOcv1t3UCBAgQIECAAAECBAgQIECAQGsBgZzWVBYkQIAAAQIECBAgQIAAAQIECCxXQCBnuf62ToAAAQIECBAgQIAAAQIECBBoLSCQ05rKggQIECBAgAABAgQIECBAgACB5QoI5CzX39YJECBAgAABAgQIECBAgAABAq0FBHJaU1mQAAECBAgQIECAAAECBAgQILBcgU2BnPX19SqPWVPf1591v61HIAJ9P/6VX/vnP5nArALaj9VuP9S/+s8xMGty/Dh+lnn8zHrcWo9ABJbVfm0K5KgOAgQIECBAgAABAgQIECBAgACBbgrs29jYON/Nou19qdbW1sY2euLEibFpE8MWOH78+NgONo+HsTdNEBiYQPN41/4NrIJ32B3t3w5A3iZAgACBQQro/wyyWlvvVJ/7P0bktK5mCxIgQIAAAQIECBAgQIAAAQIElisgkLNcf1snQIAAAQIECBAgQIAAAQIECLQWEMhpTWVBAgQIECBAgAABAgQIECBAgMByBQRylutv6wQIECBAgAABAgQIECBAgACB1gICOa2pLEiAAAECBAgQIECAAAECBAgQWK6AQM5y/W2dAAECBAgQIECAAAECBAgQINBaQCCnNZUFCRAgQIAAAQIECBAgQIAAAQLLFRDIWa6/rRMgQIAAAQIECBAgQIAAAQIEWgsI5LSmsiABAgQIECBAgAABAgQIECBAYLkCAjnL9bd1AgQIECBAgAABAgQIECBAgEBr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)

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

