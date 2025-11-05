# Finding 02 — Stored XSS in /comments (High)

**Severity:** High  
**Endpoint:** `POST /comments` (comment content)

## Description
User-supplied comment content is rendered on the page without proper escaping, allowing stored XSS.

## PoC
Payload:
```html
<script>alert(document.cookie)</script>
```
Steps:
1. Submit comment containing payload.
2. Visit page that displays comments; alert executes.

## Impact
Cookie theft, session hijacking, phishing via DOM manipulation, and other client-side attacks.

## Remediation
- Apply proper output encoding (HTML-encode) when rendering untrusted content.
- Consider a CSP and input sanitization libraries.
- HttpOnly flag on session cookies to mitigate cookie theft.

## Evidence
See `evidence/screenshots/xss.png`.
