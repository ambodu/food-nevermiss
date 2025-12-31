# User & Auth Spec

## User data model

```
User
- id: string (UUID)
- email: string | null
- phone: string | null
- nickname: string
- avatar: string | null
- created_at: string (ISO 8601)
```

**Notes**
- Either `email` or `phone` is required.
- `created_at` is server-generated.

## Feature scope

- Register / login
- User profile
- Favorites list
- Browsing history (optional)

## Authentication methods

- Email verification code
- OAuth (WeChat / Google)

## API design

### POST /auth/register

Registers a new user via email/phone or OAuth.

**Request (email/phone)**

```json
{
  "email": "name@example.com",
  "phone": null,
  "verification_code": "123456",
  "nickname": "NeverMiss",
  "avatar": "https://cdn.example.com/avatar.png"
}
```

**Request (OAuth)**

```json
{
  "provider": "wechat",
  "oauth_token": "token",
  "nickname": "NeverMiss",
  "avatar": "https://cdn.example.com/avatar.png"
}
```

**Response**

```json
{
  "token": "jwt-or-session",
  "user": {
    "id": "uuid",
    "email": "name@example.com",
    "phone": null,
    "nickname": "NeverMiss",
    "avatar": "https://cdn.example.com/avatar.png",
    "created_at": "2024-01-01T00:00:00Z"
  }
}
```

### POST /auth/login

Logs in a user with email/phone verification code or OAuth.

**Request (email/phone)**

```json
{
  "email": "name@example.com",
  "phone": null,
  "verification_code": "123456"
}
```

**Request (OAuth)**

```json
{
  "provider": "google",
  "oauth_token": "token"
}
```

**Response**

```json
{
  "token": "jwt-or-session",
  "user": {
    "id": "uuid",
    "email": "name@example.com",
    "phone": null,
    "nickname": "NeverMiss",
    "avatar": "https://cdn.example.com/avatar.png",
    "created_at": "2024-01-01T00:00:00Z"
  }
}
```

### GET /user/me

Returns the current user profile.

**Response**

```json
{
  "id": "uuid",
  "email": "name@example.com",
  "phone": null,
  "nickname": "NeverMiss",
  "avatar": "https://cdn.example.com/avatar.png",
  "created_at": "2024-01-01T00:00:00Z"
}
```

### GET /user/favorites

Returns the current user's favorites list.

**Response**

```json
{
  "items": [
    {
      "id": "fav_1",
      "name": "Spicy Hotpot",
      "created_at": "2024-01-02T00:00:00Z"
    }
  ]
}
```
