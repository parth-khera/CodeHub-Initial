# Authentication Flow for CodeHub

## Overview
JWT-based authentication with refresh tokens for security and session management. Stateless on server-side, tokens stored client-side.

## Flow Diagram (Mermaid)
```mermaid
sequenceDiagram
    participant U as User
    participant F as Frontend
    participant B as Backend
    participant DB as Database

    U->>F: Enter credentials
    F->>B: POST /auth/login {email, password}
    B->>DB: Verify user
    DB-->>B: User data
    B->>B: Generate JWT & Refresh Token
    B-->>F: {access_token, refresh_token, user}
    F->>F: Store tokens (localStorage/secure cookie)
    F->>B: API request with Authorization: Bearer <access_token>
    B->>B: Validate JWT
    B-->>F: Response

    Note over F: Access token expires
    F->>B: POST /auth/refresh {refresh_token}
    B->>B: Validate refresh token
    B->>B: Generate new access_token
    B-->>F: {access_token}
    F->>F: Update stored token

    U->>F: Logout
    F->>B: POST /auth/logout
    B->>B: Invalidate refresh token (optional, since stateless)
    B-->>F: Success
    F->>F: Clear tokens
```

## Token Details
- **Access Token**: Short-lived (15-30 min), contains user ID, role
- **Refresh Token**: Long-lived (7-30 days), used to get new access tokens
- **Storage**: Secure HTTP-only cookies for production, localStorage for dev
- **Signing**: RSA or HMAC, stored securely

## Security Measures
- Password hashing with bcrypt
- Rate limiting on auth endpoints
- Token blacklisting for logout (optional, use short expiry)
- HTTPS required
- CSRF protection if using cookies

## Role-Based Access
- User roles: user, admin
- Guards check JWT payload for role
- Admin endpoints protected

## Error Handling
- Invalid tokens: 401 Unauthorized
- Expired tokens: 401, client refreshes
- Invalid credentials: 400 Bad Request

## Implementation Notes
- Use passport-jwt in NestJS
- Middleware for token validation
- Refresh endpoint checks refresh token validity

This flow ensures secure, scalable authentication suitable for high-traffic apps.