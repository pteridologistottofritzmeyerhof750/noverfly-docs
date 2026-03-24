# Authentication

NoverFly uses a secure authentication system based on **JWT tokens** with support for multiple providers and multi-tenant access control.

---

## Authentication Methods

### Email + Password

Standard registration with email verification.

**Register:**
```http
POST /v1/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecureP@ss123",
  "name": "John Doe"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_abc123",
      "email": "user@example.com",
      "name": "John Doe"
    },
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

### Google OAuth 2.0

One-click sign-in with Google. No password required.

**Initiate OAuth:**
```http
GET /v1/auth/google
```

This redirects the user to Google's consent screen. After authorization, the user is redirected back to NoverFly with an authorization code.

**Callback:**
```http
GET /v1/auth/google/callback?code=AUTH_CODE
```

Returns the same JWT token structure as email registration.

---

## JWT Tokens

NoverFly issues two types of tokens:

| Token | Purpose | Expiry |
|-------|---------|--------|
| **Access Token** | API authentication | 15 minutes |
| **Refresh Token** | Obtain new access tokens | 7 days |

### Using Access Tokens

Include the access token in every API request:

```http
GET /v1/sites
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### Refreshing Tokens

When the access token expires, use the refresh token:

```http
POST /v1/auth/refresh
Content-Type: application/json

{
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

**Response:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

> **Note:** Refresh tokens are rotated on each use. The old refresh token is invalidated.

---

## Login

```http
POST /v1/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecureP@ss123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_abc123",
      "email": "user@example.com",
      "name": "John Doe",
      "avatar": "https://storage.noverfly.com/avatars/usr_abc123.jpg"
    },
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

---

## Logout

```http
POST /v1/auth/logout
Authorization: Bearer YOUR_ACCESS_TOKEN
```

This invalidates the current session and refresh token.

---

## Password Reset

### Request Reset Email
```http
POST /v1/auth/forgot-password
Content-Type: application/json

{
  "email": "user@example.com"
}
```

### Reset Password
```http
POST /v1/auth/reset-password
Content-Type: application/json

{
  "token": "RESET_TOKEN_FROM_EMAIL",
  "password": "NewSecureP@ss456"
}
```

---

## User Profile

### Get Current User
```http
GET /v1/auth/me
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Update Profile
```http
PATCH /v1/auth/me
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "name": "Jane Doe",
  "avatar": "https://..."
}
```

---

## Multi-Tenant Access

After authentication, users can access resources within their organizations (tenants).

### List Organizations
```http
GET /v1/organizations
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Switch Tenant Context

Include the `X-Tenant-Id` header in requests to specify which organization's data you want to access:

```http
GET /v1/sites
Authorization: Bearer YOUR_ACCESS_TOKEN
X-Tenant-Id: org_xyz789
```

All data-fetching endpoints are tenant-scoped. Without `X-Tenant-Id`, requests default to the user's personal organization.

---

## Roles & Permissions

| Role | Permissions |
|------|------------|
| **Owner** | Full access, billing, delete organization |
| **Admin** | Manage members, all site operations |
| **Editor** | Create/edit sites, manage content |
| **Viewer** | Read-only access to sites and data |

### Invite a Member
```http
POST /v1/organizations/:orgId/members
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "email": "teammate@example.com",
  "role": "editor"
}
```

---

## API Keys (API Key + Secret Key)

For programmatic access (CI/CD, integrations, server-to-server), generate an **API Key + Secret Key pair** from your account dashboard.

### Generate Keys

Go to **Account Dashboard → Settings → API Keys** and click **"Create API Key"**.

You will receive:

| Key | Format | Purpose |
|-----|--------|--------|
| **API Key** (public) | `nf_pk_live_abc123...` | Identifies your account |
| **Secret Key** (private) | `nf_sk_live_xyz789...` | Authenticates requests |

> ⚠️ **The Secret Key is shown only once.** Save it securely. If lost, revoke and create a new pair.

### Using API Key + Secret Key

Send both keys as headers:

```http
GET /v1/sites
X-API-Key: nf_pk_live_abc123...
X-API-Secret: nf_sk_live_xyz789...
X-Tenant-Id: org_xyz789
```

**cURL example:**
```bash
curl https://api.noverfly.com/v1/sites \
  -H "X-API-Key: nf_pk_live_abc123..." \
  -H "X-API-Secret: nf_sk_live_xyz789..." \
  -H "X-Tenant-Id: org_xyz789"
```

### API Key + Secret also works on GFK Storage API

```bash
curl -X POST https://gfk.noverfly.com/assets/upload \
  -H "X-API-Key: nf_pk_live_abc123..." \
  -H "X-API-Secret: nf_sk_live_xyz789..." \
  -H "X-Tenant-Id: org_xyz789" \
  -F "file=@photo.jpg"
```

### Scoped API Keys

You can also create API keys with specific permission scopes:

```http
POST /v1/api-keys
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "name": "My Integration",
  "scopes": ["sites:read", "sites:write", "cms:read"]
}
```

**Response:**
```json
{
  "apiKey": "nf_pk_live_abc123...",
  "secretKey": "nf_sk_live_xyz789...",
  "name": "My Integration",
  "scopes": ["sites:read", "sites:write", "cms:read"],
  "createdAt": "2025-01-15T10:00:00Z"
}
```

> ⚠️ The `secretKey` is returned **only once** in this response. Store it immediately.

Use the API Key + Secret Key as headers:
```http
GET /v1/sites
X-API-Key: nf_pk_live_abc123...
X-API-Secret: nf_sk_live_xyz789...
X-Tenant-Id: org_xyz789
```

Or you can use just the API Key as Bearer token (for backward compatibility):
```http
GET /v1/sites
Authorization: Bearer nf_pk_live_abc123...
X-Tenant-Id: org_xyz789
```

---

## Security Best Practices

- **Never expose tokens in client-side code** — Store in HttpOnly cookies
- **Use HTTPS only** — All API calls must use `https://`
- **Rotate API keys regularly** — Revoke unused keys
- **Use the minimum scopes needed** — Principle of least privilege
- **Enable Google OAuth** — Stronger than email/password alone

---

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 401 | `UNAUTHORIZED` | Missing or invalid token |
| 403 | `FORBIDDEN` | Insufficient permissions for this resource |
| 422 | `INVALID_CREDENTIALS` | Wrong email or password |
| 429 | `RATE_LIMITED` | Too many authentication attempts |

---

## Next Steps

- [API Reference](api.md) — Full endpoint documentation
- [Security](security.md) — Data protection and compliance
