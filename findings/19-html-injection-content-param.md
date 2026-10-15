# Finding 19 — HTML Injection via content Parameter

| Field | Detail |
|---|---|
| **Severity** | 🟡 Medium |
| **Affected URL** | `http://demo.testfire.net/index.jsp?content=<h1>HTML</h1>` |
| **OWASP Category** | A03:2021 – Injection |

---

## Description & Impact

The `content` GET parameter on `index.jsp` is used to load page fragments. In addition to enabling directory traversal (Finding 10), the parameter also accepts and renders **raw HTML markup** without sanitization. An attacker can inject arbitrary HTML into the page — headings, forms, images, links — altering its appearance in the victim's browser.

While this stops short of script execution (unlike the XSS findings), HTML injection enables:

- **Fake login forms** embedded in the legitimate bank page for credential harvesting (phishing).
- **Defacement** — altering page content to spread misinformation or damage trust.
- A **stepping stone to XSS** — if a browser or WAF is more permissive with HTML than with `<script>` tags, injection of event-handler attributes (`onclick`, `onmouseover`) may execute JavaScript.

---

## Steps to Reproduce

1. Navigate to:
   `http://demo.testfire.net/index.jsp?content=<h1>HTML</h1>`
2. **Result:** The injected `<h1>HTML</h1>` tag is rendered as a real, styled heading on the page — displayed as large bold text rather than being escaped or stripped.

> *Screenshot not available for this finding — confirmed through direct browser observation; the injected tag was visually rendered as a styled heading within the page layout.*

---

## Remediation

- **Sanitize/HTML-encode all output derived from the `content` parameter** before rendering it in the page.
- Replace the arbitrary file-path mechanism with a **server-side allow-list** of valid page identifiers (e.g. `content=home` maps to a fixed, safe template). User input should never select or render raw page fragments.
- This same parameter is the root of the directory traversal issue (Finding 10) — fixing the allow-list approach resolves both findings simultaneously.
