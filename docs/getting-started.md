# Getting Started

This guide walks you through creating your first website on NoverFly — from signing up to publishing your site.

---

## 1. Create Your Account

Visit [noverfly.com](https://noverfly.com) and click **Sign Up**.

You can register with:
- **Email + Password** — Standard registration with email verification
- **Google** — One-click OAuth sign-in

After registration, you'll be guided through a short onboarding flow:
1. Choose your display name
2. Select your use case (personal, business, agency, developer)
3. Pick your first template or start blank

---

## 2. Create an Organization

Organizations (tenants) are the top-level container for your projects.

- Every user starts with a **personal organization** created automatically
- You can create additional organizations for teams or clients
- Each organization has its own members, sites, storage, and billing

### From the Dashboard:
1. Click the organization switcher (top-left)
2. Click **+ New Organization**
3. Enter a name and optional logo
4. Invite team members (optional)

---

## 3. Create Your First Site

1. From the dashboard, click **+ New Site**
2. Choose a starting point:
   - **Blank** — Empty canvas
   - **Template** — Pre-designed layouts (business, portfolio, blog, store)
   - **Import** — Coming soon
3. Enter your site name
4. Click **Create**

Your site is created instantly with:
- A unique URL: `https://yoursite.noverfly.com`
- A default home page
- The GlowDesign editor ready to use

---

## 4. Design with GlowDesign

Open the GlowDesign editor by clicking **Edit** on your site.

### Interface Overview

```
┌──────────────────────────────────────────────────┐
│  Toolbar (save, undo, redo, preview, publish)    │
├──────────┬──────────────────────┬────────────────┤
│          │                      │                │
│  Panel   │     Canvas           │  Properties    │
│  (left)  │     (center)         │  (right)       │
│          │                      │                │
│  • Pages │  [Your website       │  • Styles      │
│  • Layers│   preview renders    │  • Settings    │
│  • Comps │   here]              │  • Animations  │
│  • Assets│                      │  • Events      │
│          │                      │                │
└──────────┴──────────────────────┴────────────────┘
```

### Adding Components

1. Open the **Components** panel (left sidebar)
2. Drag a component onto the canvas:
   - **Layout** — Container, Grid, Flexbox, Section
   - **Content** — Heading, Paragraph, Image, Video, Icon
   - **Forms** — Input, Button, Checkbox, Select, Form
   - **Interactive** — Accordion, Tabs, Modal, Slider
   - **E-commerce** — Product Card, Cart, Checkout
3. Customize in the **Properties** panel (right sidebar)

### Responsive Design

Switch between breakpoints at the top of the canvas:
- 🖥️ Desktop (1280px+)
- 💻 Tablet (768px)
- 📱 Mobile (375px)

Styles cascade down: desktop styles apply to all, tablet overrides for ≤768px, mobile overrides for ≤375px.

---

## 5. Add Content with CMS

1. Open your site's **CMS** section from the dashboard
2. Create a **Collection** (e.g., "Blog Posts")
3. Define fields: title (text), body (rich text), image (file), date (date)
4. Add entries
5. In GlowDesign, bind a **Collection List** component to your collection
6. Content renders dynamically on your site

---

## 6. Set Up E-Commerce (Optional)

1. Go to **Settings > E-Commerce**
2. Connect your payment provider (Stripe or PayPal)
3. Add products in the **Products** section
4. Add Cart and Checkout components in GlowDesign
5. Configure shipping and tax rules

---

## 7. Publish Your Site

1. Click the **Publish** button (top-right in GlowDesign)
2. Your site goes live at `https://yoursite.noverfly.com`
3. Optional: connect a custom domain in **Settings > Domains**

### Custom Domain Setup

1. Go to **Settings > Domains**
2. Click **Add Domain**
3. Enter your domain (e.g., `www.mysite.com`)
4. Add the DNS records shown:
   - `CNAME www → proxy.noverfly.com`
   - `A @ → [provided IP]`
5. Wait for DNS propagation (usually 5–30 minutes)
6. SSL is provisioned automatically

---

## Next Steps

- [GlowDesign Editor](glowdesign.md) — Master the visual editor
- [CMS & Database](cms.md) — Build dynamic content
- [E-Commerce](ecommerce.md) — Start selling online
- [API Reference](api.md) — Integrate with your tools
