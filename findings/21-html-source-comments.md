# Finding 21 — Information Disclosure in HTML Source Comments

| Field | Detail |
|---|---|
| **Severity** | 🟢 Low |
| **Affected URL** | `http://demo.testfire.net/login.jsp` |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

The HTML source of the login page contains a developer comment that references an **internal contact process and a phone number** for obtaining the "latest admin login." This comment was left in the production page and is visible to anyone who views the page source.

While this does not directly compromise the application, it provides attackers with:

- **Social engineering material**: a phone number and internal process they can reference to impersonate staff or contact employees under false pretences.
- **Confirmation of an admin account**: the comment implies admin credentials are obtainable via a specific internal channel — useful reconnaissance for targeted phishing.

---

## Steps to Reproduce

1. Navigate to `http://demo.testfire.net/login.jsp`.
2. Right-click → **View Page Source** (or intercept the response in Burp Suite).
3. Search for HTML comments (`<!-- ... -->`).
4. **Result:** An HTML comment near the login header references an internal contact process and phone number for obtaining admin login details.

> *Screenshot not available for this finding — confirmed by viewing the raw HTML source of the login page.*

---

## Remediation

- **Strip all internal notes, TODOs, and contact/process details from HTML before deploying to production.** Use a build pipeline step to remove comments automatically.
- Keep internal operational procedures (including who to contact for credentials) in internal documentation only — never in client-facing HTML.
- Audit all pages for similar developer comments and remove them.
