# Finding 04 — Broken Access Control (High)

**Severity:** High  
**Endpoint:** `/basket` API (OWASP Juice Shop)  
**Tested Environment:** OWASP Juice Shop (demo.owasp-juice.shop)  
**Tool Used:** Burp Suite (Proxy & Repeater)

---

## Description
Broken Access Control occurs when a web application does not properly enforce what users are allowed to do. In this case, the **basket API** in OWASP Juice Shop allowed changing the `token` or `userId` parameter to another value, revealing or modifying another user’s shopping basket.

---

## Proof of Concept (PoC)

1. Logged in as a normal user and viewed the empty basket.  
2. In **Burp Suite**, turned *Intercept On* and captured the request to `/api/Basket`.  
   Example (redacted):

   ```
   GET /rest/basket/1 HTTP/1.1
   Host: demo.owasp-juice.shop
   Authorization: Bearer eyJhbGciOi...
   ```

   The basket ID (`1`) corresponded to the logged-in user.

3. Sent the request to **Repeater**, changed the ID to `2`, and resent:

   ```
   GET /rest/basket/2 HTTP/1.1
   Host: demo.owasp-juice.shop
   Authorization: Bearer eyJhbGciOi...
   ```

4. Response showed another user’s basket data containing `“Raspberry Juice”`.  
5. When the modified request was forwarded through the proxy, the victim’s basket appeared in the browser — confirming unauthorized access.

---

## Impact
An attacker could view or manipulate resources belonging to other users, leading to:

- Exposure of user-specific data (items, transactions, addresses, etc.)  
- Unauthorized modifications to other users’ baskets or profiles  
- Potential privilege escalation if similar flaws exist in admin endpoints

**OWASP Reference:** A01:2021 — Broken Access Control  
**CWE:** CWE-284 (Improper Access Control)

---

## Remediation
- Implement server-side access control checks to verify ownership of resources before serving data.  
- Never rely on client-supplied identifiers (`id`, `token`) for authorization decisions.  
- Use session or JWT claims to map requests to the authenticated user.  
- Perform authorization after authentication, for every sensitive operation.  
- Include automated unit/integration tests for access control logic.

---

## Evidence
- Burp Suite Repeater screenshots showing successful access to `/basket/2` while logged in as user 1.  
- Saved request/response logs in `evidence/burp-requests.txt` (section “Broken Access Control”).  
- Screenshot: `evidence/screenshots/04-broken-access-control/broken-access-1.png`.

---

**Status:** Confirmed  
**Risk:** High — direct object reference exposure and privilege escalation risk.  
**Recommendation:** Fix access control logic and retest after patch.
