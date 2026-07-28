# Security & HTTPS for SEO: SSL/TLS, Security Headers & Safe Browsing

> **A first-principles guide to security as an SEO ranking signal, SSL/TLS certificate configuration, security response headers (HSTS, CSP), and mixed content fixes.**

---

## 📌 Executive Summary

Search engines prioritize user safety. HTTPS (HTTP over SSL/TLS) has been an explicit Google ranking signal since 2014. Web applications returning security warnings, expired SSL certificates, or HTTP mixed-content errors suffer search ranking downgrades and browser access blocks.

```mermaid
flowchart LR
    A[Unencrypted HTTP Connection] --> B[Browser Displays "Not Secure" Warning]
    B --> C[User Bounces Immediately -> High Bounce Rate]
    A --> D[Switch to HTTPS + HSTS Security Headers]
    D --> E[SSL Green Lock + Minor SEO Ranking Boost]
    E --> F[Higher Conversion & Lower Bounce Rate]
```

---

## 1. HTTPS Security Headers Checklist

Configure essential security headers in your web server (Caddy, Nginx, or Go host):

```text
┌───────────────────────────────────────────────────────────────────────────┐
│                      RECOMMENDED SECURITY HEADERS                         │
└───────────────────────────────────────────────────────────────────────────┘
   Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
   X-Content-Type-Options: nosniff
   X-Frame-Options: SAMEORIGIN
   Referrer-Policy: strict-origin-when-cross-origin
```

### Example Caddyfile Security Configuration
```caddy
seoer.ai {
    header {
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "SAMEORIGIN"
        Referrer-Policy "strict-origin-when-cross-origin"
    }
    reverse_proxy localhost:8080
}
```

---

## 2. Fixing Mixed Content Errors

**Mixed Content** occurs when a page served over secure HTTPS requests insecure resources (images, scripts, CSS) over plain HTTP.

```html
<!-- BAD: Mixed Content Request on HTTPS Page -->
<img src="http://example.com/image.png" alt="Insecure Image">

<!-- GOOD: Relative or Secure Protocol Request -->
<img src="https://example.com/image.png" alt="Secure Image">
```

---

## 3. Summary

HTTPS and web security are foundational to user trust and search rankings. By automating AutoSSL certificates, enforcing Strict-Transport-Security (HSTS), and fixing mixed content errors, you protect your users and secure your domain's rankings.
