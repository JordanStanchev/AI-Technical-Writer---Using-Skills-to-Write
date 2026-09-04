# Passkey API Endpoints

**Author:** Platform Security Engineering
**Date:** 2026-08-22
**Status:** Draft — API is finalised, docs need TW formatting

---

All passkey API endpoints require a valid session JWT in the `Authorization: Bearer <token>` header unless noted otherwise.

---

## Enrollment

### POST /api/v1/passkeys/register/begin

Initiates the WebAuthn registration ceremony. Returns `PublicKeyCredentialCreationOptions`.

Request body: none required (user is identified from session JWT)

Response 200:
```json
{
  "challenge": "<base64url>",
  "rp": { "name": "SecurePath", "id": "app.securepath.io" },
  "user": { "id": "<base64url>", "name": "user@example.com", "displayName": "User Name" },
  "pubKeyCredParams": [
    { "alg": -7, "type": "public-key" },
    { "alg": -257, "type": "public-key" }
  ],
  "timeout": 60000,
  "authenticatorSelection": {
    "userVerification": "required",
    "residentKey": "preferred"
  },
  "attestation": "none"
}
```

Errors: `401 UNAUTHORIZED`, `429 RATE_LIMITED`

---

### POST /api/v1/passkeys/register/complete

Completes the registration ceremony. The client sends the `PublicKeyCredential` returned by `navigator.credentials.create()`.

Request body: `PublicKeyCredential` object (JSON-encoded attestation response)

Response 200:
```json
{
  "credentialId": "<base64url>",
  "createdAt": "2026-08-22T14:30:00Z",
  "message": "Passkey enrolled successfully"
}
```

Errors: `400 WEBAUTHN_CHALLENGE_EXPIRED`, `400 WEBAUTHN_ORIGIN_MISMATCH`, `400 WEBAUTHN_USER_VERIFICATION_FAILED`, `409 WEBAUTHN_DUPLICATE_CREDENTIAL`, `422 PASSKEY_LIMIT_REACHED`

---

## Authentication

### POST /api/v2/auth/webauthn/challenge

Called after successful password validation. Returns `PublicKeyCredentialRequestOptions` if the user has passkeys enrolled.

Request body: none (user identified from partial session)

Response 200:
```json
{
  "challenge": "<base64url>",
  "rpId": "app.securepath.io",
  "allowCredentials": [
    { "id": "<base64url>", "type": "public-key" }
  ],
  "userVerification": "required",
  "timeout": 60000
}
```

Response 204: user has no passkeys enrolled (TOTP challenge will be issued instead)

---

### POST /api/v2/auth/webauthn/verify

Verifies the WebAuthn assertion and issues a session JWT on success.

Request body: `PublicKeyCredential` (assertion response from `navigator.credentials.get()`)

Response 200:
```json
{
  "sessionToken": "<JWT>",
  "refreshToken": "<JWT>",
  "expiresAt": "2026-08-23T14:30:00Z"
}
```

Errors: `400 WEBAUTHN_CHALLENGE_EXPIRED`, `401 WEBAUTHN_SIGNATURE_INVALID`, `401 WEBAUTHN_CREDENTIAL_NOT_FOUND`, `401 WEBAUTHN_USER_VERIFICATION_REQUIRED`

---

## Management

### GET /api/v1/passkeys

Returns a list of all passkeys enrolled for the authenticated user.

Response 200:
```json
{
  "passkeys": [
    {
      "credentialId": "<base64url>",
      "friendlyName": "Work MacBook",
      "createdAt": "2026-08-10T09:00:00Z",
      "lastUsedAt": "2026-08-22T14:30:00Z",
      "deviceType": "platform"
    }
  ]
}
```

---

### PATCH /api/v1/passkeys/{credentialId}

Updates the friendly name of a passkey.

Request body: `{ "friendlyName": "string" }` (max 50 characters)

Response 200: updated passkey object

---

### DELETE /api/v1/passkeys/{credentialId}

Revokes a passkey. The credential is flagged as REVOKED in user-db. Future authentication attempts with this credential ID are rejected.

Response 204: no content

Errors: `404 PASSKEY_NOT_FOUND`, `403 FORBIDDEN` (if attempting to delete another user's credential)

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
