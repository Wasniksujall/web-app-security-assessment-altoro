# Finding 20 — Server Status Check Service Publicly Exposed

| Field | Detail |
|---|---|
| **Severity** | 🟢 Low |
| **Affected URL** | `http://demo.testfire.net/util/serverStatusCheckService.jsp` |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

An internal-looking diagnostic utility endpoint is reachable by any unauthenticated visitor. The endpoint returns server health and status information that is intended for internal monitoring only. Exposing such endpoints publicly:

- Confirms the application is running and reveals availability information (useful for attackers timing their attacks).
- May disclose internal infrastructure details, environment configuration, or service dependencies.
- Provides an additional unauthenticated attack surface for exploitation of the diagnostic service itself.

---

## Steps to Reproduce

1. Without logging in, navigate to:
   `http://demo.testfire.net/util/serverStatusCheckService.jsp`
2. **Result:** The server returns a status/health response to the unauthenticated visitor.

> *Screenshot not available for this finding — confirmed by direct browser navigation to the endpoint.*

---

## Remediation

- **Remove this endpoint from the production environment** or move it behind the firewall so it is only reachable from internal monitoring systems.
- If the endpoint must remain, require authentication and restrict access to specific internal IPs or monitoring agents via an allow-list at the reverse proxy or firewall level.
- Audit all paths under `/util/` for similarly exposed internal endpoints.
