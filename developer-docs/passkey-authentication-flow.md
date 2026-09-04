# Passkey Authentication Flow

**Author:** Platform Security Engineering
**Date:** 2026-08-18
**Status:** Draft

---

## Overview

After a passkey has been enrolled, the user can use it as the second factor during sign-in. The flow follows the WebAuthn Level 2 authentication ceremony.

---

## Flow description

After the user submits valid username and password credentials, the `auth-service` checks whether the user has one or more passkeys enrolled with status `ACTIVE` in `user-db`. If yes, a WebAuthn challenge is issued instead of a TOTP prompt.

The `auth-service` generates a `PublicKeyCredentialRequestOptions` object:
- `challenge`: 32 fresh random bytes (CSPRNG), stored server-side TTL 5 min
- `timeout`: 60000 ms
- `rpId`: `app.securepath.io`
- `allowCredentials`: list of the user's enrolled credential IDs (allows the browser to select the correct authenticator)
- `userVerification`: `required`

The frontend calls `navigator.credentials.get()` with the options. The user's authenticator prompts for UV (biometric/PIN). On success, the authenticator signs the challenge with the stored private key and returns a `PublicKeyCredential` (assertion).

The frontend sends the assertion to `POST /api/v2/auth/webauthn/verify`. The `webauthn-proxy` performs verification:
1. Verifies the client data JSON (origin matches, challenge matches, type is `webauthn.get`)
2. Verifies the authenticator data (rpIdHash, UP flag, UV flag set)
3. Verifies the assertion signature against the stored public key
4. Updates the sign count in `user-db`

On success, the `auth-service` issues a full session JWT (TTL 8h, refresh token TTL 30d), identical to the session issued after successful TOTP validation.

---

## Fallback behaviour

If the user has passkeys enrolled but their current device does not have access to any of the enrolled authenticators (e.g., using a new device), the frontend should offer a fallback option to use TOTP or a backup code. The fallback is user-initiated — the user clicks "Use a different method". The `auth-service` does not automatically fall back.

---

## Error states

- `WEBAUTHN_CHALLENGE_EXPIRED` — challenge TTL exceeded
- `WEBAUTHN_SIGNATURE_INVALID` — signature verification failed; possible credential tamper or clone
- `WEBAUTHN_CREDENTIAL_NOT_FOUND` — the submitted credential ID does not match any enrolled passkey for this user
- `WEBAUTHN_USER_VERIFICATION_REQUIRED` — UV flag was not set in the authenticator data; authentication rejected
- `WEBAUTHN_SIGN_COUNT_ANOMALY` — sign count did not increment as expected; logged, not blocking in v4.2

---

*FICTIONAL DOCUMENT — © JPDocu Services LTD. Created for educational purposes.*
