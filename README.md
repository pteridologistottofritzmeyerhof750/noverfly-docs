# 🚀 NoverFly Documentation

**Build, sell, and manage your digital business — all in one cloud.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Website](https://img.shields.io/badge/Website-noverfly.com-brightgreen)](https://noverfly.com)
[![API Status](https://img.shields.io/badge/API-api.noverfly.com-orange)](https://api.noverfly.com)

---

## 🌍 What is NoverFly?

NoverFly is an all-in-one SaaS platform that allows you to:

- 🎨 **Design websites** with GlowDesign Lite — a powerful drag-and-drop visual editor
- 🧩 **Manage data and CMS** — dynamic content, collections, and structured data
- 🛒 **Sell products online** — full e-commerce with cart, checkout, and payments
- 🤖 **Use AI tools** — AI-powered content generation, design assistance, and more
- 🌐 **Deploy instantly** — one-click publish to production with custom domains
- 📊 **Analytics & Insights** — track visitors, conversions, and performance
- 👥 **Multi-tenant organizations** — collaborate with teams across multiple projects

---

## ⚡ Quick Start

1. **Create an account** → [noverfly.com](https://noverfly.com)
2. **Create your first site** → Choose a template or start blank
3. **Open GlowDesign editor** → Visual drag-and-drop builder
4. **Design your pages** → Components, styles, responsive layouts
5. **Click Publish** → Your site is live on the internet

---

## 📚 Documentation

| Topic | Description |
|-------|-------------|
| [Introduction](docs/introduction.md) | Overview of the NoverFly platform |
| [Getting Started](docs/getting-started.md) | Create your first project step by step |
| [Authentication](docs/authentication.md) | JWT, OAuth, sessions, and security |
| [GlowDesign Editor](docs/glowdesign.md) | Visual editor, components, and design system |
| [CMS & Database](docs/cms.md) | Dynamic content, collections, and data management |
| [E-commerce](docs/ecommerce.md) | Products, orders, checkout, and payments |
| [API Reference](docs/api.md) | REST API endpoints, rate limits, and usage |
| [Deployment](docs/deployment.md) | Publishing, custom domains, and hosting |
| [Security](docs/security.md) | Data protection, encryption, and compliance |

---

## 🔐 Authentication

NoverFly uses **JWT authentication** and supports:

- Email + password registration
- Google OAuth 2.0
- Session management with refresh tokens
- Multi-tenant organization switching

---

## 💰 Pricing & Plans

| Plan | Price | Quota | Features |
|------|-------|-------|----------|
| **Free** | $0/mo | 1 site, 100 pages, 500 MB storage | GlowDesign Lite, basic CMS |
| **Pro** | $19/mo | 5 sites, unlimited pages, 10 GB storage | Custom domains, e-commerce, AI tools |
| **Business** | $49/mo | 20 sites, unlimited pages, 50 GB storage | Priority support, analytics, team collaboration |
| **Enterprise** | Custom | Unlimited | Dedicated infrastructure, SLA, custom integrations |

> Visit [noverfly.com/pricing](https://noverfly.com/pricing) for detailed plan comparison.

---

## 🏗️ Platform Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Cloudflare CDN                     │
│         (noverfly.com, *.noverfly.com)              │
├───────────────┬─────────────────────────────────────┤
│   Frontend    │           Backend API               │
│  (Next.js)    │      (Node.js / Fastify)            │
│               │                                     │
│  • Marketing  │  • REST API (api.noverfly.com)      │
│  • Dashboard  │  • PostgreSQL + Redis               │
│  • GlowDesign │  • S3 Storage (OVH)                 │
│  • Help Center│  • BullMQ Workers                   │
│  • Dev Portal │  • WebSocket (real-time)             │
└───────────────┴─────────────────────────────────────┘
```

---

## 🧑‍💻 API Overview

Base URL: `https://api.noverfly.com/v1`

```bash
# Authenticate
curl -X POST https://api.noverfly.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "yourpassword"}'

# Get current user
curl https://api.noverfly.com/v1/auth/me \
  -H "Authorization: Bearer YOUR_TOKEN"

# List sites
curl https://api.noverfly.com/v1/sites \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Tenant-Id: YOUR_TENANT_ID"
```

See the full [API Reference](docs/api.md) for all endpoints.

---

## 🔑 Account System

- **Users** — Individual accounts with email/password or Google OAuth
- **Organizations (Tenants)** — Multi-tenant workspaces for team collaboration
- **Roles** — Owner, Admin, Editor, Viewer
- **API Keys** — Programmatic access for integrations
- **Quotas** — Per-plan resource limits (sites, pages, storage, API calls)

---

## 💡 Vision

NoverFly combines the power of **Webflow**, **Shopify**, and **Firebase** into one unified platform — designed for creators, businesses, and developers who want to build, sell, and manage everything from a single dashboard.

---

## 📞 Support

- 🌐 Help Center: [help.noverfly.com](https://help.noverfly.com)
- 📧 Email: support@noverfly.com
- 📖 Documentation: [docs.noverfly.com](https://docs.noverfly.com)
- 🐛 Bug Reports: [GitHub Issues](https://github.com/gloowflix-hash/noverfly-docs/issues)

---

## License

This documentation is licensed under the [MIT License](LICENSE).

> **Note:** This repository contains documentation only.
> The NoverFly platform source code is proprietary and owned by Gloowflix Cloud.
