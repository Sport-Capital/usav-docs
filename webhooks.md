# USAV Partner API Webhooks

This document describes the outbound webhook contract for USAV Partner API partners.

Webhooks are asynchronous notifications sent from USAV to a partner-controlled HTTPS endpoint when selected Partner API domain events occur. They complement the Partner API REST endpoints; the webhook payload is a bounded event snapshot, not a replacement for fetching the full current resource state through the API.

## Contract summary

| Area | Contract |
|---|---|
| Delivery method | `POST` request with `Content-Type: application/json` |
| Delivery guarantee | At-least-once delivery; duplicate events are possible and expected |
| Idempotency key | `payload.id`, also sent as `USAV-Event-Id` |
| Authentication | HMAC-SHA256 signature in `USAV-Webhook-Signature` |
| Callback auth | No Partner API bearer token is sent to callback URLs |
| Success response | Any HTTP `2xx` status |
| Timeout | 10 seconds per delivery attempt |
| Retryable failures | Network/timeout failures, HTTP `408`, `409`, `425`, `429`, and `5xx` |
| Terminal failures | Callback URL validation failures, signing-secret errors, and non-retryable HTTP `4xx` |

Partners must build receivers as idempotent consumers. Store the webhook event id before applying side effects and ignore or safely replay duplicates with the same `payload.id`.

## Subscription setup

Webhook subscriptions are managed by USAV through MMS. There is no partner self-service Partner API endpoint for subscription management in the launch contract.

A subscription contains:

| Field | Description |
|---|---|
| `name` | Human-readable subscription name used by USAV operations. |
| `callbackUrl` | Public HTTPS endpoint that receives webhook `POST` requests. |
| `eventTypes` | Specific subscribed event types. An empty list means all supported launch events. |
| `scopeType` | Subscription scope. Region-scoped subscriptions are the normal partner setup. `ALL_REGIONS` is reserved for explicitly global USAV-managed subscriptions. |
| `regionId` | Region associated with a region-scoped subscription. |
| `isActive` | Inactive subscriptions do not receive deliveries. |
| `oneTimeSecret` | Secret shown only when the subscription is created or rotated. It is used for signature verification. |

### Secret handling

USAV displays the webhook signing secret only once during create/rotate. Store it immediately in the partner secret manager.

The secret currently uses the `usav_whsec_...` prefix. Treat the full value as opaque secret material. USAV stores only a SHA-256 hash of the secret and cannot recover the original value.

Rotating a secret replaces the signing key for future delivery attempts immediately. If possible, configure the receiver to accept both the previous and the new secret during a short transition window, then remove the previous secret after successful deliveries with the new secret are observed.

## Callback URL requirements

Partner callback URLs must satisfy these requirements:

- use HTTPS;
- accept HTTP `POST` with a JSON body;
- be publicly reachable through DNS;
- not include username/password credentials in the URL;
- not point to localhost, `.local`, link-local, private, metadata, or other non-public network targets;
- return a `2xx` response within 10 seconds;
- avoid redirects; process the request at the configured URL;
- do not require an `Authorization` header, because USAV does not send one;
- do not put secrets in query parameters. Use the HMAC signature instead.

## Event envelope

Every delivery uses the same top-level JSON envelope:

```json
{
  "id": "7f7d5fe2-8b9a-4e44-92e4-b0f8c8db4f8c",
  "type": "membership.paid",
  "created": "2026-06-01T12:00:00.000Z",
  "data": {
    "object": {
      "id": "b4f3af66-7e36-40dd-9f22-1d941f43fdb5",
      "profileId": "a67c28ff-4427-4869-b5a8-377c4f233cf2",
      "regionId": "e4b27524-9d12-4f2a-9e28-927aeb7df8a1",
      "status": "Paid",
      "startDate": "2026-06-01",
      "endDate": "2026-08-31"
    }
  }
}
```

| Field | Type | Description |
|---|---|---|
| `id` | string | Opaque webhook event id. Use it as the idempotency key. Production events are currently UUID-backed, but partners must not depend on the format. |
| `type` | string | Event type. See [Supported event types](#supported-event-types). |
| `created` | string | UTC ISO-8601 timestamp for the event envelope. |
| `data.object` | object | Minimal event snapshot. The shape depends on `type`. Use REST endpoints to fetch full current resource details when needed. |

Trigger-produced payloads omit null fields. Partners must tolerate missing optional fields and additive fields.

## Delivery headers

USAV sends the following headers on webhook deliveries:

| Header | Description |
|---|---|
| `Content-Type` | Always `application/json`. |
| `User-Agent` | `USAV-Partner-Webhook-Relay/1.0`. |
| `USAV-Event-Id` | Same value as `payload.id`. |
| `USAV-Webhook-Timestamp` | Unix timestamp in seconds generated immediately before delivery. |
| `USAV-Webhook-Attempt` | Delivery attempt number for this event/subscription pair. First attempt is `1`. |
| `USAV-Webhook-Signature` | HMAC-SHA256 signature formatted as `v1=<hex>`. |

HTTP header names are case-insensitive.

## Signature verification

Partners must verify every webhook before processing it.

### Canonical string

The signature is computed over this exact canonical string:

```text
<event-id>.<timestamp>.<raw-json-body>
```

Where:

- `<event-id>` is the value of `USAV-Event-Id`;
- `<timestamp>` is the value of `USAV-Webhook-Timestamp`;
- `<raw-json-body>` is the exact UTF-8 request body as delivered over HTTP.

Verify against the raw request body before JSON parsing, formatting, normalization, or mutation.

### Signing key derivation

USAV stores `sha256(oneTimeSecret)` and uses the 32-byte SHA-256 digest as the HMAC key.

Verification algorithm:

1. Read the raw request body as bytes/string without modifying it.
2. Read `USAV-Event-Id`, `USAV-Webhook-Timestamp`, and `USAV-Webhook-Signature`.
3. Reject if the signature header does not start with `v1=`.
4. Compute `sha256(oneTimeSecret)` as bytes.
5. Compute `HMAC-SHA256(key=sha256(oneTimeSecret), message=<event-id>.<timestamp>.<raw-json-body>)`.
6. Compare the computed hex digest to the `v1=` digest using constant-time comparison.
7. Reject stale timestamps using a replay window appropriate for the partner system. USAV recommends 5 minutes unless your infrastructure needs a narrower window.
8. Parse JSON only after the signature check passes.
9. Confirm `payload.id` matches `USAV-Event-Id`.

### Node.js verification example

```ts
import { createHash, createHmac, timingSafeEqual } from 'node:crypto';

type HeadersLike = Record<string, string | string[] | undefined>;

function header(headers: HeadersLike, name: string): string | undefined {
  const value = headers[name.toLowerCase()] ?? headers[name];
  return Array.isArray(value) ? value[0] : value;
}

export function verifyUsavWebhook(options: {
  headers: HeadersLike;
  oneTimeSecret: string;
  rawBody: Buffer | string;
  toleranceSeconds?: number;
}) {
  const eventId = header(options.headers, 'usav-event-id');
  const timestamp = header(options.headers, 'usav-webhook-timestamp');
  const signatureHeader = header(options.headers, 'usav-webhook-signature');
  const toleranceSeconds = options.toleranceSeconds ?? 300;

  if (!eventId || !timestamp || !signatureHeader?.startsWith('v1=')) {
    return false;
  }

  const timestampSeconds = Number.parseInt(timestamp, 10);
  if (!Number.isFinite(timestampSeconds)) {
    return false;
  }

  const nowSeconds = Math.floor(Date.now() / 1000);
  if (Math.abs(nowSeconds - timestampSeconds) > toleranceSeconds) {
    return false;
  }

  const rawBody = Buffer.isBuffer(options.rawBody)
    ? options.rawBody.toString('utf8')
    : options.rawBody;
  const canonicalString = `${eventId}.${timestamp}.${rawBody}`;
  const signingKey = createHash('sha256').update(options.oneTimeSecret).digest();
  const expectedHex = createHmac('sha256', signingKey).update(canonicalString).digest('hex');
  const receivedHex = signatureHeader.slice('v1='.length);

  const expected = Buffer.from(expectedHex, 'hex');
  const received = Buffer.from(receivedHex, 'hex');

  return expected.length === received.length && timingSafeEqual(expected, received);
}
```

### Python verification example

```py
import hashlib
import hmac
import time


def verify_usav_webhook(headers, raw_body: bytes, one_time_secret: str, tolerance_seconds: int = 300):
    event_id = headers.get("USAV-Event-Id") or headers.get("usav-event-id")
    timestamp = headers.get("USAV-Webhook-Timestamp") or headers.get("usav-webhook-timestamp")
    signature = headers.get("USAV-Webhook-Signature") or headers.get("usav-webhook-signature")

    if not event_id or not timestamp or not signature or not signature.startswith("v1="):
        return False

    try:
        timestamp_seconds = int(timestamp)
    except ValueError:
        return False

    if abs(int(time.time()) - timestamp_seconds) > tolerance_seconds:
        return False

    signing_key = hashlib.sha256(one_time_secret.encode("utf-8")).digest()
    message = f"{event_id}.{timestamp}.".encode("utf-8") + raw_body
    expected = hmac.new(signing_key, message, hashlib.sha256).hexdigest()
    received = signature[len("v1="):]

    return hmac.compare_digest(expected, received)
```

## Supported event types

The launch webhook event catalog is closed over the following event types. Do not assume other internal MMS events are available until USAV explicitly adds them to this contract.

Payload field lists describe fields produced by the current database event triggers. Null values may be omitted.

| Event type | Fired when | `data.object` fields | Notes |
|---|---|---|---|
| `membership.created` | A `Membership` row is inserted. | `id`, `profileId`, `regionId`, `status`, `startDate`, `endDate` | Region-scoped when `regionId` is present. |
| `membership.updated` | A `Membership` row changes. | `id`, `profileId`, `regionId`, `status`, `startDate`, `endDate` | Sent for any changed membership row. |
| `membership.paid` | `Membership.status` transitions to `Paid`. | `id`, `profileId`, `regionId`, `status`, `startDate`, `endDate` | This is a transition event, not a periodic paid-state snapshot. |
| `profile.created` | A `Profile` row is inserted. | `id`, `memberId`, `status`, `firstName`, `lastName` | Profile-only events are not inherently region-scoped. Region subscriptions receive only events with a resolvable region; global subscriptions may receive profile-only events. |
| `profile.updated` | A `Profile` row changes. | `id`, `memberId`, `status`, `firstName`, `lastName` | Same scope caveat as `profile.created`. |
| `profile.suspended` | A `ProfileSuspension` row is inserted. | `id`, `profileId`, `type`, `startDate`, `endDate` | Profile suspension events are not inherently region-scoped unless future payloads include a region. |
| `club.affiliated` | `Club.affiliationStatus` crosses into `Active`. | `id`, `name`, `code`, `regionId`, `affiliationStatus` | Region-scoped by `regionId`. |
| `club.disaffiliated` | `Club.affiliationStatus` crosses out of `Active`. | `id`, `name`, `code`, `regionId`, `affiliationStatus` | Region-scoped by `regionId`. |
| `credential.completed` | A supported credential/task row is inserted or updated into `Completed`. | `id`, `membershipMembershipRoleId`, `membershipRoleTaskId`, `status`, `requiredFrom`, `completedAt`, `validUntil`, `credential` | Only supported credential types are emitted: `SAFESPORT`, `LITMOS`, `NCSI`. |
| `credential.expired` | A supported completed credential/task row is written with `validUntil <= now()`. | `id`, `membershipMembershipRoleId`, `membershipRoleTaskId`, `status`, `requiredFrom`, `completedAt`, `validUntil`, `credential` | Current trigger is write-triggered only. Pure time-passing expiry requires a future scheduler/sweep. |
| `club.transfer.initiated` | A `ProfileClubTransfer` is inserted/updated with status `Pending` or `WaitForOtherSide`. | `id`, `profileId`, `profileClubId`, `fromClubId`, `toClubId`, `status`, `fromStatus`, `toStatus` | Scope resolves from destination club region first, then source club region. |
| `club.transfer.approved` | A `ProfileClubTransfer.status` changes to `Approved`. | `id`, `profileId`, `profileClubId`, `fromClubId`, `toClubId`, `status`, `fromStatus`, `toStatus` | Approval does not necessarily mean the final club assignment mutation has completed. |
| `club.transfer.rejected` | A `ProfileClubTransfer.status` changes to `Rejected`. | `id`, `profileId`, `profileClubId`, `fromClubId`, `toClubId`, `status`, `fromStatus`, `toStatus` | Terminal rejected transfer event. |
| `club.transfer.completed` | `ProfileClub.status` transitions from `IN_TRANSFER` to `APPROVED` for the matching transfer. | `id`, `profileId`, `profileClubId`, `fromClubId`, `toClubId`, `status`, `profileClubStatus` | Completion is tied to final club assignment state, not only the transfer row status. |
| `team.created` | A `ClubTeam` row is inserted. | `id`, `clubId`, `seasonId`, `name`, `division`, `gender` | Scope resolves through the owning club. |
| `team.updated` | A `ClubTeam` row changes or a `ClubTeamMember` roster row is inserted, updated, or deleted. | Team update shape: `id`, `clubId`, `seasonId`, `name`, `division`, `gender`. Roster mutation shape: `id`, `rosterMemberId`, `profileId`, `rosterMutation`. | `rosterMutation` is one of `insert`, `update`, `delete` when the event comes from roster membership changes. |

The nested `credential` object currently contains:

```json
{
  "id": "f41738e6-7bd4-43e2-a498-cb4dd7fdd2f9",
  "name": "SafeSport Training",
  "type": "SAFESPORT"
}
```

## Event examples

The examples below show complete JSON request bodies for each supported event type. Values are illustrative. Production event ids are opaque and unique; do not depend on their format. Trigger-produced payloads use `jsonb_strip_nulls`, so null fields may be omitted from real deliveries.

### `membership.created`

```json
{
  "id": "10000000-0000-4000-8000-000000000001",
  "type": "membership.created",
  "created": "2026-06-01T12:00:00.000Z",
  "data": {
    "object": {
      "id": "40000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "regionId": "30000000-0000-4000-8000-000000000001",
      "status": "Unpaid",
      "startDate": "2026-06-01",
      "endDate": "2026-08-31"
    }
  }
}
```

### `membership.updated`

```json
{
  "id": "10000000-0000-4000-8000-000000000002",
  "type": "membership.updated",
  "created": "2026-06-02T15:30:00.000Z",
  "data": {
    "object": {
      "id": "40000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "regionId": "30000000-0000-4000-8000-000000000001",
      "status": "Suspended",
      "startDate": "2026-06-01",
      "endDate": "2026-08-31"
    }
  }
}
```

### `membership.paid`

```json
{
  "id": "10000000-0000-4000-8000-000000000003",
  "type": "membership.paid",
  "created": "2026-06-03T09:45:00.000Z",
  "data": {
    "object": {
      "id": "40000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "regionId": "30000000-0000-4000-8000-000000000001",
      "status": "Paid",
      "startDate": "2026-06-01",
      "endDate": "2026-08-31"
    }
  }
}
```

### `profile.created`

```json
{
  "id": "10000000-0000-4000-8000-000000000004",
  "type": "profile.created",
  "created": "2026-06-01T10:00:00.000Z",
  "data": {
    "object": {
      "id": "20000000-0000-4000-8000-000000000001",
      "memberId": "USAV1234567",
      "status": "active",
      "firstName": "Avery",
      "lastName": "Morgan"
    }
  }
}
```

### `profile.updated`

```json
{
  "id": "10000000-0000-4000-8000-000000000005",
  "type": "profile.updated",
  "created": "2026-06-04T14:15:00.000Z",
  "data": {
    "object": {
      "id": "20000000-0000-4000-8000-000000000001",
      "memberId": "USAV1234567",
      "status": "active",
      "firstName": "Avery",
      "lastName": "Morgan-Jones"
    }
  }
}
```

### `profile.suspended`

```json
{
  "id": "10000000-0000-4000-8000-000000000006",
  "type": "profile.suspended",
  "created": "2026-06-05T08:00:00.000Z",
  "data": {
    "object": {
      "id": "e0000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "type": "misconduct",
      "startDate": "2026-06-05T00:00:00.000Z",
      "endDate": "2026-07-05T00:00:00.000Z"
    }
  }
}
```

### `club.affiliated`

```json
{
  "id": "10000000-0000-4000-8000-000000000007",
  "type": "club.affiliated",
  "created": "2026-06-06T11:00:00.000Z",
  "data": {
    "object": {
      "id": "50000000-0000-4000-8000-000000000001",
      "name": "Example Volleyball Club",
      "code": "EXVC",
      "regionId": "30000000-0000-4000-8000-000000000001",
      "affiliationStatus": "Active"
    }
  }
}
```

### `club.disaffiliated`

```json
{
  "id": "10000000-0000-4000-8000-000000000008",
  "type": "club.disaffiliated",
  "created": "2026-06-07T11:00:00.000Z",
  "data": {
    "object": {
      "id": "50000000-0000-4000-8000-000000000001",
      "name": "Example Volleyball Club",
      "code": "EXVC",
      "regionId": "30000000-0000-4000-8000-000000000001",
      "affiliationStatus": "Archived"
    }
  }
}
```

### `credential.completed`

```json
{
  "id": "10000000-0000-4000-8000-000000000009",
  "type": "credential.completed",
  "created": "2026-06-08T16:20:00.000Z",
  "data": {
    "object": {
      "id": "a0000000-0000-4000-8000-000000000001",
      "membershipMembershipRoleId": "b0000000-0000-4000-8000-000000000001",
      "membershipRoleTaskId": "c0000000-0000-4000-8000-000000000001",
      "status": "Completed",
      "requiredFrom": "2026-06-01T00:00:00.000Z",
      "completedAt": "2026-06-08T16:10:00.000Z",
      "validUntil": "2027-06-08T23:59:59.000Z",
      "credential": {
        "id": "d0000000-0000-4000-8000-000000000001",
        "name": "SafeSport Training",
        "type": "SAFESPORT"
      }
    }
  }
}
```

### `credential.expired`

```json
{
  "id": "10000000-0000-4000-8000-000000000010",
  "type": "credential.expired",
  "created": "2026-06-09T09:00:00.000Z",
  "data": {
    "object": {
      "id": "a0000000-0000-4000-8000-000000000001",
      "membershipMembershipRoleId": "b0000000-0000-4000-8000-000000000001",
      "membershipRoleTaskId": "c0000000-0000-4000-8000-000000000001",
      "status": "Completed",
      "requiredFrom": "2025-06-01T00:00:00.000Z",
      "completedAt": "2025-06-08T16:10:00.000Z",
      "validUntil": "2026-06-08T23:59:59.000Z",
      "credential": {
        "id": "d0000000-0000-4000-8000-000000000001",
        "name": "SafeSport Training",
        "type": "SAFESPORT"
      }
    }
  }
}
```

### `club.transfer.initiated`

```json
{
  "id": "10000000-0000-4000-8000-000000000011",
  "type": "club.transfer.initiated",
  "created": "2026-06-10T13:00:00.000Z",
  "data": {
    "object": {
      "id": "80000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "profileClubId": "90000000-0000-4000-8000-000000000001",
      "fromClubId": "50000000-0000-4000-8000-000000000010",
      "toClubId": "50000000-0000-4000-8000-000000000001",
      "status": "Pending",
      "fromStatus": "Approved",
      "toStatus": "Pending"
    }
  }
}
```

### `club.transfer.approved`

```json
{
  "id": "10000000-0000-4000-8000-000000000012",
  "type": "club.transfer.approved",
  "created": "2026-06-11T13:00:00.000Z",
  "data": {
    "object": {
      "id": "80000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "profileClubId": "90000000-0000-4000-8000-000000000001",
      "fromClubId": "50000000-0000-4000-8000-000000000010",
      "toClubId": "50000000-0000-4000-8000-000000000001",
      "status": "Approved",
      "fromStatus": "Approved",
      "toStatus": "Approved"
    }
  }
}
```

### `club.transfer.rejected`

```json
{
  "id": "10000000-0000-4000-8000-000000000013",
  "type": "club.transfer.rejected",
  "created": "2026-06-11T14:00:00.000Z",
  "data": {
    "object": {
      "id": "80000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "profileClubId": "90000000-0000-4000-8000-000000000001",
      "fromClubId": "50000000-0000-4000-8000-000000000010",
      "toClubId": "50000000-0000-4000-8000-000000000001",
      "status": "Rejected",
      "fromStatus": "Approved",
      "toStatus": "Rejected"
    }
  }
}
```

### `club.transfer.completed`

```json
{
  "id": "10000000-0000-4000-8000-000000000014",
  "type": "club.transfer.completed",
  "created": "2026-06-12T13:00:00.000Z",
  "data": {
    "object": {
      "id": "80000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "profileClubId": "90000000-0000-4000-8000-000000000001",
      "fromClubId": "50000000-0000-4000-8000-000000000010",
      "toClubId": "50000000-0000-4000-8000-000000000001",
      "status": "Approved",
      "profileClubStatus": "APPROVED"
    }
  }
}
```

### `team.created`

```json
{
  "id": "10000000-0000-4000-8000-000000000015",
  "type": "team.created",
  "created": "2026-06-13T09:30:00.000Z",
  "data": {
    "object": {
      "id": "70000000-0000-4000-8000-000000000001",
      "clubId": "50000000-0000-4000-8000-000000000001",
      "seasonId": "60000000-0000-4000-8000-000000000001",
      "name": "18U National",
      "division": "18U",
      "gender": "Female"
    }
  }
}
```

### `team.updated`

A `team.updated` event can come from a direct `ClubTeam` row change:

```json
{
  "id": "10000000-0000-4000-8000-000000000016",
  "type": "team.updated",
  "created": "2026-06-14T09:30:00.000Z",
  "data": {
    "object": {
      "id": "70000000-0000-4000-8000-000000000001",
      "clubId": "50000000-0000-4000-8000-000000000001",
      "seasonId": "60000000-0000-4000-8000-000000000001",
      "name": "18U National Red",
      "division": "18U",
      "gender": "Female"
    }
  }
}
```

It can also come from a roster mutation on `ClubTeamMember`:

```json
{
  "id": "10000000-0000-4000-8000-000000000017",
  "type": "team.updated",
  "created": "2026-06-14T10:15:00.000Z",
  "data": {
    "object": {
      "id": "70000000-0000-4000-8000-000000000001",
      "rosterMemberId": "f0000000-0000-4000-8000-000000000001",
      "profileId": "20000000-0000-4000-8000-000000000001",
      "rosterMutation": "insert"
    }
  }
}
```


## Scope and event filtering

A delivery is sent only when all of these are true:

1. the subscription is active;
2. the subscription event filter is empty, or it contains the event type;
3. the subscription scope matches the event scope.

Scope behavior:

| Subscription scope | Matching rule |
|---|---|
| `REGION` | Event must resolve to the same `regionId` as the subscription. |
| `ALL_REGIONS` | Subscription must have `regionId = null`; receives events regardless of region when event type filter matches. |

Events with no resolvable region do not go to region-scoped subscriptions. This is intentional fail-closed behavior to avoid leaking cross-region data.

## Delivery and retry semantics

USAV writes webhook events to a durable outbox, then the Supabase Edge relay drains the outbox and sends partner callbacks. The database trigger that creates the event does not call partner endpoints directly.

Delivery properties:

- at-least-once delivery;
- no exactly-once guarantee;
- no strict ordering guarantee across different events or subscriptions;
- retry attempts reuse the same `payload.id`;
- a later retry can redeliver an event to a callback URL that previously acknowledged it if another matching subscription failed in the same outbox event lifecycle;
- partners must deduplicate by `payload.id`.

### Retry schedule

| Failed attempt | Next action |
|---:|---|
| 1 | retry after 60 seconds |
| 2 | retry after 5 minutes |
| 3 | retry after 15 minutes |
| 4 | retry after 60 minutes |
| 5 | dead-letter; no further automatic retry |

Retry-scheduled attempts are recorded internally with `RETRY_SCHEDULED`. When retries are exhausted or a failure is terminal, the delivery is dead-lettered.

### Response handling

| Partner response | USAV behavior |
|---|---|
| Any `2xx` | Delivery is marked delivered. |
| `408`, `409`, `425`, `429`, `5xx` | Delivery is retryable until the maximum attempt is reached. |
| Other `4xx` | Delivery is terminal/dead-lettered. |
| Network error or timeout | Delivery is retryable until the maximum attempt is reached. |

USAV stores only a bounded response body excerpt for diagnostics. Do not return secrets, tokens, PII, or full internal error dumps from webhook endpoints.

## Partner receiver checklist

Use this checklist before enabling a production subscription:

- [ ] Endpoint is public HTTPS and accepts `POST` JSON.
- [ ] Endpoint can read the raw request body before JSON parsing.
- [ ] HMAC signature verification is implemented with constant-time comparison.
- [ ] Timestamp replay protection is enforced.
- [ ] `payload.id` is persisted as an idempotency key before side effects.
- [ ] Duplicate delivery with the same `payload.id` is safe.
- [ ] The handler returns `2xx` quickly after durable acceptance, not after long downstream workflows.
- [ ] Long-running processing is moved to partner-side background jobs/queues.
- [ ] The endpoint does not require `Authorization` from USAV.
- [ ] Receiver logs do not store webhook secrets, raw signature secrets, bearer tokens, or full sensitive payloads.
- [ ] Monitoring alerts on repeated non-2xx responses or missing expected event flow.

## Troubleshooting

| Symptom | Likely cause | Action |
|---|---|---|
| `401` or invalid signature in partner receiver | Receiver verifies parsed/reformatted JSON instead of raw body, uses the one-time secret directly as HMAC key, or has the wrong rotated secret. | Verify the raw body, compute `sha256(oneTimeSecret)` as bytes, and compare against `v1=<hex>`. |
| Duplicate events | Normal at-least-once retry behavior. | Deduplicate by `payload.id`. |
| No delivery for a region subscription | Event did not match event type filter, subscription inactive, or event had no resolvable region. | Check subscription configuration and event scope. Profile-only events may require global scope. |
| `credential.expired` not emitted exactly at expiry time | Current trigger emits only when the supported credential row is written with an already-expired `validUntil`. | Treat clock-only expiry as a known launch limitation until USAV adds a scheduler/sweep. |
| Delivery dead-lettered after partner `4xx` | Non-retryable client error returned by partner endpoint. | Fix endpoint behavior, then ask USAV operations to replay or re-enqueue if needed. |
| Delivery retries after `5xx` or timeout | Partner endpoint unavailable or too slow. | Return `2xx` after durable acceptance and move heavy processing to a queue. |

## Compatibility rules

Partners should treat the webhook contract as forward-compatible:

- accept additive fields in `data.object`;
- ignore unknown fields;
- do not depend on JSON property order;
- do not depend on `id` string format beyond uniqueness;
- tolerate omitted optional fields;
- use event type allowlists in receiver code;
- fetch full current state through Partner API REST endpoints when the webhook snapshot is insufficient.
