# Finding 24 — Server Version Disclosure and Internal Executive Information Exposed

| Field | Detail |
|---|---|
| **Severity** | 🟢 Low |
| **Affected URLs** | `http://demo.testfire.net/` (Server header) · `http://demo.testfire.net/index.jsp?content=inside_executives.htm` |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

Two related information-disclosure issues were identified that provide useful reconnaissance material:

**Vector A — Server Version in Response Header:** The HTTP `Server` response header discloses the exact application server and version: **Apache Tomcat/7.0.92** (via the `Apache-Coyote/1.1` header, corroborated by error pages). Tomcat 7.0.92 is an end-of-life release with numerous publicly documented CVEs. An attacker can look up applicable exploits for this specific version with a single search.

**Vector B — Internal Executive/Management Details Page:** A page listing company executives and management details is reachable by any visitor through the `content` parameter mechanism (`inside_executives.htm`) without authentication. Named individuals and their roles are disclosed to anonymous visitors, providing material for targeted phishing and social-engineering attacks against the organization.

---

## Steps to Reproduce

### Vector A — Server Version
1. Inspect the HTTP response headers for any page (DevTools → Network or Burp Suite).
2. **Result:** The `Server` header returns `Apache-Coyote/1.1`; error pages additionally confirm `Apache Tomcat/7.0.92`.

### Vector B — Executive Information
1. Without logging in, navigate to:
   `http://demo.testfire.net/index.jsp?content=inside_executives.htm`
2. **Result:** A page listing internal executive/management names and roles loads without requiring any authentication.

> *Screenshots not available for these findings — confirmed through Burp Suite response header inspection (Vector A) and direct browser navigation (Vector B).*

---

## Remediation

**Vector A — Server Version:**
- **Suppress or generalize the `Server` response header** at the web server or reverse-proxy level (e.g. set it to a generic value or remove it entirely).
- Upgrade Apache Tomcat from 7.0.92 to a current, supported release and keep it patched.

**Vector B — Executive Information:**
- **Require authentication** for any internal-facing content pages.
- Audit all values reachable through the `content` parameter for similarly sensitive pages and restrict access.
- Apply the allow-list fix described in Finding 10 and Finding 19 to prevent arbitrary content enumeration.
