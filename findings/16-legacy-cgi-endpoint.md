# Finding 16 — Reference to Legacy High-Risk Endpoint (cgi.exe)

| Field | Detail |
|---|---|
| **Severity** | 🟡 Medium |
| **Affected URL** | `http://demo.testfire.net/cgi.exe` (linked from the Locations navigation menu) |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

The site's navigation menu contains a link that points directly to a `.exe` endpoint — `cgi.exe`. Legacy CGI executables are historically associated with **OS command injection vulnerabilities**: they commonly pass URL parameters directly to shell commands or system calls, allowing an attacker to inject shell metacharacters (`;`, `|`, `&&`, `` ` ``, `$(...)`) to execute arbitrary OS commands on the server.

This was identified as a **high-risk observation** rather than a confirmed exploit in this assessment pass — the endpoint was not fully exercised to determine whether command injection is present. However, the presence of a `.exe` CGI endpoint in a modern web application is a significant red flag that warrants dedicated targeted testing.

---

## Steps to Reproduce

1. View the page source or sidebar navigation menu on any page.
2. Locate the "Locations" menu item and inspect its `href` attribute.
3. **Result:** The link points to `cgi.exe` — a legacy executable endpoint pattern that should not exist in a modern web application.

> *Screenshot not available for this finding — the endpoint was identified through source inspection rather than active exploitation.*

---

## Remediation

- **Retire the `/cgi.exe` endpoint entirely** if it is unused or deprecated. Remove the navigation link.
- If the endpoint is still required, conduct dedicated security testing for OS command injection by submitting shell metacharacters in all parameters.
- Ensure any CGI process runs under a **least-privilege service account** with no shell access and no ability to execute system commands.
- Apply strict server-side input validation — reject any parameter value containing shell metacharacters.
