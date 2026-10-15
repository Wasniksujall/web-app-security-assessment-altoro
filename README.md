# Web Application Security Assessment — Altoro Mutual (demo.testfire.net)

**Assessor:** Sujal Wasnik  
**Assessment Period:** 6–9 August 2026  
**Target:** [http://demo.testfire.net/](http://demo.testfire.net/) — Altoro Mutual

> [!IMPORTANT]
> **This is a self-directed security assessment of a deliberately vulnerable application.**
> [demo.testfire.net](http://demo.testfire.net/) is an **intentionally insecure banking demo** created and maintained by IBM for security education and testing purposes. It is not a real bank. No real users, accounts, or financial data were involved. All testing was performed on this publicly accessible demo target, within its intended use as a security-practice environment.

---

## Overview

This repository documents a manual black-box web application security assessment I conducted on the Altoro Mutual demo application. The assessment covers authentication, session management, input handling, access control, information disclosure, and HTTP security configuration — producing **24 findings** ranging from Critical authentication bypass and broken access control to Low-severity hardening gaps.

---

## Scope & Methodology

| Item | Detail |
|---|---|
| **Target** | `http://demo.testfire.net/` and all sub-paths |
| **Assessment Type** | Manual black-box security testing |
| **Period** | 6–9 August 2026 |
| **Testing Areas** | Authentication · Session Management · Input Validation · Access Control · Information Disclosure · HTTP Security Headers |

### Tools Used
- **Burp Suite Community Edition** — HTTP proxy, Repeater, Intercept, HTTP History
- **Browser DevTools** — Cookie inspection, network traffic, page source analysis
- **Manual payload testing** — SQL injection, XSS, path traversal, parameter manipulation

---

## Findings Summary

| # | Finding | Severity | File |
|---|---|---|---|
| 01 | Admin Panel Accessible Without Authorization | 🔴 Critical | [01-admin-panel-no-auth.md](findings/01-admin-panel-no-auth.md) |
| 02 | SQL Injection — Login Authentication Bypass (+ field mapping) | 🔴 Critical | [02-sql-injection-login.md](findings/02-sql-injection-login.md) |
| 03 | IDOR — Unauthorized Access to Other Users' Bank Accounts | 🔴 Critical | [03-idor-bank-accounts.md](findings/03-idor-bank-accounts.md) |
| 04 | Broken Access Control — Fund Transfer Request Tampering | 🔴 Critical | [04-broken-access-control-transfer.md](findings/04-broken-access-control-transfer.md) |
| 05 | Session Fixation | 🟠 High | [05-session-fixation.md](findings/05-session-fixation.md) |
| 06 | Reflected XSS — Search Box and URL Parameter | 🟠 High | [06-reflected-xss-search.md](findings/06-reflected-xss-search.md) |
| 07 | XSS — Feedback Form Name Field and img onerror Bypass | 🟠 High | [07-xss-feedback-and-img-tag.md](findings/07-xss-feedback-and-img-tag.md) |
| 08 | Sensitive Account Data in Client-Side Cookie (Base64 only) | 🟠 High | [08-sensitive-data-cookie.md](findings/08-sensitive-data-cookie.md) |
| 09 | Login Credentials Transmitted Over Unencrypted HTTP | 🟠 High | [09-credentials-over-http.md](findings/09-credentials-over-http.md) |
| 10 | Directory Traversal — Access to web.xml Configuration File | 🟠 High | [10-directory-traversal.md](findings/10-directory-traversal.md) |
| 11 | Swagger UI and API Schema Publicly Exposed | 🟡 Medium | [11-swagger-exposed.md](findings/11-swagger-exposed.md) |
| 12 | Open Redirect via disclaimer.htm | 🟡 Medium | [12-open-redirect.md](findings/12-open-redirect.md) |
| 13 | No Account Lockout Policy | 🟡 Medium | [13-no-account-lockout.md](findings/13-no-account-lockout.md) |
| 14 | Missing Cookie Security Flags (Secure, SameSite) | 🟡 Medium | [14-missing-cookie-flags.md](findings/14-missing-cookie-flags.md) |
| 15 | Verbose Error Messages / Java Stack Trace Disclosure | 🟡 Medium | [15-verbose-errors-stack-trace.md](findings/15-verbose-errors-stack-trace.md) |
| 16 | Reference to Legacy High-Risk Endpoint (cgi.exe) | 🟡 Medium | [16-legacy-cgi-endpoint.md](findings/16-legacy-cgi-endpoint.md) |
| 17 | Missing Security Response Headers (CSP, X-Frame-Options, X-Content-Type-Options) | 🟡 Medium | [17-missing-security-headers.md](findings/17-missing-security-headers.md) |
| 18 | Missing CSRF Protection on State-Changing Forms | 🟡 Medium | [18-missing-csrf-protection.md](findings/18-missing-csrf-protection.md) |
| 19 | HTML Injection via content Parameter | 🟡 Medium | [19-html-injection-content-param.md](findings/19-html-injection-content-param.md) |
| 20 | Server Status Check Service Publicly Exposed | 🟢 Low | [20-server-status-exposed.md](findings/20-server-status-exposed.md) |
| 21 | Information Disclosure in HTML Source Comments | 🟢 Low | [21-html-source-comments.md](findings/21-html-source-comments.md) |
| 22 | HSTS Header Missing | 🟢 Low | [22-hsts-missing.md](findings/22-hsts-missing.md) |
| 23 | Referrer-Policy Header Missing | 🟢 Low | [23-referrer-policy-missing.md](findings/23-referrer-policy-missing.md) |
| 24 | Server Version Disclosure + Internal Executive Info Exposed | 🟢 Low | [24-server-version-disclosure.md](findings/24-server-version-disclosure.md) |

**Severity breakdown:** 4 Critical · 6 High · 9 Medium · 5 Low

> **Note on finding count:** The original assessment identified 29 individual findings. Duplicates with the same root cause and fix were consolidated: the two SQL injection entries (authentication bypass + field mapping on `/doLogin`) are merged into Finding 02; the four XSS vectors are merged into Findings 06 and 07; the two Swagger endpoints are merged into Finding 11; the two server-disclosure findings are merged into Finding 24.

---

## Report

The full written assessment report is available in [`report/Security_Assessment_Report.pdf`](report/Security_Assessment_Report.pdf).

---

## Repository Structure

```
web-app-security-assessment-altoro/
├── README.md
├── report/
│   └── Security_Assessment_Report.pdf
├── findings/
│   └── 01-admin-panel-no-auth.md ... 24-server-version-disclosure.md
└── screenshots/
    └── 01-admin-panel-burp-history.png ... 15-verbose-error-stack-trace.png
```

---

## About

**Sujal Wasnik** — Final-year B.Tech Cyber Security student.  
This project is a self-directed security assessment conducted for learning and portfolio purposes on a publicly available, intentionally vulnerable demo application.
