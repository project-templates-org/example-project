# Auth database schema

| Table | Columns | Notes |
|---|---|---|
| `users` | `id` (uuid, pk), `email` (citext, unique), `password_hash` (nullable), `created_at` | `password_hash` null for SSO-only users |
| `identities` | `id`, `user_id` → users, `provider` (`google`), `provider_uid`, unique(`provider`,`provider_uid`) | one row per SSO link |
| `sessions` | `id`, `user_id` → users, `expires_at`, `revoked_at` | 30-day rolling sessions |
| `password_resets` | `token_hash`, `user_id`, `expires_at` (now + 30 min), `used_at` | fixes #5: compare `expires_at` in UTC |

Passwords: Argon2id. Tokens are stored hashed (SHA-256), never in plaintext.
