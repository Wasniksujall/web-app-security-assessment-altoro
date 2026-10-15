# Finding 17 — Missing Security Response Headers

| Field | Detail |
|---|---|
| **Severity** | 🟡 Medium |
| **Affected URL** | `http://demo.testfire.net/` (site-wide) |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

HTTP responses across the application are missing three standard security hardening headers:

| Header | Status | Risk if absent |
|---|---|---|
| `Content-Security-Policy` | ❌ Missing | Increases impact of XSS (Findings 06–07); no browser-level script execution controls |
| `X-Frame-Options` | ❌ Missing | Page can be embedded in an `<iframe>`, enabling clickjacking attacks |
| `X-Content-Type-Options` | ❌ Missing | Browser may MIME-sniff responses, executing files as a different content type than intended |

These are low-effort, high-impact defensive headers that are straightforward to add at the server or reverse-proxy level and are expected by security scanners, bug bounty programs, and OWASP checklists.

---

## Steps to Reproduce

1. Load any page on `http://demo.testfire.net/`.
2. Open DevTools → **Network** → select any HTML response → **Headers** tab.
3. **Result:** `Content-Security-Policy`, `X-Frame-Options`, and `X-Content-Type-Options` are all absent from the response headers.

> *Screenshot not available for this finding — header absence was verified through Burp Suite HTTP history and browser DevTools during routine traffic inspection.*

---

## Remediation

Add the following headers to all HTTP responses at the server or reverse-proxy level:

```http
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
```

- **CSP** significantly reduces XSS exploitability even if output encoding is missed somewhere.
- **X-Frame-Options: DENY** prevents clickjacking by blocking the page from loading in any frame.
- **X-Content-Type-Options: nosniff** stops browsers from MIME-sniffing content.
