# Finding 23 — Referrer-Policy Header Missing

| Field | Detail |
|---|---|
| **Severity** | 🟢 Low |
| **Affected URL** | `http://demo.testfire.net/` (site-wide) |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

No `Referrer-Policy` header is set on any HTTP response. Without this header, the browser uses its default referrer behavior — which, depending on the browser version and context, may include **sending the full URL of the current page** (including query parameters) in the `Referer` header when users click outbound links.

For a banking application, URLs may contain sensitive query parameters (e.g. account numbers in `listAccounts=`, session references, etc.). These could be leaked to any third-party site that an outbound link points to, via the `Referer` request header that the browser automatically attaches.

---

## Steps to Reproduce

1. Inspect the HTTP response headers for any page on `http://demo.testfire.net/`.
2. **Result:** No `Referrer-Policy` header is present.

> *Screenshot not available for this finding — confirmed through Burp Suite response header inspection.*

---

## Remediation

Add a `Referrer-Policy` header to all responses:

```http
Referrer-Policy: strict-origin-when-cross-origin
```

- `strict-origin-when-cross-origin` sends only the origin (no path or query parameters) when navigating cross-origin, and the full URL only for same-origin requests.
- Alternatively, `no-referrer` suppresses the header entirely for maximum privacy.
