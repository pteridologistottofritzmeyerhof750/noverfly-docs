# Deployment

NoverFly handles hosting and deployment for you — publish your site with one click and it's live on the internet.

---

## Publishing Your Site

### One-Click Publish

1. Open your site in GlowDesign
2. Click the **Publish** button (top-right)
3. Your site is built and deployed in seconds
4. Live at `https://yoursite.noverfly.com`

### What Happens During Publish

1. **Build** — Your site pages are compiled and optimized
2. **Assets** — Images and files are uploaded to CDN
3. **Deploy** — Content is distributed globally via Cloudflare
4. **SSL** — HTTPS is automatically configured
5. **DNS** — Your subdomain is active immediately

---

## Hosting

All NoverFly sites are hosted on our cloud infrastructure:

| Feature | Details |
|---------|---------|
| **CDN** | Cloudflare global network (300+ locations) |
| **SSL** | Automatic HTTPS for all sites |
| **Uptime** | 99.9% SLA (Business and Enterprise) |
| **DDoS Protection** | Cloudflare DDoS mitigation |
| **Edge Caching** | Static assets cached at edge nodes |
| **Auto-Scaling** | Handles traffic spikes automatically |
| **Backups** | Daily automated backups |

---

## Custom Domains

Connect your own domain to any NoverFly site.

### Setup Steps

1. Go to **Settings > Domains** in your site dashboard
2. Click **+ Add Domain**
3. Enter your domain (e.g., `www.example.com`)
4. Configure DNS records at your domain registrar:

### DNS Configuration

**For root domain (`example.com`):**
```
Type: A
Name: @
Value: 51.77.147.243
TTL: Auto
```

**For subdomain (`www.example.com`):**
```
Type: CNAME
Name: www
Value: proxy.noverfly.com
TTL: Auto
```

**Using Cloudflare (recommended):**
If you use Cloudflare for DNS, enable the orange cloud (proxy) and set SSL mode to **Full (Strict)**.

### SSL Certificates

- **Free subdomains** (`*.noverfly.com`) — SSL included automatically via Cloudflare Origin certificate
- **Custom domains** — SSL provisioned automatically via Let's Encrypt or Cloudflare Origin
- **Certificate renewal** — Automatic, no manual intervention needed

### Domain Verification

After adding DNS records:
1. NoverFly checks DNS propagation automatically
2. Status changes from "Pending" to "Active"
3. Propagation typically takes 5–30 minutes (up to 48 hours in rare cases)

---

## Environments

### Production
- Default environment
- Accessible to the public
- Cached at edge for maximum performance
- URL: `https://yoursite.noverfly.com` or your custom domain

### Preview
- Generated for every save in GlowDesign
- Private URL for reviewing changes before publishing
- Not indexed by search engines
- URL: `https://preview-{hash}.noverfly.com`

---

## Deploy via API

Trigger deployments programmatically:

```http
POST /v1/sites/:siteId/publish
Authorization: Bearer YOUR_TOKEN
X-Tenant-Id: YOUR_TENANT_ID
```

**Response:**
```json
{
  "success": true,
  "data": {
    "deployId": "deploy_abc123",
    "status": "building",
    "url": "https://yoursite.noverfly.com",
    "createdAt": "2025-01-15T10:00:00Z"
  }
}
```

### Check Deploy Status

```http
GET /v1/sites/:siteId/deploys/:deployId
```

**Response:**
```json
{
  "deployId": "deploy_abc123",
  "status": "live",
  "buildTime": 3200,
  "url": "https://yoursite.noverfly.com",
  "createdAt": "2025-01-15T10:00:00Z",
  "completedAt": "2025-01-15T10:00:03Z"
}
```

### Deploy States

| Status | Description |
|--------|-------------|
| `queued` | Deploy is in the queue |
| `building` | Site is being built |
| `deploying` | Being pushed to CDN |
| `live` | Successfully deployed |
| `failed` | Build or deploy error |

---

## CI/CD Integration

Automate deployments with your CI/CD pipeline using API keys.

### GitHub Actions Example

```yaml
name: Deploy to NoverFly
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Deploy
        run: |
          curl -X POST https://api.noverfly.com/v1/sites/${{ secrets.SITE_ID }}/publish \
            -H "Authorization: Bearer ${{ secrets.NOVERFLY_API_KEY }}" \
            -H "X-Tenant-Id: ${{ secrets.TENANT_ID }}"
```

---

## Performance Optimization

NoverFly automatically optimizes your site:

| Optimization | Description |
|-------------|-------------|
| **Image Optimization** | WebP conversion, lazy loading, responsive sizes |
| **Code Minification** | HTML, CSS, JS minified |
| **Gzip/Brotli** | Compressed responses |
| **Critical CSS** | Inline above-the-fold CSS |
| **Prefetching** | Link prefetching for faster navigation |
| **Font Optimization** | Font-display: swap, subsetting |
| **Cache Headers** | Long-lived cache for static assets |

---

## Bandwidth & Usage

| Plan | Bandwidth/Month | Build Minutes/Month |
|------|----------------|-------------------|
| Free | 1 GB | 100 |
| Pro | 100 GB | 1,000 |
| Business | 1 TB | 10,000 |
| Enterprise | Unlimited | Unlimited |

---

## Rollbacks

If something goes wrong after publishing:

1. Go to **Settings > Deploys**
2. View deploy history
3. Click **Rollback** on any previous deploy
4. Your site reverts to that version in seconds

---

## Next Steps

- [Security](security.md) — Hosting security details
- [API Reference](api.md) — Deploy via API
- [Getting Started](getting-started.md) — Create your first site
