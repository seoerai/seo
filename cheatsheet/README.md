# ⚡️ Search Engineering & GEO Developer CLI Cheatsheet

> **SEOER.AI Lab Spec // Module 23: First-principles developer CLI reference for cURL crawler simulation, Lighthouse CLI headless runners, server log forensics, and universal JSON-LD templates.**

---

## 1. Googlebot HTTP Header Inspection via cURL

Simulate search engine crawler requests directly from your terminal:

```bash
# 1. Simulate Googlebot Desktop Request (HTTP Headers + Body)
curl -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" -Iv https://seoer.ai

# 2. Simulate Googlebot Mobile Smartphone Request
curl -A "Mozilla/5.0 (Linux; Android 6.0.1; Nexus 5X Build/MMB29P) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Mobile Safari/537.36 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" -Iv https://seoer.ai

# 3. Check for Gzip / Brotli Compression Support
curl -H "Accept-Encoding: gzip, br" -I https://seoer.ai

# 4. Check HTTP Status Code & Canonical Target Redirects
curl -s -D - https://seoer.ai/old-page -o /dev/null
```

---

## 2. Lighthouse CLI Automated Audits

Run automated headless performance and technical SEO audits from CI/CD pipelines:

```bash
# 1. Run Headless Lighthouse Audit for Desktop
npx lighthouse https://seoer.ai --preset=desktop --only-categories=performance,seo --output=json --output-path=./lighthouse-desktop.json

# 2. Run Headless Lighthouse Audit for Mobile
npx lighthouse https://seoer.ai --form-factor=mobile --only-categories=performance,seo --output=html --output-path=./lighthouse-mobile.html

# 3. Fail CI Pipeline if Performance Score drops below 90%
npx lighthouserc run --assert.assertions.categories:performance="error"
```

---

## 3. Server Log Inspection One-Liners

```bash
# Extract Googlebot requests from Nginx/Caddy access.log
grep -i "googlebot" /var/log/nginx/access.log | awk '{print $7}' | sort | uniq -c | sort -nr | head -n 20

# Identify 5xx Server Errors encountered by crawlers
grep -E "Googlebot|Bingbot|PerplexityBot" /var/log/nginx/access.log | awk '$9 ~ /^5/ {print $7, $9}'

# Measure average server response time (TTFB) for search bots
awk '$9==200 {sum+=$10; count++} END {if (count>0) print "Avg Response Time: " sum/count "ms"}' access.log
```

---

## 4. Universal Schema.org JSON-LD Microdata Snippet

```html
<!-- Generic WebPage + Organization JSON-LD Template -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://seoer.ai/#organization",
      "name": "seoer.ai",
      "url": "https://seoer.ai",
      "logo": "https://seoer.ai/logo.png"
    },
    {
      "@type": "WebSite",
      "@id": "https://seoer.ai/#website",
      "url": "https://seoer.ai",
      "name": "seoer.ai",
      "publisher": { "@id": "https://seoer.ai/#organization" }
    }
  ]
}
</script>
```

---

## 5. Summary

This developer cheatsheet streamlines technical SEO execution. Keep these cURL commands, Lighthouse CLI scripts, and server log one-liners close at hand during development and production deployments.
