# Finding 18 — Missing CSRF Protection on Forms

| Field | Detail |
|---|---|
| **Severity** | 🟡 Medium |
| **Affected URL** | `http://demo.testfire.net/` (all state-changing forms — transfer, feedback, login) |
| **OWASP Category** | A01:2021 – Broken Access Control |

---

## Description & Impact

Forms across the application that perform state-changing actions (fund transfers, feedback submissions, account operations) do **not include anti-CSRF tokens**. Without a per-session, unpredictable token validated server-side, an attacker can host a malicious web page containing a hidden form that automatically submits a request to the banking application. If a logged-in victim visits that page, their browser silently sends the authenticated request — including their session cookie — completing the action without their knowledge or consent.

For a banking application, the most critical scenario is a **cross-site fund transfer**: a victim visits a malicious page and unknowingly triggers a transfer of funds to an attacker-controlled account.

---

## Steps to Reproduce

1. Log in to `http://demo.testfire.net/`.
2. Navigate to **Transfer Funds** (`/bank/transfer.jsp`).
3. View the page source (right-click → View Source) or intercept the form in Burp.
4. Search the form HTML for a hidden input with a name like `csrf_token`, `_token`, or `authenticity_token`.
5. **Result:** No anti-CSRF token field is present in the form. The form relies solely on the session cookie — which browsers send automatically on any cross-origin request.

> *Screenshot not available for this finding — absence of CSRF token was confirmed through HTML source inspection of the transfer and feedback forms.*

---

## Remediation

- **Implement per-session CSRF tokens**: generate a cryptographically random token for each session, embed it as a hidden field in every state-changing form, and validate it server-side on every submission. Reject requests where the token is missing or incorrect.
- Set **`SameSite=Strict`** (or `Lax`) on session cookies as a complementary defense — modern browsers block cross-site cookie submission for `Strict`. See Finding 14.
- Consider the **Double Submit Cookie** pattern as an alternative if stateless CSRF protection is preferred.
