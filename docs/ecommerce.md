# E-Commerce

NoverFly includes a complete **e-commerce system** that lets you sell physical and digital products directly from your website.

---

## Overview

- 🛍️ Product catalog with variants and inventory
- 🛒 Shopping cart with real-time updates
- 💳 Checkout with Stripe and PayPal
- 📦 Order management and fulfillment
- 🎫 Discount codes and promotions
- 📊 Sales analytics and reporting
- 🌍 Multi-currency support
- 📧 Automated order emails

---

## Setting Up E-Commerce

### 1. Enable E-Commerce

1. Go to your site's **Settings > E-Commerce**
2. Toggle **Enable E-Commerce**
3. Select your currency (USD, EUR, GBP, etc.)

### 2. Connect Payment Provider

**Stripe:**
1. Click **Connect Stripe**
2. Authorize NoverFly in your Stripe dashboard
3. Stripe is ready — test and live modes supported

**PayPal:**
1. Click **Connect PayPal**
2. Enter your PayPal client ID and secret
3. PayPal checkout is enabled

> You can enable both Stripe and PayPal simultaneously.

---

## Products

### Creating a Product

1. Go to **E-Commerce > Products**
2. Click **+ New Product**
3. Fill in product details:

| Field | Description |
|-------|-------------|
| **Name** | Product title |
| **Slug** | URL-friendly identifier |
| **Description** | Rich text product description |
| **Price** | Base price |
| **Compare At Price** | Original price (for "sale" display) |
| **Images** | Product gallery (up to 10 images) |
| **Category** | Product category |
| **Tags** | Product tags for filtering |
| **SKU** | Stock Keeping Unit |
| **Weight** | For shipping calculation |
| **Inventory** | Stock quantity (0 = out of stock) |
| **Track Inventory** | Auto-decrement on purchase |
| **Digital** | Is this a digital download? |

### Product Variants

For products with options (size, color, material):

1. Click **+ Add Option** (e.g., "Size")
2. Enter values: S, M, L, XL
3. Add another option (e.g., "Color"): Red, Blue, Black
4. Variants are generated automatically (S/Red, S/Blue, M/Red, etc.)
5. Set individual prices and inventory per variant

### Product API

```http
GET /v1/ecommerce/products
Authorization: Bearer YOUR_TOKEN
X-Tenant-Id: YOUR_TENANT_ID
```

**Response:**
```json
{
  "data": [
    {
      "id": "prod_abc123",
      "name": "Classic T-Shirt",
      "slug": "classic-t-shirt",
      "price": 29.99,
      "compareAtPrice": 39.99,
      "currency": "USD",
      "images": ["https://storage.noverfly.com/..."],
      "variants": [
        {
          "id": "var_001",
          "options": { "size": "M", "color": "Blue" },
          "price": 29.99,
          "inventory": 50,
          "sku": "TS-M-BLU"
        }
      ],
      "status": "active"
    }
  ]
}
```

---

## Shopping Cart

The cart is managed client-side with server validation at checkout.

### Cart Components in GlowDesign

- **Add to Cart Button** — Adds a product (with variant selection) to cart
- **Cart Icon** — Shows item count badge in navbar
- **Cart Drawer** — Slide-out panel showing cart contents
- **Cart Page** — Full-page cart view with quantity controls

### Cart API

```http
POST /v1/ecommerce/cart
Content-Type: application/json

{
  "items": [
    {
      "productId": "prod_abc123",
      "variantId": "var_001",
      "quantity": 2
    }
  ]
}
```

---

## Checkout

### Checkout Flow

1. Customer reviews cart → adjusts quantities
2. Customer enters shipping information
3. Shipping cost calculated
4. Customer selects payment method (Stripe / PayPal)
5. Payment processed
6. Order created
7. Confirmation email sent
8. Inventory updated

### Checkout API

```http
POST /v1/ecommerce/checkout
Authorization: Bearer YOUR_TOKEN (optional for guest checkout)
Content-Type: application/json

{
  "items": [
    {
      "productId": "prod_abc123",
      "variantId": "var_001",
      "quantity": 2
    }
  ],
  "shipping": {
    "name": "John Doe",
    "address": "123 Main St",
    "city": "Paris",
    "country": "FR",
    "postalCode": "75001"
  },
  "paymentMethod": "stripe"
}
```

**Response:**
```json
{
  "orderId": "ord_xyz789",
  "checkoutUrl": "https://checkout.stripe.com/...",
  "total": 63.98,
  "currency": "USD"
}
```

---

## Orders

### Order States

| Status | Description |
|--------|-------------|
| **Pending** | Payment initiated but not confirmed |
| **Paid** | Payment successful |
| **Processing** | Being prepared for fulfillment |
| **Shipped** | Sent to customer |
| **Delivered** | Received by customer |
| **Cancelled** | Order cancelled |
| **Refunded** | Payment refunded |

### Order Management

From the NoverFly dashboard:
1. View all orders in **E-Commerce > Orders**
2. Filter by status, date, customer
3. Click an order to view details
4. Update status (Processing → Shipped)
5. Add tracking number
6. Process refunds

### Orders API

```http
GET /v1/ecommerce/orders
Authorization: Bearer YOUR_TOKEN
X-Tenant-Id: YOUR_TENANT_ID
```

```http
GET /v1/ecommerce/orders/:orderId
```

```http
PATCH /v1/ecommerce/orders/:orderId
Content-Type: application/json

{
  "status": "shipped",
  "trackingNumber": "1Z999AA10123456784",
  "trackingUrl": "https://tracking.example.com/..."
}
```

---

## Discount Codes

Create promotional discount codes:

1. Go to **E-Commerce > Discounts**
2. Click **+ New Discount**
3. Configure:

| Setting | Options |
|---------|---------|
| **Code** | e.g., `SUMMER25` |
| **Type** | Percentage or Fixed amount |
| **Value** | e.g., 25% or $10 |
| **Min Order** | Minimum order value to apply |
| **Max Uses** | Total number of times the code can be used |
| **Per Customer** | Limit uses per customer |
| **Valid From/To** | Date range for the promotion |
| **Products** | All products or specific collections |

### Apply Discount

```http
POST /v1/ecommerce/cart/discount
Content-Type: application/json

{
  "code": "SUMMER25"
}
```

---

## Digital Products

Sell downloadable files:

1. Create a product and check **Digital Product**
2. Upload the file (PDF, ZIP, etc.)
3. After purchase, customers receive a secure download link
4. Links expire after a configured number of downloads or time

---

## Sales Analytics

View e-commerce performance from your dashboard:

- **Revenue** — Daily, weekly, monthly revenue
- **Orders** — Order count and average order value
- **Products** — Best selling products
- **Customers** — New vs returning customers
- **Conversion** — Cart-to-purchase conversion rate

---

## E-Commerce Quotas

| Plan | Products | Orders/Month | Transaction Fee |
|------|----------|-------------|----------------|
| Free | 5 | 10 | 5% |
| Pro | 500 | Unlimited | 2% |
| Business | 5,000 | Unlimited | 0% |
| Enterprise | Unlimited | Unlimited | 0% |

> Transaction fees are in addition to Stripe/PayPal processing fees.

---

## Next Steps

- [GlowDesign](glowdesign.md) — Design your store pages
- [CMS](cms.md) — Manage product-related content
- [API Reference](api.md) — E-commerce API endpoints
