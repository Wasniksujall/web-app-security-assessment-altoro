# Finding 22 — HTTP Strict Transport Security (HSTS) Header Missing

| Field | Detail |
|---|---|
| **Severity** | 🟢 Low |
| **Affected URL** | `http://demo.testfire.net/` (site-wide) |
| **OWASP Category** | A05:2021 – Security Misconfiguration |

---

## Description & Impact

The application does not send a `Strict-Transport-Security` (HSTS) response header. HSTS instructs browsers to **always use HTTPS** for the domain, even if the user types `http://` or clicks an HTTP link — preventing protocol-downgrade attacks.

Combined with the absence of HTTPS altogether (Finding 09), the lack of HSTS means:

- Browsers are never told to prefer a secure connection.
- Users who manually type the HTTP URL or follow an HTTP bookmark are never upgraded automatically.
- **SSL-stripping attacks** (a type of man-in-the-middle that downgrades HTTPS to HTTP silently) are not mitigated.

---

## Steps to Reproduce

1. Inspect the HTTP response headers for any page on `http://demo.testfire.net/` (DevTools → Network, or Burp Suite).
2. **Result:** No `Strict-Transport-Security` header is present in any response.

> *Screenshot not available for this finding — confirmed through Burp HTTP history response header inspection.*

---

## Remediation

Once the site is migrated to HTTPS (see Finding 09), add the following header to all responses:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

- `max-age=31536000` instructs browsers to remember the HTTPS policy for one year.
- `includeSubDomains` extends the policy to all subdomains.
- `preload` allows submission to browser HSTS preload lists for maximum protection.
