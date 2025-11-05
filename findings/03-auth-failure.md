# Finding 03 — Insecure Password Reset Flow (Medium)

**Severity:** Medium  
**Endpoint:** `/reset-password`

## Description
The password reset flow discloses whether an email is registered and allows predictable reset tokens (or tokens with long TTL).

## PoC
- Enumerate email addresses via reset endpoint responses.
- Request reset and observe token behavior.

## Impact
User enumeration, targeted phishing, account takeover if tokens are predictable or long-lived.

## Remediation
- Return identical responses for registered/unregistered emails.
- Use cryptographically secure, single-use tokens with short TTL.
- Rate-limit reset attempts and require additional verification steps.

## Evidence
See `evidence/screenshots/reset-flow.png`.
