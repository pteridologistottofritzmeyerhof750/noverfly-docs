# CMS & Database

NoverFly includes a built-in **Content Management System (CMS)** that lets you create, organize, and manage dynamic content without code.

---

## Overview

The CMS allows you to:
- Define **collections** (content types) with custom fields
- Add, edit, and delete **entries** (items)
- Bind CMS data to **GlowDesign components** for dynamic rendering
- Filter, sort, and paginate data on the frontend
- Access content via the **REST API**

---

## Collections

A collection is a structured content type — similar to a database table.

### Creating a Collection

1. Navigate to **CMS** in your site dashboard
2. Click **+ New Collection**
3. Enter a name (e.g., "Blog Posts", "Team Members", "FAQs")
4. Define your fields
5. Click **Create**

### Example: Blog Posts Collection

| Field Name | Type | Required |
|-----------|------|----------|
| title | Text | ✅ |
| slug | Text (unique) | ✅ |
| body | Rich Text | ✅ |
| excerpt | Text | ❌ |
| coverImage | Image | ❌ |
| author | Reference → Team Members | ❌ |
| publishDate | Date | ✅ |
| tags | Multi-select | ❌ |
| published | Boolean | ✅ |

---

## Field Types

| Type | Description | Example |
|------|-------------|---------|
| **Text** | Single-line string | Title, name, slug |
| **Long Text** | Multi-line text | Description, excerpt |
| **Rich Text** | WYSIWYG HTML content | Article body |
| **Number** | Integer or decimal | Price, quantity, order |
| **Boolean** | True/false toggle | Published, featured |
| **Date** | Date with optional time | Publish date, event date |
| **Image** | File upload (image) | Cover, avatar, thumbnail |
| **File** | File upload (any type) | PDF, ZIP, document |
| **Select** | Dropdown single choice | Category, status |
| **Multi-Select** | Multiple choice tags | Tags, features |
| **Color** | Color picker | Brand color, accent |
| **URL** | Validated URL | External link, social |
| **Email** | Validated email | Contact email |
| **Reference** | Link to another collection | Author → Team Members |
| **JSON** | Raw JSON data | Custom metadata |

---

## Managing Entries

### Adding Entries

1. Open a collection
2. Click **+ New Entry**
3. Fill in the fields
4. Click **Save** (draft) or **Publish** (live)

### Entry States

| State | Visible on site? | Description |
|-------|-----------------|-------------|
| **Draft** | ❌ | Work in progress, not published |
| **Published** | ✅ | Live on your site |
| **Archived** | ❌ | Hidden but preserved |

### Bulk Operations

- Select multiple entries with checkboxes
- **Publish**, **Unpublish**, **Archive**, or **Delete** in bulk
- **Export** to CSV or JSON

---

## CMS in GlowDesign

### Collection List Component

Display a list of CMS entries on your page:

1. In GlowDesign, add a **Collection List** component
2. Bind it to a collection (e.g., "Blog Posts")
3. Configure filters and sorting
4. Design the **item template** — this repeats for each entry
5. Bind dynamic fields to text, images, links

### Dynamic Bindings

Inside a Collection List item, bind element properties to CMS fields:

- **Text content** → `{{title}}`, `{{excerpt}}`
- **Image source** → `{{coverImage}}`
- **Link URL** → `/blog/{{slug}}`
- **Visibility** → Show/hide based on `{{featured}}`

### Filtering & Sorting

Configure in the Collection List settings:

**Filters:**
- `published = true`
- `tags contains "tutorial"`
- `publishDate < today`

**Sort:**
- `publishDate` descending (newest first)
- `title` ascending (A-Z)

**Limit:**
- Show first 10, 25, 50, or all entries

---

## CMS API

Access your CMS data programmatically.

### List Entries

```http
GET /v1/cms/:collectionSlug/entries
Authorization: Bearer YOUR_TOKEN
X-Tenant-Id: YOUR_TENANT_ID
```

**Query Parameters:**
| Param | Type | Description |
|-------|------|-------------|
| `page` | number | Page number (default: 1) |
| `limit` | number | Items per page (default: 25, max: 100) |
| `sort` | string | Field and direction (e.g., `-publishDate`) |
| `filter[field]` | string | Filter by field value |
| `status` | string | Filter by status: `draft`, `published`, `archived` |

**Example:**
```http
GET /v1/cms/blog-posts/entries?status=published&sort=-publishDate&limit=10
```

**Response:**
```json
{
  "data": [
    {
      "id": "entry_abc123",
      "fields": {
        "title": "Getting Started with NoverFly",
        "slug": "getting-started",
        "body": "<p>Welcome to NoverFly...</p>",
        "coverImage": "https://storage.noverfly.com/...",
        "publishDate": "2025-01-15T10:00:00Z",
        "published": true
      },
      "createdAt": "2025-01-14T08:00:00Z",
      "updatedAt": "2025-01-15T09:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 42,
    "totalPages": 5
  }
}
```

### Get Single Entry

```http
GET /v1/cms/:collectionSlug/entries/:entryId
```

### Create Entry

```http
POST /v1/cms/:collectionSlug/entries
Content-Type: application/json

{
  "fields": {
    "title": "My New Post",
    "slug": "my-new-post",
    "body": "<p>Hello world</p>",
    "published": false
  }
}
```

### Update Entry

```http
PATCH /v1/cms/:collectionSlug/entries/:entryId
Content-Type: application/json

{
  "fields": {
    "title": "Updated Title"
  }
}
```

### Delete Entry

```http
DELETE /v1/cms/:collectionSlug/entries/:entryId
```

---

## Webhooks

Trigger external services when CMS data changes:

1. Go to **Settings > Webhooks**
2. Click **+ New Webhook**
3. Configure:
   - **URL** — Your endpoint
   - **Events** — `entry.created`, `entry.updated`, `entry.deleted`, `entry.published`
   - **Collections** — All or specific collections

**Webhook Payload:**
```json
{
  "event": "entry.published",
  "collection": "blog-posts",
  "entry": {
    "id": "entry_abc123",
    "fields": { ... }
  },
  "timestamp": "2025-01-15T10:00:00Z"
}
```

---

## Quotas

| Plan | Collections | Entries per Collection | Storage |
|------|-------------|----------------------|---------|
| Free | 3 | 100 | 500 MB |
| Pro | 20 | 5,000 | 10 GB |
| Business | Unlimited | 50,000 | 50 GB |
| Enterprise | Unlimited | Unlimited | Custom |

---

## Next Steps

- [E-Commerce](ecommerce.md) — Sell products with CMS
- [API Reference](api.md) — Full API documentation
- [GlowDesign](glowdesign.md) — Bind CMS data visually
