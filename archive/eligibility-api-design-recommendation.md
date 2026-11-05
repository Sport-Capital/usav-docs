# USAV Eligibility API - Design Recommendation

**Author:** Alex Wilson / USAV Engineering
**Date:** 2026-03-25
**Version:** 0.2 (Draft)
**Database:** CortIQ
**Status:** Recommendation for review

---

## What This Document Is

A short recommendation for how to scope the Eligibility API. The goal is to keep this API as small and focused as possible: one question in, one answer out. Anything that doesn't directly answer "is this person eligible?" gets moved to a companion API.

---

## The Core Idea

The Eligibility API answers one question: **is this person eligible to participate in a given role, on a given date?**

It evaluates that question at query time by checking a set of conditions defined in the original API document. It does not store eligibility results. It computes them fresh on every request.

---

## Recommended Endpoint

```
GET /api/v1/eligibility/{memberId}?role=player&asOfDate=2026-03-25
```

That's the primary endpoint. One member, one role, one date.

- **memberId** (path, required): The USAV member ID.
- **role** (query, required): The role being checked (e.g., `player`, `coach`, `referee`). This should be required because eligibility rules differ by role.
- **asOfDate** (query, optional, defaults to today): The date to evaluate against. Supports past dates (audits) and future dates (pre-registration).

An optional reference endpoint (`GET /api/v1/eligibility/roles`) could list valid role codes so callers know what values to pass. This is lightweight and can be cached heavily.

---

## Eligibility Conditions

The original API document (api-documentation.md) defines eight conditions that must all pass for a membership role to be eligible. These should be the foundation of the Eligibility API's condition checks:

| # | Condition | Source |
|---|---|---|
| 1 | Role status is Active | Original API doc |
| 2 | Club affiliation required and approved (if applicable to the role) | Original API doc |
| 3 | Membership has started (startDate <= evaluation date) | Original API doc |
| 4 | Membership is not expired (endDate >= evaluation date) | Original API doc |
| 5 | Membership status is Paid | Original API doc |
| 6 | No active global suspensions on the profile | Original API doc |
| 7 | No active role-specific suspensions | Original API doc |
| 8 | All required credentials completed and valid | Original API doc |

---

## What the Response Should Include

The response should contain only what's needed to confirm the person and explain the eligibility result:

- **Identity (minimal):** member ID, first name, last name, date of birth. Enough to confirm correct person. Nothing more.
- **Eligibility result:** a simple `eligible: true/false` boolean.
- **Condition breakdown:** an array showing each of the conditions above with pass/fail, expiry date (where applicable), and a short reason if it failed.
- **Earliest expiry:** the soonest any passing condition expires, as an early warning signal.

---

## What to Remove from This API

The original API document includes contact information and credential data. These don't answer the eligibility question and should be moved to their own APIs:

| Data | Recommended Home |
|---|---|
| Mailing/physical address, guardian contacts, demographics | **Contact API** - handles personal and contact information for members and their guardians |
| Referee, line judge, scorer certifications | **Credentials API** - handles all certification and credential tracking. Note: if a credential is ever confirmed as a hard eligibility requirement for a specific role (see OQ-02 below), it would surface as a condition in the eligibility response, not as credential detail. |

---

## Key Design Decisions (Recommended)

**Fail closed.** If any data source is unavailable (e.g., the background check system is down), the answer should be "ineligible" rather than returning a partial or optimistic result. Safety first.

**No caching of eligibility results.** Because eligibility is computed from live data, caching creates a window where a suspended person could appear eligible. If performance becomes an issue, the right move is to optimize the queries in CortIQ rather than caching the output.

**Condition sets should be configurable, not hardcoded.** Each role will have its own set of required conditions. Those should be stored as configuration so they can be updated without code changes when new requirements are added.

**Suspensions override everything.** The original API document distinguishes between global suspensions (affect all roles) and role-specific suspensions (affect only certain roles). Both block eligibility. A global suspension makes a person ineligible for any role regardless of all other conditions.

---

## Open Questions

These are unresolved items from the initial scope notes that need answers before the API can be finalized:

| ID | Question | Impact |
|---|---|---|
| OQ-01 | What is the full list of roles and their specific eligibility requirements? | Needs USAV national staff input. The API design can proceed, but the per-role condition configuration depends on this. |
| OQ-02 | Do any credential types (e.g., referee certification) constitute a hard eligibility requirement? | The original API document (api-documentation.md) already lists "All required credentials must be completed and valid" as one of the eight eligibility conditions. This suggests credentials are already treated as a hard gate for any role they're assigned to. Early team discussions support this as well. The Eligibility API design handles this without structural changes: credentials assigned to a role appear as conditions in the eligibility check. The Credentials API still owns the full detail; only the pass/fail status and expiry surface here. **This may already be answered by the existing document, but should be confirmed with the team.** |
| OQ-03 | Do unsigned waivers block eligibility? | The original API document includes a `waivers` array on each membership role (with fields for waiver name, version, status, and signed date). However, waivers are notably absent from the document's eight eligibility conditions. The data structure exists, but unsigned waivers do not currently block eligibility in the original design. Recommendation: leave waivers out of the Eligibility API until the team confirms whether they should be a blocking condition. If they should, waivers can be added as a condition the same way credentials are. |

---

## Next Steps

1. Review this recommendation and provide feedback.
2. Get answers to OQ-01, OQ-02, and OQ-03 from USAV national staff.
3. Once scope is locked, produce the formal API specification.
4. Define the CortIQ schema and indexing strategy.

---

*This is a working recommendation and is open for feedback.*
