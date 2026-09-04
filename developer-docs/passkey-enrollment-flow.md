# Passkey Enrollment Flow

**Author:** Platform Security Engineering
**Date:** 2026-08-18
**Status:** Draft

---

## Overview

The enrollment flow registers a new WebAuthn credential (passkey) for an authenticated user. The flow follows the WebAuthn Level 2 registration ceremony as defined by the W3C spec. The user must be signed in with a valid session JWT before enrollment can begin.

---

## Steps

The user should navigate to Settings > Security > Two-Factor Authentication > Add Passkey.

The `enrollment-service` is called by the frontend to generate a `PublicKeyCredentialCreationOptions` object. This includes:
- `rp` (relying party): `{name: "SecurePath", id: "app.securepath.io"}`
- `user`: user ID (opaque byte array), display name, name
- `challenge`: 32 random bytes generated via CSPRNG
- `pubKeyCredParams`: `[{alg: -7, type: "public-key"}, {alg: -257, type: "public-key"}]` (ES256 preferred, RS256 fallback)
- `timeout`: 60000 ms
- `authenticatorSelection`: `{userVerification: "required", residentKey: "preferred"}`
- `attestation`: "none" (consumer) / "direct" (enterprise)

The challenge is stored server-side with a TTL of 5 minutes, keyed to the user session.

The browser's `navigator.credentials.create()` is called with the options. The user must complete a UV gesture (biometric or device PIN). The browser returns a `PublicKeyCredential` containing the attestation object and client data JSON.

The frontend sends the credential to `enrollment-service` at `POST /api/v1/passkeys/register`. The `webauthn-proxy` validates:
1. Client data JSON (origin, challenge match)
2. Attestation object (for "direct" attestation, the authenticator certificate is verified)
3. Public key extracted from the credential and stored in `user-db`

On successful validation, the credential record is inserted into `user-db` with status `ACTIVE`. The credential ID, public key (COSE format), sign count (starting at 0), and metadata (device name if available via UA parsing, creation timestamp) are persisted.

The user should see a success confirmation. They can optionally assign a friendly name to the passkey (e.g., "Work MacBook", "YubiKey 5C").

An enrollment confirmation email is sent by `notification-service`.

---

## Failure states

- `WEBAUTHN_CHALLENGE_EXPIRED` — the user took more than 5 minutes to complete the gesture
- `WEBAUTHN_ORIGIN_MISMATCH` — the credential was created on a different origin (should not occur in normal use)
- `WEBAUTHN_USER_VERIFICATION_FAILED` — the authenticator reported UV flag not set; enrollment is rejected
- `PASSKEY_LIMIT_REACHED` — the user already has the maximum number of passkeys enrolled (default 5)
- `WEBAUTHN_DUPLICATE_CREDENTIAL` — a credential with the same credential ID already exists in user-db

---

## Sign count handling

The WebAuthn spec defines a sign counter to detect cloned authenticators. SecurePath records the sign count on each authentication and checks that it is greater than the stored value. If the received sign count is less than or equal to the stored value, `WEBAUTHN_SIGN_COUNT_ANOMALY` is logged and the authentication attempt is flagged for review (not outright rejected in v4.2, to avoid locking out users with synced passkeys like iCloud Keychain, which may reset the count).

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
